# Código Limpio

Cuando estamos realizando nuestras aplicaciones con Redux lo más habitual es que nos encontremos con un montón de código que se repite una y otra vez por lo que tiene bastante sentido detenernos a estudiar alguna práctica que nos ayude a escribir código más conciso y fácil de mantener, en definitiva, código más limpio.


## Estructura de ficheros y directorios

Suponiendo que el directorio `src` es el directorio raíz del que cuelgan todos los archivos y directorios en los que están estructurada nuestra aplicación lo más habitual es que tengamos los ficheros que contienen los *action creators*, los tipos de acciones definidas y los *reducer* en dicha ubicación. Por lo tanto pensemos que estamos partiendo de una estructura de archivos como la siguiente:

```javascript
src/
  actions.js
  actionTypes.js
  reducer.js
```

Un primer paso que podemos realizar para aclarar la estructura con la que trabajamos es deficir ubicar todos aquellos aspectos que tienen que ver con el trabajo con Redux bajo un mismo directorio al que denominaremos `store`. Por lo tanto nuestra primera optimización sería la siguiente:

```javascript
src/
  store/
    actions.js
    actionTypes.js
    reducer.js
```

Pero ¿qué hay detrás de esta idea? Tenemos que pensar que Redux es la parte de nuestra aplicación que se encargará de gestión del estado por lo que parece una buena idea el separar todo el oigoq ue tiene que ver con esta funcionalidad del código que se encargará de mostrar la interfaz de usuario ya que se trata de dos conceptos (aspectos) completamente diferentes pese a estar íntimamente ligados (la interfaz servirá para definir cómo modificar el estado de la aplicación y el estado servirá para determinar cómo se muestra la interfaz).

Podemos pensar en esta separación de conceptos haciendo una analogía con un supermercado donde cada uno de ellos se corresponde con una sección del mismo. Así, por ejemplo, podemos pensar en la gestión del estado como la sección de frutería mientras que todos los aspectos que están relacionados con la interfaz de usuario podría ser la sección de droguería. Ambas secciones forman parte del mismo supermercado (nuestra aplicación) pero se encargan de la venta de productos (aspectos de nuestra aplicación) totalmente diferentes.

El nombre `store` es totalmente arbitrario y podremos elegir el que queramos aunque la recomendación es que no lo llamemos `redux` ya que si así fuese no estaríamos realizando la separación de conceptos de la que estamos hablando porque no hablaríamos de gestión del estado sino de la separación de conceptos en función de la librería que estamos utilizando, lo cual no se considera una buena práctica. De hecho la regla que se suele seguir para realizar el nombrado de los artefactos que utilizaremos es basándonos en el **rol** que desempearán dentro de la aplicación.

Hasta aquí la cosa es más o menos sencilla porque estamos suponiendo que tenemos un único archivo que contiene todas las acciones de nuestra aplicación pero esto no es lo habitual porque a medida que nuestra aplicación crece nos iremos encontrando con decenas e incluso cientos de acciones que formarán parte de la misma (esto mismo se aplica para el número de *action types* y de *reducers*). Así pues, con el fin de evitar que el tamaño de estos archivos crezca de forma desproporcionada, lo que se suele hacer es crear un nuevo directorio por cada una de las características de nuestra aplicación y situar dentro del mismo los *action creators*, *action types* y *reducers* que están relacionados con dicha característica:

```javascript
src/
  store/
    feature1/
      actions.js
      actionTypes.js
      reducer.js
    feature2/
      actions.js
      actionTypes.js
      reducer.js
```

Con esto lo que estamos logrando es dividir nuestra aplicación en una serie de dominios más pequeños (denominados *bounded context*) de tal manera que cada uno de ellos contendrá sus propios *action creators*, *action types* y *reducers*.

Por ejemplo, como en nuestra aplicación de ejemplo para la gestión de los bugs dentro de proyectos de software vamos a tener que realizar la autenticación de los usuarios podemos definir un subdominio dentro de la misma que se encargue de ello. Dentro del mismo podemos recoger los *action creators* que se encargarán de realizar el login, logout, cambiar la contraseña, etc. Además es fácil deducir que vamos a necesitar otro subdominio para la gestión de los bugs junto con otro subdominio en el que recogeríamos la información de los proyectos con los que estamos trabajando:

```javascript
src/
  store/
    auth/
      actions.js
      actionTypes.js
      reducer.js
    bugs/
    projects/
```

A la larga lo que conseguiremos con ello es que el código de nuestra aplicación sea mucho más mantenible además de que cualquier persona que se incorpore al desarrollo podrá entender de forma mucho más sencilla qué es lo que hace y dónde ir a buscar las cosas.

Cuando estamos trabajando con Redux cada vez que queremos incorporar un nuevo cambio en nuestra aplicación lo más habitual es que tengamos que tocar tres archivos. Más o menos el proceso es el siguiente:

1. Definir la constante que estará asociada a la acción en el fichero que contiene los *action types*
2. Definir un nuevo *action creator* para gestionar la nueva acción.
3. Tendremos que actualizar el *reducer* para que gestione la nueva acción o bien crear uno nuevo.

Estos tres pasos llevan a la definición de un nuevo patrón a la hora de crear los ficheros que contienen el código para la gestión del estado en el que lo que se hace es fusionar estos tres ficheros en un solo evitando de esta manera el tener que navegar por distintos archivos cuando estemos implementando o manteniendo una funcionalidad. En nuestra aplicación de ejemplo esto quedaría como sigue:

```javascript
src/
  store/
    auth.js
    bugs.js
    projects.js
```

Como se pueder ver lo que se hace es sustituir cada uno de los directorios que teníamos inicialmente por un archivo `.js` que contienen el código de los *action creator*, *action types* y *reducers*. Así, si tenemos que realizar algún tipo de modificación en el subdomino que tiene que ver con la autenticación de usuarios simplemente tendremos que trabajar sobre el fichero `auth.js` y lo mismo podemos decir para el caso de las modificaciones en los subdominios de gestión de bugs o de gestión de proyectos.

De hecho este patrón es tan frecuente que se le conoce con el nombre de **Duck Pattern**. Pero ¿por qué este nombre? La razón es porque en inglés la última sílaba de Redux suena de forma muy similar a *duck* (pato). Así, en este contexto, podemos decir que un *duck* es en esencia una agrupación (bundle) de *action creators*, *action types* y *reducers* para un subdominio concreto.

Siguiendo este patrón lo que vamos a conseguir es reducir el número de directorios y ficheros que tenemos que mantener en nuestras aplicaciones lo que hace que sea mucho más fácil de gestionar y de mantener.

## Ducks Pattern

En el punto anterior hemos descrito qué es el **Duck Pattern** por lo que vamos a detenernos unos instantes en ver cómo lo podemos aplicar a nuestra aplicación de ejemplo. La estructura de archivos y directorios de las que partiremos es la siguiente:

```javascript
src/
  functional/
  actions.js
  actionTypes.js
  customStore.js
  index.js
  reducer.js
  store.js
```

El primer paso que tenemos que realizar el crear el directorio `store` donde para recoger dentro del mismo todos aquellos artefactos que tienen que ver la gestión del estado dentro de la aplicación:

```javascript
src/
  functional/
  store/
  [...]
```

El siguiente paso será mover los archivos que tienen que ver con la gestión del estado a este nuevo directorio lo que se traduce en que todos los archivos, excepto `index.js`, irán situados dentro del mismo:

```javascript
src/
  functional/
  store/
    actions.js
    actionTypes.js
    customStore.js
    reducer.js
    store.js
  index.js
```

Vamos ahora a crear un *duck* basándonos en algundo de los subdominios que podemos detectar dentro de nuestra aplicación. Teniendo en cuenta que la aplicación trata de gestionar los bugs que se pueden producir dentro de nuestros proyectos software parece claro que uno de los subdominios con los que trabajaremos será el que agrupa a la gestión de *bugs*. Por ello, dentro del directorio `store` vamos a crear un nuevo archivo al que denominaremos `bugs.js` que es el que contendrá los *action creators*, *action types* y *reducers* relacionados con la gestión de los bugs:

```javascript
src/
  functional/
  store/
    actions.js
    actionTypes.js
    bugs.js
    [...]
```

El siguiente paso consistirá en ir movimiendo de los archivos que teníamos hasta ahora todos aquellos aspectos que tienen que ver con la gestión de los bugs al archivo `bugs.js`. Comenzando con los *action types* nos dirigimos al fichero `actionTypes.js` del proyecto donde tenemos:

```javascript
export const BUG_ADDED = 'bugAdded'
export const BUG_REMOVED = 'bugRemoved'
export const BUG_RESOLVED = 'bugResolved'
```

y simplemente copiamos estas tres contantes en nuestro nuevo fichero `bugs.js` eliminándolas de `actionTypes.js` con el fin d eque no estén duplicadas. Además se considera una buena práctica de programación el añadir un comentario al principio de cada una de las secciones lógicas en las que se puede dividir el *duck* distinguiendo los *action types*, *action creators* y *reducers* que lo conforman. Así en el fichero `bugs.js` tendríamos:

```javascript
// Action types.
export const BUG_ADDED = 'bugAdded'
export const BUG_REMOVED = 'bugRemoved'
export const BUG_RESOLVED = 'bugResolved'
```

Una vez realizada esta operación el fichero `actionTypes.js` pasará a estar vacío por lo que no será necesario mantenerlo así que decidimos borrarlo del proyecto.

Vamos a centrarnos ahora en los *action creators* para lo cual lo que tenemos que hacer es identificar dentro del fichero `actions.js` todos aquellas funciones *action creators* que tienen que ver con la gestión de los bugs dentro de la aplicación. El código de este fichero es el siguiente:

```javascript
import * as actions from './actionTypes'

export const bugAdded = description => ({
  type: actions.BUG_ADDED,
  payload: {
    description
  }
})

export const bugResolved = id => ({
  types: actions.BUG_RESOLVED,
  payload: {
    id
  }
})
```

Vemos que las dos *action creators* que tenemos definidas tienen que ver con la gestión de los bugs por lo que deberemos moverlas al *duck* que estamos creando. Así dentro del fichero `bugs.js` creamos una nueva sección identificándola con un comentario y dentro de la misma situamos el código de estas funciones:

```javascript
// Action types.
[...]

// Action creators.
export const bugAdded = description => ({
  type: BUG_ADDED,
  payload: {
    description
  }
})

export const bugResolved = id => ({
  types: actions.BUG_RESOLVED,
  payload: {
    id
  }
})
```

Además no es necesario hacer uso de las sentencias `import` ya que con la aproximación que estamos siguiendo cuando estamos utilizando el *duck pattern* los *action types* están situados en el mismo archivo que las *action creators* lo que al final se traduce en que nuestro código es mucho más fácil de mantener. En nuestro ejemplo, esto se ha traducido en que tras la realización de la copia del código hemos tenido que eliminar las referencias del tipo `actions.` a la hora de establecer el tipo de acción que se crea con el *action creator*.

Una vez realizado el movimiento de los *action creators* el fichero `actions.js` que teníamos hasta ahora pasará a estar vacío y por lo tanto podremos eliminarlo de nuestro proyecto.

Vamos ahora con los *reducer* que tienen que ver con la gestión de los bugs. Si ahora abrimos el fichero `reducer.js` nos encontramos con el siguiente código:

```javascript
import * as actions from './actionTypes'

let lastId = 0

export default function reducer(state = [], action) {
  switch (action.type) {
    case actions.BUG_ADDED :
      return [
        ...state,
        {
          id: ++lastId,
          description: action.payload.description,
          resolve: false
        }
      ]

    case actions.BUG_REMOVED :
      return state.filter(bug => bug.id !== action.payload.id)

    case actions.BUG_RESOLVED :
      return state.map(bug =>
        bug.id !== action.payload.id ? id : { ...bug, resolve: true }
      )

    default :
      state
  }
}
```

Así lo que hacemos es copiar este *reducer* dentro del código del fichero `bugs.js` (nuestro *duck*) identificando una nueva sección con un comentario:

```javascript
// Action types.
[...]

// Action creators.
[...]

// Reducer
let lastId = 0

export default function reducer(state = [], action) {
  switch (action.type) {
    case BUG_ADDED :
      return [
        ...state,
        {
          id: ++lastId,
          description: action.payload.description,
          resolve: false
        }
      ]

    case BUG_REMOVED :
      return state.filter(bug => bug.id !== action.payload.id)

    case BUG_RESOLVED :
      return state.map(bug =>
        bug.id !== action.payload.id ? id : { ...bug, resolve: true }
      )

    default :
      state
  }
}
```

Al igual que sucedía con los *action creators* a la hora de definir el tipo de acción que estamos consultando en cada una de las sentencias `case` dentro del *reducer* eliminamos la referencia al `import` de los *action types* (la variable `actions`) porque los tipos de acciones con los que podemos trabajar están dentro del propio fichero `bugs.js`. Además, como tras haber realizado esta operación, el fichero `reducer.js` queda vacío lo eliminaremos de nuestro proyecto.

Antes de finalizar con la definición de nuestro *duck* es conveniente realizar una serie de puntualizaciones que hacen referencia al conjunto de reglas que tenemos que aplicar siempre que estemos utilizando el **Ducks Pattern**:

1. Dentro del fichero que construyamos el `export default` tiene que estar asignado al *reducer*.
2. Siempre se tienen que exportar de forma individual (simplemente con `export`) cada una de las funciones *action creators* que tengamos definidas.
3. No será necesario exportar las constantes asociadas a los *action types* ya que únicamente han de ser utilizadas dentro de este módulo.

Con estas últimas puntualizaciones ya podemos mostrar el código final que estará recogido dentro del fichero `bugs.js` que hemos ido construyendo:

```javascript
// Action types.
const BUG_ADDED = 'bugAdded'
const BUG_REMOVED = 'bugRemoved'
const BUG_RESOLVED = 'bugResolved'

// Action creators.
export const bugAdded = description => ({
  type: BUG_ADDED,
  payload: {
    description
  }
})

export const bugResolved = id => ({
  types: actions.BUG_RESOLVED,
  payload: {
    id
  }
})

// Reducer
let lastId = 0

export default function reducer(state = [], action) {
  switch (action.type) {
    case BUG_ADDED :
      return [
        ...state,
        {
          id: ++lastId,
          description: action.payload.description,
          resolve: false
        }
      ]

    case BUG_REMOVED :
      return state.filter(bug => bug.id !== action.payload.id)

    case BUG_RESOLVED :
      return state.map(bug =>
        bug.id !== action.payload.id ? id : { ...bug, resolve: true }
      )

    default :
      state
  }
}
```

El siguiente paso que tenemos que dar es conectar el *reducer* que está recogido dentro del *duck* al *store* de Redux. Para ello nos dirigimos al fichero `store.js` dentro del proyecto el cual contiene el siguiente código:

```javascript
import { createStore } from 'redux'
import { devToolsEnhancer } from 'redux-devtools-extension'
import reducer from './reducer'

const store = createStore(reducer, devToolsEnhancer({ trace: true })

export default store
```

Tras la refactorización que hemos realizado en el código ya no existe el *reducer* que está contenido dentro de `reducer.js` (de hecho ya no existe ese fichero) por lo que lo primero que haremos será eliminar la sentencia `import` que tiene asociada sustituyéndola por la sentencia `import` que nos permitirá importar el *reducer* que está definido dentro del módulo `bugs.js` (como se trata del `default export` no será necesario envolverlo entre llaves):

```javascript
import { createStore } from 'redux'
import { devToolsEnhancer } from 'redux-devtools-extension'
import reducer from './bugs'

const store = createStore(reducer, devToolsEnhancer({ trace: true })

export default store
```

Con todo esto todavía no hemos terminado de refactorizar nuestra aplicación para que funcione con el *duck* que acabamos de crear. Nos queda dirigirnos al fichero `index.js` que está situado en la raíz de nuestro proyecto. El código del mismo es el que mostramos a continuación:

```javascript
import store from '.'
import * as actions from './actions'

store.subscribe(() => {
  console.log('Store changed')
})

// store.dispatch(actions.bugAdded('Bug 1'))
// store.dispatch(actions.bugAdded('Bug 2'))
// store.dispatch(actions.bugAdded('Bug 3'))
// store.dispatch(actions.bugResolved(1))

console.log(store.getState())
```

Lo primero que tenemos que hacer es modificar la sentencia `import` que se encarga de importar dentro del código el *store* de Redux ya que hemos movido el archivo que lo contiene al directorio `store`:

```javascript
import store from './store/store'
[...]
```

El siguiente paso que tendremos que dar consistirá en importar todas las funciones *action creator*
que están recogidas dentro del *duck* que hemos creado porque son las que utilizaremos dentro del archivo por lo que escribiremos lo siguiente:

```javascript
import store from './store/store'
import * as actions from './store/bugs'
[...]
```

Ahora bien ¿se puede considerar una mala práctica de programación el importar todos los objetos que exporta un módulo dentro de nuestro fichero? Es decir, ¿se considera una mala práctica el realizar eun `import * as`? La respuesta es *depende* y siempre tendremos que fijarnos en el número de objetos que son exportardos en el módulo que importamos. En nuestro ejemplo el número de objetos es realmente pequeño (los *action creators* y el *reducer*) por lo que no estaremos penalizando la carga de la aplicación pero si hiciésemos esto mismo con una librería más grande (como podría ser **lodash**) nuestra aplicación incluiría objetos que no estaríamos utilizando y por lo tanto que son totalmente innecesarios.

Si además de las modificaciones anterior quitamos los comentarios de las líneas de código del fichero nos encontraremos con lo siguiente:

```javascript
import store from './store/store'
import * as actions from './actions/bugs'

store.subscribe(() => {
  console.log('Store changed')
})

store.dispatch(actions.bugAdded('Bug 1'))
store.dispatch(actions.bugAdded('Bug 2'))
store.dispatch(actions.bugAdded('Bug 3'))
store.dispatch(actions.bugResolved(1))

console.log(store.getState())
```

Si recargamos el código de la aplicación dentro de nuestro navegador web en la consola de JavaScript dentro de las herramientas para desarrolladores veremos los siguientes mensajes:

```javascript
Store changed!
Store changed!
Store changed!
Store changed!
> (3) [{...}, {...}, {...}]
```

Esto nos viene a indicar que nuestra aplicación seguirá funcionando de forma correcta. No obstante nos queda un pequeño detalle que todavía podemos refinar con el fin de lograr que nuestro código sea mucho más legible y tiene que ver con la siguiente sentencia `import` dentro del fichero `index.js`:

```javascript
import store from './store/store'
[...]
```

¿No parece un poco 'raro' el hacer una importación de `store/store`? La respuesta es que sí por lo que vamso a detenernos unos instantes en solucionarlo. Para ello el primer paso que vamos a dar consistirá en renombrar el fichero `store.js` como `configureStore.js` y dentro del mismo en vez de exportar directamente el `store`:

```javascript
[...]
export default store
```

lo que vamos a hacer es exportar una función que será la encargada de crear el *store* de Redux y retornarlo como resultado de la invocación. Esto que puede parecer extraño es simple de implementar:

```javascript
import { createStore } from 'redux'
import { devToolsEnhancer } from 'redux-devtools-extension'
import reducer from './bugs'

export default function configureStore() {
  const store = createStore(reducer, devToolsEnhancer({ trace: true })
  return store
}
```

Hecho esto nos volvemos a dirigir al fichero `index.js` para realizar la importación de esta función en vez del *store* que como hacíamos anteriormente:

```javascript
import configureStore from './store/configureStore'
[...]
```

Ahora si invocamos a esta función obtendremos como resultado el objeto que representa al *store* de Redux y que podremos utilizar en el código como si lo hubiésemo importado directamente:

```javascript
import configureStore from './store/configureStore'
import * as actions from './actions/bugs'

const store = configureStore()
[...]
```

Acabamos de ver un ejemplo detallado de cómo se ha de implemente el **Duck Pattern** pero tenemos que tener claro que Redux no obliga a que lo sigamos ya que es totalmente agnóstico en lo que respecta a cómo se ha de estructurar nuestras aplicaciones. Ahora bien, tenemos que pensar que este patrón nos va a facilitar tanto la localización como el mantenimiento de nuestras aplicaciones.

## Redux Toolkit

A partir de este punto vamos a ver en profundidad la herramienta [**Redux Toolkit**](https://redux-toolkit.js.org/) ya que es la que el propio equipo que desarrolla Redux nos recomienda para facilitar nuestro trabajo. Para trabajar con ella lo primero que tendremos que hacer es instalarla como parte de nuestro proyecto de desarrollo gracias a `npm`:

```javascript
$ npm install --save @reduxjs/toolkit
```

Con la librería instalada dentro de nuestro proyecto vamos a tener a nuestra disposición una serie de funcione sque nos ayudarán durante el desarrollo de nuestras aplicaciones. En los siguientes puntos vamos a verlas así como a estudiar la forma en la que podemos utilizarlas.

### Creación del store

La primera de las funciones que vamos a ver es la que nos va a permitir crear el *store* de Redux. Volviendo a nuestra aplicación de ejemplo recordememos que es dentro del fichero `configureStore.js` donde estamos creando el *store* para la aplicación:

```javascript
import { createStore } from 'redux'
import { devToolsEnhancer } from 'redux-devtools-extension'
import reducer from './bugs'

export default function configureStore() {
  const store = createStore(reducer, devToolsEnhancer({ trace: true }))
  return store
}
```

Como se puede ver lo que estamos haciendo es importar la función `createStore` de la librería `redux` ya que posteriormente la vamos a invocar para crear el *store*. **Redux Toolkit** nos ofrece su propia función para crear el *store* donde básicamente se está invocando a esta función pero además le añade funcionalidad adicional propocionándonos una serie de herramientas para facilitar nuestro desarrollo siendo una de ellas `redux-devtool-extension` por lo que no tendremos que realizar la importación dentro de nuestro código.

Otra de las venjatas que nos ofrecerá el utilizar la función para crear el *store* de **Redux Toolkit** es que podremos lanzar acciones asíncronas sobre nuestro *store* lo que nos permitirá realizar cosas como la llamada a API externas para obtener la información con la que nutriremos el *store*. En este punto no vamos a preocuparnos en entender cómo funciona la invocación de llamadas asíncronas porque es algo de lo que vamos a ocuparnos en un punto posterior pero lo que sí que tenemos que tener presente es que si no hacemos uso de esta librería tendremos que proporcionar algún tipo de middleware que nos proporcione esta funcionalidad cuando estemos creando nuestro *store*.

En definitiva lo que tenemos que entender es que si utilizamos la función `createStore` de forma directa en nuestra aplicación es nuestra responsabilidad proporcionar todas las opciones de configuración mientras que si utilizamos la función que nos proporciona **Redux Toolkit** nos ahorraremos el tener que realizar todo este trabajo. ¿Y cuál es esta función? `configureStore`. Así, lo primero que tenemos que hacer dentro del código será importarla haciendo innecesaria la importación de las funciones `createStore` y `devToolsEnhancer`:

```javascript
import { configureStore } from '@reduxjs/toolkit'
[...]
```

Si ahora dejamos el código tal cual tendríamos un problema porque si nos fijamos en el nombre que le estamos dando a la función que estamos exportando dentro del fichero es el mismo que el de la función que estamos importando:

```javascript
import { configureStore } from '@reduxjs/toolkit'
[...]

export default function configureStore() {
[...]
```

¿Qué solución podemos adoptar para resolverlo? Pues hay dos aproximaciones: la primera de ella es cambiar el nombre de la función que estamos exportando para que no coincida:

```javascript
[...]
export default function configureAppStore() {
[...]
```

O bien la que suele ser preferida que consiste en exportar una función anónima (*anonymous function*) de la siguiente manera:

```javascript
[...]
export default function() {
[...]
```

Pero ¿qué tendremos que hacer dentro de la función que estamos exportando? Evidentemente lo que perseguimos es crear el *store* para nuestra aplicación por lo que tenemos que invocar a la función `configureStore` la cual recibirá como parámetro un objeto con las opciones de configuración que queramos aplicar siendo cada una de ellas un atributo de este objeto:

|atributo|descripción|
|---|---|
|reducer|Aquí especeficaremos cuál será el *root reducer* para nuestro *store*.
|middleware|Un array con todas las funciones que queremos que actúen como middleware.
|devTools|Booleano para indicar si de forma automática se dará soporte a las DevTools de Redux. Por defecto será `true`.
|preloadedState|El estado inicial que se cargará en el *store*.
|enhancers|Un array con todos los *enhancers* (más adelante veremos que son) que se aplicará sobre el *store*.

De todos estos atributos el único que es obligatorio es `reducer`. Además si se quiere obtener más información al respecto de cada uno de ellos se recomienda [leer la docuementación oficial](https://redux-toolkit.js.org/api/configureStore).

Aplicando lo que acabamos de explicar podemos deducir que el código del fichero `createStore.js` final para nuestra aplicación será el que se muestra a continuación:

```javascript
import { configureStore } from '@reduxjs/toolkit'
import reducer from './bugs'

export default function() {
  return configureStore({ reducer })
}
```

### Action creators

Detengámos unos intantes en pensar en cómo es el proceso que seguimos a la hora de definir las acciones que se pueden llevar a cabo sobre el *store* de nuestra aplicación. En primer lugar siempre creamos una serie de constantes que nos ayuden a identificarlas para luego utilizarlas en las funciones que actúan como *actions creators*. Vamos a ir un paso más alla' deteníendonos en unos instantes para ver cómo es el código que está recogido dentro de estas funciones en nuestra aplicación de ejemplo:

```javascript
// Action creators.
export const bugAdded = description => ({
  type: BUG_ADDED,
  payload: {
    description
  }
})

export const bugResolved = id => ({
  types: actions.BUG_RESOLVED,
  payload: {
    id
  }
})
```

Como se puede ver todas estas funciones están retornando un objeto el cual está siempre formado por dos atributos: `type` (para identificar la acción de la que se trata) y `payload` (donde se recoge toda la información adicional que será necesaria para poder realizar al acción). Y es en este aspecto en donde las funciones que nos ofrece **Redux Toolkit** nos van a ayudar a simplificar el código de nuesta aplicación.

**Redux Toolkit** pone a nuestra disposición del función `createAction` y como tal, si vamos a hacer uso de ella lo primero que tendremos que hacer es importala en nuestro código. Si nos vamos a la [documentación oficial](https://redux-toolkit.js.org/api/createAction) vemos que esta función precisa que le pasemos un parámetro para su correcto funcionamiento y que su tipo sea un string. Pero ¿qué es lo que vamos a obtener tras su invocación? Para verlo vamos a ejecutar el siguiente código JavaScript:

```javascript
import { createAction } from '@reduxjs/toolkit'

const action = createAction('bugUpdated')
console.log(action)
```

Si ejecutamos el código anterior en la consola de JavaScript dentro de las herramientas para desarrolladores veremos un resultado como el siguiente:

```javascript
f bugUpdated
```

es decir, que la invocación de la función `createAction` ha dado como resultado una función denominada `bugUpdated` el cual se corresponde con el string que le hemos pasado como parámetro. ¿Qué sucederá ahora si invocamos esta función? Para ello modificamos ligeramente el código anterior:

```javascript
import { createAction } from '@reduxjs/toolkit'

const action = createAction('bugUpdated')
console.log(action())
```

Si volvemos a dirigirnos a la consola de JavaScript dentro de las herramientas para desarrolladores veremos algo como lo siguiente:

```javascript
{ type: 'bugUpdated', payload: undefined }
```

Lo que estamos viendo es un objeto que representa a una acción (*action object*) donde su tipo (atributo `type`) ha sido establecido al valor `bugUpdated` (recordemos que es el valor del string que hemos pasado como parámetro a la hora de invocar a la función `createAction`) y su `payload` tiene establecido el valor `undefined`. Vamos a modificar nuevamente el código anterior pero en este caso pasándole un argumento cuando estamos invocando a la función retornada por `createAction`:

```javascript
[...]
const action = createAction('bugUpdated')
console.log(action(1))
```

Si ahora nos dirigimos a la consola de JavaScript lo que obtenemos es algo parecido a lo siguiente:

```javascript
{ type: 'bugUpdated', payload: 1 }
```

es decir, que el valor que pasamos como parámetro a la función que retarna la invocación de `actionCreator` se establecerá como el valor del atributo `payload` del *action object*. ¿Qué sucederá si ahora invocamos a la función pasándole como parámetro un objeto?

```javascript
[...]
const action = createAction('bugUpdated')
console.log(action({ id: 1 }))
```

En la consola de JavaScript obtendremos la siguiente información:

```javascript
{ type: 'bugUpdated', payload: { id: 1 }}
```

cosa que, por otra parte, nos podríamos esperar ya que el valor del parámetro con el que hemos invocado la función retornada por `actionCreator` es un objeto y hemos visto que es ese valor el que se le asigna al atributo `payload` del *action object*.

Por lo tanto podemos concluir que la función que retorna la función `createAction` es en esencia una función del tipo *action creator*. Vamos a ver cómo podemos aplicarla dentro de nuestro proyecto y más concretamente en el *duck* encargado de la gestión de los bugs de las aplicaciones. El primer paso que tendremos que dar será, dentro del fichero `bugs.js`, importala para poder ser utilizada e invocarla para obtener el *action creator* para alguna de las acciones que tenemos definidas:

```javascript
import { createAction } from '@reduxjs/toolkit'

const bugUpdated = createAction('bugUpdated')

[...]
```

Además, si recordamos, en el código que teníamos anteriormente dentro de este mismo fichero teníamos definidas dos *action creators* para añadir y resovel los diferntes bugs que van siendo detectados dentro de nuestras aplicación. En concreto el código de estas funciones es el siguiente:

```javascript
[...]
// Action creators.
export const bugAdded = description => ({
  type: BUG_ADDED,
  payload: {
    description
  }
})

export const bugResolved = id => ({
  types: actions.BUG_RESOLVED,
  payload: {
    id
  }
})
```

Gracias a `actionCreator` lo que perseguimos es sustituir todas estas líneas de código por una única línea por cada una de las *action creators* que tengamos que definir lo que hace que nuestro código sea mucho más fácil de mantener y esté mucho más limpio. Pero, además no vamos a necesitar definir cada una de las constantes que establecen los tipos de acciones que se pueden llevar a cabo relacionadas con la gestión de los bugs:

```javascript
[...]
// Action types.
const BUG_ADDED = 'bugAdded'
const BUG_REMOVED = 'bugRemoved'
const BUG_RESOLVED = 'bugResolved'

// Action creators.
export const bugAdded = description => ({
  type: BUG_ADDED,
  [...]
```

Pero, ¿y qué sucederá en los *reducer* donde vamos a precisar saber el tipo de acción que se ha producido para poder determinar la secuencia de operaciones que se han de llevar a cabo sobre el *store*? Esto será también muy sencillo ya que, como sabemos, las funciones en JavaScript son a su vez objetos y como tal podrán tener definidos atributos. En el caso de la función que retorna la invocación de `createAction` el objeto que la representa tiene asociado el atributo `type` que contendrá el valor del string con el que ha sido invocada (el parámetro) y que, como sabemos, representa al tipo de acción que se ha llevado a cabo. Así, si en el ejemplo que estamo siguiente hasta ahora escribimos:

```javascript
[...]
const bugUpdated = createAction('bugUpdated')
console.log(bugUpdated.type)
```

El valor que obtendremos por la consola de JavaSCript será algo como lo siguiente:

```javascript
bugUpdated
```

También podemos obtener el tipo de acción que se crea cuando se invoca a `actionCreator` simplemente invocando al método `toString()` del objeto que representa a la función *action creator*. En otras palabras, el siguiente código producirá el mismo resultado que el que acabamos de ver:

```javascript
[...]
const bugUpdated = createAction('bugUpdated')
console.log(bugUpdated.toString())
```

Con todas estas ideas en mente vamos a ver qué tenemos que hacer para poder refactorizar el código recogido en el fichero `bugs.js`. Como ya hemos dicho eliminaremos las constantes que representan a los tipos de acciones e implementaremos cada uno de los *action creators* para que utilice la función `createAction`:

```javascript
import { createAction } from '@reduxjs/toolkit'

// Action creators.
export const bugAdded = createAction('bugAdded')
export const bugResolved = createAction('bugResolved')
export const bugRemoved = createActioN('bugRemoved')
[...]
```

¿Por qué hemos definido el *action creator* asociado a la acción `bugRemoved`? Pues porque es uno de los tipos de acción que deberá utilizar el *reducer* y que hasta ahora no habíamos implementado dejando de esta manera el código lo más completo posible.

Vamos con el código del *reducer* que tenemos defenido en el archivo ya que ahora no podemos hacer uso de las constantes que representan a los tipos de acciones porque las hemos eliminado sino que deberemos utilizar alguna de las dos técnicas que hemos descrito anteriormente. En nuestro caso vamos a optar por consultar el atributo `type` asociado al objeto que retorna `actionCreator` por lo que escribiremos lo siguiente:

```javascript
// Reducer
let lastId = 0

export default function reducer(state = [], action) {
  switch (action.type) {
    case bugAdded.type :
      [...]

    case bugRemoved.type :
      return state.filter(bug => bug.id !== action.payload.id)

    case bugResolved.type :
      [...]

    default :
      state
  }
}
```

Hechas todas estas refactorizaciones nos dirigiremos a nuestro fichero `index.js` para poder hacer uso de las mismas ya que la forma en la que se invocarán las nuevas *action creator* que acabamos definir diferirán con lo que teníamos hasta ahora. En concreto vamos a fijarnos en el código que dentro de este fichero estamos utilizando para crear las acciones:

```javascript
[...]
store.dispatch(actions.bugAdded('Bug 1'))
store.dispatch(actions.bugAdded('Bug 2'))
store.dispatch(actions.bugAdded('Bug 3'))
store.dispatch(actions.bugResolved(1))
```

Antes de realizar la refactorización de los *action creators* la definición de la función `bugAdded` lo que hacía erá asignar como `payload` del *action object* un objeto con un único atributo denominado `description` siendo su valor el string que se recibe como parámetro (esto es lo que espera l *reducer* que hemos definido para este tipo de acción). Como hemos explicado anteriormente si no realizamos ninguna modificación en el código lo que haremos será crear una serie de *action objects* donde su atributo `payload` tendrá asignado el string `Bug 1`, `Bug 2` y `Bug 3` respectivamente lo cual no es lo que queremos. Entonces ¿qué deberíamos hacer para solventarlo? La respuesta es cambiar valor del parámetro con el que se invoca haciendo que sea un objeto que tiene un único atributo `description` y cuyo valor será el que le corresponderá en cada uno de los casos:

```javascript
[...]
store.dispatch(actions.bugAdded({ description: 'Bug 1' }))
store.dispatch(actions.bugAdded({ description: 'Bug 2' }))
store.dispatch(actions.bugAdded({ description: 'Bug 3' }))
store.dispatch(actions.bugResolved(1))
```

Siguiendo este mismo razonamiento tendremos que invocar a la función `bugResolved` para que reciba como parámetro un objeto formado por un único atributo `id` y cuyo valor será el identificador del bug que será marcado como resuelto ya que así lo espera el *reducer* que se encargará de resolver esta acción sobre el *store*. Por lo tanto el código refactorizado quedará de la siguiente manera:

```javascript
[...]
store.dispatch(actions.bugAdded({ description: 'Bug 1' }))
store.dispatch(actions.bugAdded({ description: 'Bug 2' }))
store.dispatch(actions.bugAdded({ description: 'Bug 3' }))
store.dispatch(actions.bugResolved({ id: 1 }))
```

### Reducers

Tras las últimas modificaciones que hemos llevado a cabo sobre el código del *duck* de nuestra aplicación de ejemplo el aspecto del *reducer* que tenemos implementado es el siguiente:

```javascript
[...]
// Reducer
let lastId = 0

export default function reducer(state = [], action) {
  switch (action.type) {
    case bugAdded.type :
      return [
        ...state,
        {
          id: ++lastId,
          description: action.payload.description,
          resolve: false
        }
      ]

    case bugRemoved.type :
      return state.filter(bug => bug.id !== action.payload.id)

    case bugResolved.type :
      return state.map(bug =>
        bug.id !== action.payload.id ? id : { ...bug, resolve: true }
      )

    default :
      state
  }
}
```

Como vemos el código del *reducer* está formado por una sentencia `switch` donde vamos comprobando el tipo de la acción que se ha recibido en cada una de las sentencias `case` y dentro de las mismas se llevarán a cabo las instrucciones que van a permitir realizar la actualización de la información contenida dentro del *store* retornando el nuevo estado del mismo con una sentencia `return` dentro del propio `case`.

Aunque esta es una práctica muy extendida, existen desarrolladores a los que no les gusta este patrón de actuación (patrón que se conoce como **Inmediately Update Pattern**) y es aquí donde entrará en juego **Redux Tookit** ya que nos permitirá crear *reducer* sin tener que hacer uso de la sentencia `switch` y dentro de la misma podremos escribir código que permitirá actualizar directamente el estado del *store* (es decir, que este *reducer* no tendrá porqué ser una *pure function*). Pero ¿cómo es posible si hemos visto que los *reducer* han de ser *pure functions*? La respuesta es que por dentro **Redux Toolkit** está utilizando **immer** que es una [librería](https://github.com/immerjs/immer) que va permitir realizar la gestión del estado inmutable por nosotros.

Para poder trabajar con esta funcionalidad lo primero que tenemos que hacer es importar la función `createReducer` dentro del código de nuestro *duck* de la siguiente manera:

```javascript
import { createAction, createReducer } from '@reduxjs/toolkit'
```

La función `createReducer` recibe dos parámetros siendo el primero de ellos el estado inicial del *store* el cual podemos deducir de la propia implementación de nuestro *reducer* ya que se trata del primer parámetro con que este último es invocado. Así, como en el código de nuestro *reducer* tenemos:

```javascript
[...]
export default function reducer(state = [], action) {
```

podemos deducir que el estado inicial es un array vacío y así lo indicaremos como el primer parámetro para la invocación de `createReducer`:

```javascript
createReducer([], ...)
```

El segundo parámetro que espera `createReducer` es un objeto en el que tendremos que mapear las acciones a las funciones que queremos que se ejecuten cuando se producen dichas acciones. En otras palabras, este objeto va a tener como atributos las acciones a las que queremos que responda el *reducer* y como el valor asociado a dichas atributos las funciones que recogerán el conjunto de instrucciones que queremos que se lleven a cabo sobre el *store* cuando se produzcan dichas acciones (esto lo hace muy parecido a las funciones *handler event* de JavaScript donde indicamos el evento que se estará produciendo en el navegador y la función que queremos que se ejecute cuando se produzca dicho evento).

Vamos a comenzar a construcir este objeto para nuestro ejemplo. Comenzaremos por la acción `bugAdded` (así es el tipo que tiene asignado) por lo que lo primero que tendremos que definir será un atributo dentro de este objeto con este nombre:

```javascript
createReducer([], {
  bugAdded: ...
})
```

¿Y cómo ha de ser la función que tendrá asignada? Lo primero que tenemos que saber es que esta función recibirá siempre dos parámetros: el estado del *store* y el objeto que representa a la acción lo que se corresponde con los parámetros que recibe una función que desempeña el papel de *reducer* en **Redux**. Así la definición de la misma es como sigue:

```javascript
createReducer([], {
  bugAdded: (state, action) => { ... }
})
```

Como ya hemos comentado anteriormente, dentro de esta función vamos a poder escribir código que pueda mutar el estado del *store*. Lo que tenemos que preguntarnos es ¿qué es lo que sucede cuando se lanza una acción del tipo `bugAdded` dentro de nuestra aplicación? Recordemos el código asociado a la gestión de la misma dentro del *reducer* que estamos refactorizando:

```javascript
[...]
case bugAdded.type :
  return [
    ...state,
    {
      id: ++lastId,
      description: action.payload.description,
      resolve: false
    }
  ]
```

Es decir, que lo que se está haciendo es crear un nuevo objeto que representa al bug que queremos añadir y posteriormente lo añadimos al array que contiene la información de todos los bugs recogidos dentro del *store* retornando un nuevo array con todos los bugs que había hasta ahora más el que acabamos de crear lo que hace que nuestro *reducer* se comporte como una función pura. Ahora bien, en nuestra nueva función no tenemos que garantizar que estemos ante una función pura por lo que simplemente añadimos el objeto que representará al bug al array que representa el *store*, evitándonos tener que utilizar el *spread operator* para garantizar la inmutabilidad:

```javascript
createReducer([], {
  bugAdded: (state, action) => {
    state.push({
      id: ++lastId,
      description: action.payload.description,
      resolve: false
    })
  }
})
```

Como ya hemos dicho **Redux Toolkit** está utilizando internamente **immer** por lo que el código que acabamos de escribir será transformado en una función que seguirá el patrón **Inmediately Update Pattern**. De hecho, **immer** posee una función denominada `produce` que recibe dos parámetros siendo el primero de ellos un objeto que representa el estado inicial y el segundo la función que se encargará de actualizar dicho estado. Ahora bien, en el caso de `produce` la función que recibe como segundo parámetro recibe un único parámetro (denominado `draftState`) que no es más que el objeto sobre el que vamos a escribir nuestro código mutable, como por ejemplo añadirle atributos, etc.

```javascript
produce(initialState, draftState => {
  draftState.x = 1
  [...]
})
```

Vemos que el objeto `draftState` lo que está haciendo es actuar como un *proxie* ya que es sobre el donde realizaremos todos los cambios que serán llevados a cabo en el objeto `initialState`. Sabiendo esto, cuando nosotros estamos definiendo el segundo parámetro de `createReducer` la función que escribamos será la que actuará como el valor de `draftState` lo que va a permitir que **immer** pueda definir por nosotros el patrón **Inmediately Update Pattern**. No profundizaremos más en cómo funciona **immer** porque se escapa del ámbito de este manual.

Continuando con la refactorización de nuestro *reducer* el segundo tipo de acciones que tenemos que gestionar es `bugResolved` por lo que creamos un nuevo atributo dentro del objeto con este nombre y le asignamos como función el código que nos va a permitir encontrar el bug que tiene un determinado identificador para actualizar el valor de su atributo `resolved` como `true`:

```javascript
createReducer([], {
  bugAdded: (state, action) => {
    state.push({
      id: ++lastId,
      description: action.payload.description,
      resolve: false
    })
  },

  bugResolved: (state, action) => {
    const index = state.findIndex(bug => bug.id === action.payload.id)
    state[index].resoved = true
  }
})
```

De hecho, todavía podemos aclarar más el código que estamos creando si en vez de llamar al primer parámetro de las funciones que estamos definiendo como `state` le llamamos como `bugs` nos ayudará a aclarar con qué tipo de información estamos trabajando dentro del *store*.

```javascript
createReducer([], {
  bugAdded: (bugs, action) => {
    bugs.push({
      id: ++lastId,
      description: action.payload.description,
      resolve: false
    })
  },

  bugResolved: (bugs, action) => {
    const index = bugs.findIndex(bug => bug.id === action.payload.id)
    bugs[index].resoved = true
  }
})
```

Otro aspecto a tener en cuenta cuando estamos haciendo uso de la función `createReducer` es que en el objeto que le pasamos como segundo parámetro no tenemos que definir qué es lo que queremos que suceda cuando el tipo de acción no es reconocida por nuestro *reducer*, en otras palabras, qué es lo que tenemos que hacer en la sentencia `default` asociada al reducer. De hecho, es un error bastante habitual que como desarrolladores nos estemos olvidando de este sentencia lo cual se traduciría en que como resultao de la invocación de nuestro *reducer* se retornaría como nuevo valor del estado `undefined` lo que provocaría que eliminásemos toda la información dentro del *store*.

Es importante recordar que el nombre de los atributos que definamos en el objeto que pasamos a la función `createReducer` ha de ser el mismo que el string que le pasamos a la función `createAction` por lo que si en algún momento queremos cambiarlo no deberíamos olvidarnos de cambiar en los dos sitios. ¿Pero esto no parece una buen forma de gestionar nuestro código? Así pues, vamos a dar un paso más adelante de tal manera que no tengamos que preocuparnos de ello para lo cual, en vez de escribir directamente el nombre del atributo, vamos a hacer uso de JavaScript para que determine de forma dinámica cuál será el nombre de dicho atributo gracias al uso de los corchetes.

Con esto en mente y recordando que cuando invocamos a la función `createAction` el objeto que representa a la función que retorna posee el atributo `type` el cual posee el nombre que identifica al tipo de la acción. Así pues refactorizamos nuestro código para que haga uso de esta característica:


```javascript
// Action creators.
export const bugAdded = createAction('bugAdded')

// Reducer
createReducer([], {
  [bugAdded.type]: (bugs, action) => {
    bugs.push({
      id: ++lastId,
      description: action.payload.description,
      resolve: false
    })
  },
  [...]
```

Repitiendo este mismo razonamiento para el caso de la acción que sirve para resolver uno de los bugs que están dentro del *store* el código que nos quedaría sería algo como lo siguiente donde ya simplemente tendremos que indicar que el `export default` del *duck* será la función que retorna la invocación de la función `createReducer` lo que nos deja con lo siguiente:

```javascript
// Reducer
export default createReducer([], {
  [bugAdded.type]: (bugs, action) => {
    bugs.push({
      id: ++lastId,
      description: action.payload.description,
      resolve: false
    })
  },

  [bugResolved.type]: (bugs, action) => {
    const index = bugs.findIndex(bug => bug.id === action.payload.id)
    bugs[index].resoved = true
  }
})
```

### Slices

En los apartados anteriores hemos visto como gracias a **Redux Toolkit** hemos podido definir tanto los *action creators* como los *reducer* que vamos a utilizar en nuestra aplicación pero los creadores de la librería han ido un paso más adelante ya que nos vas a proporcionar la posibilidad de realizar estos dos pasos de una sola vez. Esta funcionalidad está presente en la función `createSlice`.

Como siempre lo primero que tendremos que hacer para poder utilizar esta función es importarla dentro del código de nuestro *duck* de la siguiente manera:

```javascript
import { createAction, createReducer, createSlice } from '@reduxjs/toolkit'
```

Una vez tenemos la función a nuestra disposición lo que tenemos que hacer es invocarla pasándole como parámetro un objeto que contendrá la configuración que queremos que aplique gracias a los atributos que definamos dentro del mismo. El primero de los atributos que tenemos que conocer es `name` en el cual simplemente le damos un nombre al *slice* que estamos creando. Siguiendo con nuestro ejemplo, el *slice* que vamos a utilizar sirve para gestionar la información de los bugs dentro de las aplicaciones por lo que decidimos identificarlo de la siguiente manera:

```javascript
createSlice({
  name: 'bugs'
})
```

El siguiente parámetro que tenemos que conocer es `initialState` donde, como podemos deducir por su nombre, servirá para especificar qué valores están contenidos inicialmente dentro del *store* que, en el ejemplo que estamos siguiendo, será un array vacío porque incialmente en el sistema no está recogida la información de ningún bug:

```javascript
createSlice({
  name: 'bugs',
  initialState: []
})
```

El siguiente atributo es `reducers` que espera a su vez tener asignado como valor un objeto en el que lo que se hace es relacionar los diferentes tipos de acciones que vamos a poder lanzar sobre el *store* definiendo un atributo por cada una de ellas y como valor la función que se encargará de gestionar dicha acción:

```javascript
createSlice({
  name: 'bugs',
  initialState: []
  reducers: {
    // action => action hanlders
  }
})
```

Cada una de estas funciones recibirá dos parámetros: en primer lugar el estado en el que se encuentra el *store* en el momento en el que es invocada y como segundo parámetro el objeto que representa a la acción que se ha lanzado. Es más, como hemos visto en el punto anterior, no estamos obligados a llamar al primer parámetro `state` sino que simplemente la podemos llamar como el tipo de elemento dentro del *store* con el que estamos trabajando. Aplicando esta idea al ejemplo que estamos siguiendo llamaríamos a este primer parámetro `bugs` porque es el tipo de información que tenemos dentro del *store*:

```javascript
createSlice({
  name: 'bugs',
  initialState: []
  reducers: {
    // action => action hanlders
    bugAdded: (bugs, action) => { }
  }
})
```

Ahora tenemos que definir el cuerpo de esta función que en nuestro caso será el mismo que está recogido en el *reducer* encargado de registrar un nuevo bug dentro de nuestra aplicación y que hemos visto en el punto anterior. Esto nos deja lo siguiente:

```javascript
createSlice({
  name: 'bugs',
  initialState: []
  reducers: {
    // action => action hanlders
    bugAdded: (bugs, action) => {
      bugs.push({
        id: ++lastId,
        description: action.payload.description,
        resolve: false
      })
    }
  }
})
```

El siguiente paso que tendremos que dar será continuar añadiendo atributos al objeto asociado al atributo `reducers` con el resto de acciones que se han de gestionar. En nuestro ejemplo tenemos una acción más que es `bugResolved` por lo que definimos lo siguiente:

```javascript
createSlice({
  name: 'bugs',
  initialState: []
  reducers: {
    // action => action hanlders
    bugAdded: (bugs, action) => {
      bugs.push({
        id: ++lastId,
        description: action.payload.description,
        resolve: false
      })
    },

    bugResolved: (bugs, action) => {
      const index = bugs.findIndex(bug => bug.id === action.payload.id)
      bugs[index].resoved = true
    }
  }
})
```

Hecho esto vamos a ver qué es lo que retorna esta función para lo cual simplemente vamos a recoger el resultado de la invocación de la misma en una variable y a escribirla por la consola:

```javascript
const slice = createSlice({ ... })
console.log(slice)
```

Si ahora ejecutamos la aplicación en la consola de JavaScript obtendremos como salida un objeto que contiene la siguiente información:

```javascript
{
  name: 'bugs'
  reducer: f (state, action)
  actions: { 
    bugAdded: f(), 
    bugResolved: f() 
  }
  caseReducers: { 
    bugAdded: (bugs, action) => { ... }, 
    bugResolved: (bugs, action) => { ... }
  }
  __proto__: Object
}
```

Como podemos ver la invocación de la función `createSlice` retorna un objeto que está formado por cuatro atributos:

|atributo|descripción|
|---|---|
|name|Contiene el valor del atributo `name` que hemos definido a la hora de invocar a la función.
|reducer|Se trata de una función que recibe dos parámetros: el estado en el que se encuentra el *store* y la acción que se ha lanzado sobre el mismo. Por lo tanto es un *reducer* normal que ha sido creado sin que nosotros tengamos que hacer nada.
|actions|Es una objeto que contendrá un atributo por cada una de las acciones que se pueden llevar a cabo sobre el *store* asignándole a cada una de ellas una función que actuará como *action creator* a la hora de crear los objetos que representarán a dichas acciones.
|caseReducers|Se trata también de un objteo formado también por atributo con cada una de las acciones que hemos definido pero en este caso tendrá asignado como valor la función con la que hemos invocado a `createSlice`.

Y ahora, ¿qué tenemos que exportar por defecto? En los ejemplos anteriores hemos visto que siempre tiene que ser *reducer* que está asignado a nuestro *duck* por lo que la sentencia que tendríamos escribir sería la siguiente:

```javascript
[...]
export default slice.reducer
```

Pero no solament deberemos exportar el *reducer* ya que debemos ser capaces de crear acciones desde fuera de nuestro *duck* por lo que deberíamos exportar cada una de las funciones que están contenidas en el atributo `actions` del objeto que retorna `createSlice` como *named exports*.

```javascript
export const { bugAdded, bugResolved } = slice.actions
export default slice.reducer
```

Como se puede ver hemos utiliza *object destructuring* para obtener los atributos del objeto que queremos exportar para posteriormente exportarlos.

El código final de nuestro *duck* para la gestión de los bugs que se pueden producir dentro de los errores en nuestras aplicaciones finalizaría conteniendo el siguiente código:

```javascript
import { createSlice } from '@reduxjs/toolkit'

let lastId = 0

createSlice({
  name: 'bugs',
  initialState: []
  reducers: {
    bugAdded: (bugs, action) => {
      bugs.push({
        id: ++lastId,
        description: action.payload.description,
        resolve: false
      })
    },

    bugResolved: (bugs, action) => {
      const index = bugs.findIndex(bug => bug.id === action.payload.id)
      bugs[index].resoved = true
    }
  }
})

export const { bugAdded, bugResolved } = slice.actions
export default slice.reducer
```

Si volvemos a ejecutar el código de nuestra aplicación pero en este caso nos vamos a las **Redux Dev Tools** dentro de nuestro navegador en la columna en la que se muestra el conjunto de acciones que se han lanzado sobre el *store* observaremos lo siguiente:

```javascript
@@INIT
bugs/bugAdded
bugs/bugAdded
bugs/bugAdded
bugs/bugResolved
```

podemos ver que todas las acciones que hemos lanzado están precedidas por `bugs` englobándolas dentro de un *namespace* lo que nos puede ayudar a identificarlas fácilmente ya que el valor que tendrá este prefijo siempre será el valor que le hayamos asignado al atributo `name` del objeto con la configuración que se pasa en la invocación de la función `createSlice`. 


## Ejercicio

En este ejercicio vamos a crear un nuevo *slice* dentro de nuestra aplicación para la gestión de bugs que nos permita gestionar la lista de proyectos que forman parte de nuestra aplicación. En concreto se tiene que cumplir que cuando arranque nuestra aplicación deberá lanzarse una acción a la que vamos a denominar `projectAdded`:

```javascript
@@INIT
projects/projectAdded
```

Y cuando esta acción es tratada por parte de nuestra aplicación la ifnormación que ha de estar contenida dentro del *store* ha de ser la siguiente:

```javascript
[{ id: 1, name: 'Project 1' }]
``` 

El fichero que contiene el código del partido a partir del cual tenemos que realizar nuestro ejercicio es el que se muestra a continuación el cual estará contenido en el fichero `configureStore.js` reemplazando lo que teníamos definido hasta ahora (en el siguiente capítulo veremos como podemos combinar varios *reducer* para trabajar con diferentes partes dentro de nuestra aplicación). En otras palabras, reemplazaremos el *reducer* que estaba asociando a las gestión de los bugs por el *reducer* que se encargará de la gestión de los proyectos cambiando la sentencia `import`:

```javascript
import { configureStore } from '@reduxjs/toolkit'
import reducer from './projects'

export default function() {
  return configureStore({ reducer })
}
```

### Solución

Lo primero que tenemos que hacer es crear dentro del directorio `src/store` de nuestro proyecto un nuevo fichero denominado `projects.js` ya que es dentro del mismo donde vamos a recoger el *duck* con toda la información asociada a la gestión de los proyectos de la aplicación y dentro del mismo lo primero que haremos será importar la función `createSlice`:

```javascript
import { createSlice } from '@reduxjs/toolkit'
```

Ahora pasamos a invocar a la función definiendo el objeto con la configuración para nuestro *slice*. En primer lugar definimos que el nombre que le queremos dar será `projects`:

```javascript
createSlice({
  name: 'projects'
})
```

En lo que respecta al estado inicial del *store* relacionado con la gestión de los proyectos vamos a considerar que cuando arranca la aplicación no habrá ningún proyecto definido por lo que asignaremos al atributo `initialState` un array vacío:

```javascript
createSlice({
  name: 'projects',
  initialState: [],
})
```

El siguiente paso consistirá en definir el valor del atributo `reducers` del objeto que, si recordamos de la explicación anterior, en esencia lo que tenemos que hace es definir un atributo por cada una de las acciones que se pueden llevar a cabo sobre el *store* dándole como valor una función que se encargará de gestionar dicha acción, función que recibe dos parámetros: el estado actual del *store* y el objeto que representa a la acción que se ha llevado a cabo. Como en nuestro ejercicio se nos está diciendo que únicamente tenemos que gestionar una acción `projectAdded` escribiremos lo siguiente:

```javascript
createSlice({
  name: 'projects',
  initialState: [],
  reducers: {
    projectAdded: (state, action) => {
      // ...TODO
    }
  }
})
```

De hecho podemos sustituir el nombre del primero de los parámetros de la función por el nombre de la información con la que estamos trabajando dentro del *store* que en este caso son los proyectos que se están gestionando. Ahora lo que tenemos que hacer es crear el código que nos va a permitir registrar un nuevo proyecto dentro del *store* para lo cual definiremos una variable fuera de la función `createSlice` que sirva para llevar la cuanta del identificador del proyecto:

```javascript
let lastId = 0
createSlice({...})
```

Y ahora ya podemos utilizarla dentro del código de la función y así poder crear la información del objeto que representará al proyecto dentro del *store*:

```javascript
createSlice({
  name: 'projects',
  initialState: [],
  reducers: {
    projectAdded: (project, action) => {
      project.push({
        id: ++lastId,
        name: action.payload.name
      })
    }
  }
})
```

El siguiente paso consistirá en guardar el objeto que retorna la invocación de `createSlice` en una variable para así poder exportar el atirbuto `reducer` como el *export default* y las funciones que están asociadas al objeto vinculado con el atributo `actions` como los *named exports*.

```javascript
const slice = createSlice({ ... })

export { projectAdded } from slice.actions
export default slice.reducer
```

Agrupando todas las explicaciones anteriores podemos concluir que el código que formará parte del fichero `projects.js` es el que se muestra a continuación:

```javascript
import { createSlice } from '@reduxjs/toolkit'

let lastId = 0

const slice = createSlice({
  name: 'projects',
  initialState: [],
  reducers: {
    projectAdded: (project, action) => {
      project.push({
        id: ++lastId,
        name: action.payload.name
      })
    }
  }
})

export { projectAdded } from slice.actions
export default slice.reducer
```

Si ahora queremos ver si funciona nuestro modificación simplemente nos dirigimos al fichero `index.js` para realizar las siguientes modificaciones:

```javascript
import configureStore from './store/configureStore'
import * as actions from './store/bugs'
import { projectAdded } from './store/projects'

const store = configureStore() 

store.dispatch(projectAdded({ name: 'Project 1' }))
store.dispatch(actions.bugAdded('Bug 1'))
store.dispatch(actions.bugAdded('Bug 2'))
store.dispatch(actions.bugAdded('Bug 3'))
store.dispatch(actions.bugResolved(1))
```

Si ahora guardamos esta modificación y vemos que es lo que está pasando dentro de **Redux Dev Tools** observaremos que las acciones que se han llevado a cabo sobre el *store* son las siguientes:

```javascript
@@INIT
projects/projectAdded
bugs/bugAdded
bugs/bugAdded
bugs/bugAdded
bugs/bugResolved
```

Si ahora nos situamos sobre la primera de ellas veremos dentro de la pestaña *Diff* que en el *store* se ha creado un nuevo elemento con el siguiente valor:

```javascript
0: { id: 1, name: 'Project 1' }
```

Pero ¿qué sucede con el resto de las acciones que se han lanzado sobre el *store*? Nos estamos refiriendo a las acciones del tipo `bugs/bugAdded`. La respuesta es que no tienen ningún tipo de impacto ya que si seleccionamos cualquiera de ellas y nos vamos a la pestaña *Diff* no se estará mostrando ningún cambio. ¿La razón? Nuestro *store* no tiene asociados un *reducer* que sepa tratar este tipo de acciones así que, pese a que son lanzadas sobre el mismo, simplemente se ignoran.

En el siguiente capítulo estudiaremos cómo se pueden combinar los *reducer* que podamos tener en nuestra aplicación con el fin de gestionar la información del *store*.
