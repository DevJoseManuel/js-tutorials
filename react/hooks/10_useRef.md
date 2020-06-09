# `useRef`

Para realizar el estudio en profundidad del **hook** `useRef` vamos a partir de un componente básico de React en el que simplemente mostramos un campo de texto en el que esperamos que el usuario nos escriba información (en este caso su nombre) y, a continuación, en dentro de un elemento html `<div>` lo que haremos será escribir el nombre introducido en dicho campo.

```javascript
import React, { useState } from 'react'

export default function App() {
  const [name, setName] = useState('')

  return (
    <>
      <input type='text' value={ name } onChange={ e => setName(e.target.value) } />
      <div>Your name is { name }</div>
    </>
  )
}
```

Como podemos observar este componete de React es realmente simple (de hecho uno de los más básicos que podemos llegar a crear) presentando como única peculiaridad que estamos haciendo uso del **hook** `useState` para guardar el valor del campo del formulario.

Vamos a ir un paso más adelante y supongamos que queremos recoger de alguna manera el número de veces que este componente ha sido renderizado y no solamente eso sino que además queremos mostrár esta información al usuario. Esto quiere decir que en la parte correspondiente al código JSX de nuestro componente añadiríamos algo como lo siguiente:

```javascript
return (
  <>
    <input type='text' value={ name } onChange={ e => setName(e.target.value) } />
    <div>Your name is { name }</div>
    <div>I rendered { x } times</div>
  </div>
)
```

donde `x` será la variable que recogerá el número de veces que ha sido renderizado. Pero ¿qué podemos hacer para recoger el valor de esta variable? Lo primero que se nos puede ocurrir es utilizar una nueva variable variable dentro del estado del componente la cual declararemos utilizando nuevamente el **hook** `useState`:

```javascript
const [name, setName] = useState('')
const [renderCount, setRenderCount] = useState(0)
```

¿Y qué tendremos que hacer ahora para poder incrementar el valor de esta variable cada vez que se renderiza de nuevo nuestro componente? La respuesta a esta pregunta pasa por contestar previamente a ¿qué **hook** sabemos que se puede definir para que se ejecute en cada nuevo reendizado del componente? La respuesta es `useEffect` sin tener que especificar la variable que controlará cuando se ha de ejecutar la función definida en dicho **hook**. Así pues, en primer lugar importamos `useEffect` para poderlo utilizar:

```javascript
import React, { useState, useEffect } from 'react'
```

Y, tras definir las variables que formarán parte del estado del componente con el **hook** `useState` vamos a definir el código de la función que queremos que se ejecute con cada renderizado de nuestro componente. Así pues, definimos lo siguiente:

```javascript
useEffect(() => {
  setRenderCount(prevRenderCount => prevRenderCount + 1)
})
```

de esta manera tendremos la garantía de que cada vez que se renderiza el componente se ejecuta la función contenida dentro de `useEffect` en la cual simplemente se llama a la función que se encargará de establecer el valor de la variable `renderCount` que está definida en el estado del componente (recordemos que la función `setRenderCount` es la función que nos retorna el **hook** `useState` cuando estamos definiendo la variable del estado).

---
**Nota:** es importante recordar que cuando estamos utilizando la función que nos permite establecer el nuevo valor de una variable del estado de un componente que nos retorna el **hook** `useState` se permite utilizar como parámetro una función la cual recibe como único parámetro el valor anterior de la variable que se quiere actualizar y ha de retornar el valor con el que se actualizará dicha variable.

Así, en nuestro ejemplo la función que hemos utilizado como parámetro para `setRenderCount` estará recibiendo como parámetro el valor que previamente tendría este variable antes de ejecutar la función y lo único que hacemos es retornar dicho valor incrementado en una unidad.

---

¿Qué problema hay en el código anterior? Pues si nos paramos a pensar un poco en el comportamiento de nuestro componente vemos que hemos definido que el **hook** `useEffect` se ha de ejecutar siempre con cada nuevo ciclo de renderizado lo cual quiere decir que la primera vez que se monta el componente en el DOM de la página se ejecutará la función que tiene asociada incrementando en una unidad el número de veces que se ha mostrado el componente. Como este valor se guarda en un atributo del estado del componente y dicho valor se ha modificado, React nos garantizará que se volverá a renderizar el componente, lo que provoca que se vuelva a ejecutar la función contenida en el `useEffect`, lo que hace que se modifique el valor de la variable recogida en el estado, lo que provoca un nuevo renderizado, etc.

En definitiva, con la definición que acabamos de hacer lo que hemos conseguido ha sido crear un bucle infinito de renderizado del componente. Así, si queremos guardar el número de veces que un componente ha sido renderizado está claro que almacenar la información en el estado del componente no es la solución adecuada.

Entonces ¿qué solución podemos adoptar? En React la solución pasa por hacer uso de algo que se conoce como **refs**. Lo primero que tenemos que saber de ellas es que en cierta manera la información que gestiona es parecida a la que guardamos en el estado del componente en el sentido de que se conserva entre cada uno de los renderizados del componente en el que se está utilizando pero con la salvedad que cualquier actualización que realicemos en esa información no va a provocar que el componete se renderice de nuevo (como sí que sucede con la información recogida en el estado).

Para nuestro caso de uso parece que el uso de una **ref** es lo más adecuado por lo que vamos a hacer uso de otro de los **hooks** que nos proporciona React: `useRef`. Este **hook** espera recibir como parámetro el valor inicial que querremos proporcionarle a nuestra **ref** y, al contrario que sucede con el `useState`, únicamente nos retornará una valor que es lo que se conoce como **ref**.

Así pues, si aplicamos este concepto a nuestro ejemplo, lo primero que tendremos que hacer será importar este **hook** dentro de nuestro componente:

```javascript
import { React, useState, useEffect, useRef } from 'react'
```

Hecho esto ya podemos definir nuestra **ref** de la siguiente manera:

```javascript
const [name, setName] = useState('')
const renderCount = useRef(0)
```

Es decir, que hemos definido nuestra **ref** `renderCount` y la hemos inicializado al valor 0 indicando de esta manera que inicialmente el número de veces que se ha renderizado el componete será de cero (ninguna).

Ahora bien, ¿qué es exactamente lo que nos retorna la invocación del **hook** `useRef`? La respuesta no es valor primitivo como podríamos estar pensando sino que retorna un objeto que posee un único atributo denominado `current` cuyo valor será igual al valor que le hemos pasado como parámetro al **hook**. Esto nos viene a decir que, en nuestro ejemplo, cuando se ejecute el **hook** `useRef` el objeto que tendrá asignado la variable `renderCount` será el siguiente:

```javascript
{ current: 0 }
```

Lo que tenemos que entender es que el valor que está asignado al atributo `current` de nuestra **ref** es el valor que persistirá entre cada uno de los nuevos render del componente. Partiendo de esto podemos volver a hacer uso nuevamente del **hook** `useEffect` pero en este caso en cada uno de los ciclos de renderizado que se ejecuten lo que haremos será incrementar en una unidad el valor de este atributo `current` teniendo además la certeza de que esta operación no provoca un nuevo renderizado de nuestro componente:

```javascript
useEffect(() => {
  renderCount.current = renderCount.current + 1
})
```

¿Qué deberemos escribir ahora en el código JSX para poderle mostrar al usuario el número de veces que ha sido renderizado el componente? Pues simplemente el valor del atributo `current`  de nuestra **ref** de la siguiente manera:

```javascript
<>
  <input type='text' value={ name } onChange={ e => setName(e.target.value) } />
  <div>Your name is { name }</div>
  <div>I rendered { renderCount.current }</div>
</div>
```

Por lo tanto, lo más importante con lo que nos tenemos que quedar del uso de las **refs** dentro de nuestro componentes es que nos van a servir para guardar información que queremos que esté relacionada con el componente en el que se utiliza de una forma muy similar a la información que guardamos en el estado pero sabiendo que un cambio en dicha información **nunca** va a provocar que se realice un nuevo renderizado del componente en el que se está utilizando.

Ahora bien, la forma más habitual de encontrarnos con las **refs** de React dentro de nuestras aplicaciones es para hacer referencia a alguno de los elementos html que forma nuestra página. Pero ¿cómo podemos lograr esto? Lo primero que tenemos que saber es que en React sobre cualquiera de las etiquetas html que podemos utilizar dentro dentro del código JSX podemos definir el atributo `ref` asignándole como valor el objeto que nos ha retornado la invocación del **hook** `useRef`.

Vamos a ver este caso de uso creando una **ref** dentro de nuestro componente de ejemplo para que haga referncia al campo de texto que estamos utilizando para recoger la información del usuario. Para ello lo primero que tendríamos que hacer es crear una nueva variable dentro de nuestro componente que tenga como valor el resultado de la invocación del **hook** `useRef`. Ahora bien, ¿qué valor tendrá el atributo `current` del objeto **ref** cuando creamos la **ref**? La respuesta en este caso es ninguno (todavía no se lo hemos asignado al elemento `<input>`) por lo que utilizamos la versión de `useRef` que no recibe ningún parámetro:

```javascript
const [name, setName] = useState('')
const renderCount = useRef(0)
const inputRef = useRef()
```

El siguiente paso consistira en hacer uso del atributo `ref` dentro del elemento `<input>` que tenemos definido en el código JSX pasándole como valor el objeto **ref** que acabamos de definir. Así pues escribimos lo siguiente:

```javascript
<input
  ref={ inputRef }
  type='text'
  value={ name }
  onChange={ e => setName(e.target.value) }
/>
```

¿Con esto qué es lo que estamos consiguiendo? O dicho de otra manera, ¿qué es lo que nos garantiza React? La respuesta es que en cada nuevo renderizado del componente el valor que tendrá asignado el atributo `current` de la **ref** `inputRef` será el elemento del DOM que representa al campo de texto.

Vamos a plantearnos un nuevo caso de uso dentro de nuestro componente en el cual vamos a mostrar un botón dentro de la página de tal manera que cuando se pulse lo que queremos que suceda es que el foco se sitúe en nuestro campo de texto de tal manera que podamos comenzar a escribir valores dentro de mismo inmediatamente. Para ello lo primero que tenemos que hacer es crear el código JSX que se encargue de mostrar el nuevo botón:

```javascript
<div>I rendered { renderCount.current }</div>
<button onClick={ focus }>Focus</button>
```

El siguiente paso consistirá en definir la función `focus` ya que esta será la que se ha de ejecutar en el momento en el que se pulsa en el botón. En esta función lo que tendremos que hacer será acceder al atributo `current` de nuestra **ref** (que recordemos que representa al nodo del DOM que representa nuestro campo de texto) por lo que simplemente invocaremos a su método `focus` para establecerle el foco:

```javascript
function focus() {
  inputRef.current.focus()
}
```

---
**Nota:** el nodo que tendrá asignado el atributo `current` será el mismo (es decir, que tiene los mismos atributos y métodos) que si accedemos a dicho campo de texto utilizando, por ejemplo, una función de JavaScript del tipo `querySelector` del objeto `document`. Se puede obtener [más información](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector) consultando la MDN.

---

Teniendo en cuenta lo que acabamos de describir hay desarrolladores que prefieren gestionar ellos mediante el uso de **refs** los valores que están asociados a los campos de texto, etc. dejando de lado todas las ventajas que nos ofrece el uso de React para ello (el uso del estado y la forma en la que se actualiza el valor que se almacena dentro del mismo). Pero no solamente esto, también deberíamos evitar siempre el uso de métodos como `appendChild` dentro de un objeto DOM al que hagamos referencia con una **ref** ya que siempre complicará el código con el que estaremos trabajando.

Existe una caso de uso adicional en el que el uso de las **refs** es realmente adecuado y es cuando queremos guardar el valor previo de alguno de las propiedades de nuestro estado ya que de otra manera no tendríamos una forma fácil de acceder al valor anterior al último renderizado de alguno de los valores que se está guardando en el estado. Pero ¿qué pasos tenemos que dar para lograrlo?

Siguiendo con nuestro ejemplo, vamos a suponer que lo que queremos guardar es el valor que antes del último renderizado ha tenido asignado la variable del estado `name`. Lo primero que tendremos que ver es cómo podemos guardar el valor que previamente tenía dicha variable en la **ref** para lo cual nuevamente vamos a volver a hacer uso del **hook** `useEffect` pero esta vez centrándonos en que la función que se queremos que se ejecute únicamente deberá hacerlo cada vez que cambie el valor de la variable del estado `name` (es decir, que vamos a hacer uso del segundo de los parámetros que recibe el **hook** `useEffect` indicando como variable a chequear `name`):

```javascript
useEffect(() => {
  // aquí el código de la función.
}, [name])
```

¿Qué más vamos a necesitar? Eviedentemente la **ref** en la que queremos recoger el valor que previamente tenía la variable `name`. Esto se traduce en que antes de la declaración de `useEffect` vamos a hacer una invocación del **hook** `useRef` en el que estableceremos que el valor anterior de la variable en cuestión la primra vez que se renderiza el componente será igual a la cadena vacía:

```javascript
const prevName = useRef('')

useEffect(() => {
  // aquí el código de la función.
}, [name])
```

Con esto en mente dentro de `useEffect` lo único que tendremos que hacer será actualizar el valor del atributo `current` de la **ref** que acabamos de crear para que guarde el valor de la variable del estado:

```javascript
useEffect(() => {
  prevName.current = name
}, [name])
```

Además si queremos mostrar tanto el valor actual que ha sido introducido en el campo del formulario (en otras palabras, el valor que tienen asignado la variable del estado `name`) además de la que tenía previamente escribiremos el siguiente código JSX:

```javascript
<div>Your name is { name } and it used to be { prevName.current }</div>
```

En este caso lo que tenemos que saber es que la primera vez que se renderice el componente el mensaje que será mostrado por la pantalla será:

```console
Your name is and it used to be 
```

Es decir, que tanto el valor de la variable `name` como el valor que está asignado a la **ref** `prevName` en su atributo `current` estarán vacíos. Si ahora escribimos un caracters (por ejemplo, `a`) en el campo de texto el mensaje que se muestra por la pantalla será:

```console
Your name is a it used to be
```

Es decir, que ahora el valor de la variable `name` dentro del estado será `a` pero sin embargo el valor del atributo `current` del objeto **ref** seguirá siendo la cadena vacía recogiendo el valor que previamente tenía la variable `name`. Si ahora tecleamos, por ejemplo, `b` el mensaje que se nos mostrará será:

```console
Your name is ab it used to be a
```

Vemos pues que el valor del atributo `current` de nuestra **ref** efectivamente está haciendo referencia al valor previo de la variable del estado `name` antes de que esta haya sido actualizada como así esperábamos.

## Resumen

Tenemos que entender que el uso de las **refs** de React no solamente van a ser útiles para tener acceso a los nodos del DOM a los cuales están vinculados sino que uno de los casos de uso más intenresantes es que nos van a permitir persistir información entre cada uno de los renderizados de los componentes, además de que si se produce una actualización en dicha información a persistir esto no va a provocar un nuevo renderizado del componente como sí que sucede en el caso de que estemos actualizando la información de una variable del estado.

Es más, en el caso de los **functional components** de React esta es la única manera que tenemos para guardar la información entre cada uno de los render del componente cosa que no sucedía en el caso de los **class components** donde siempre podíamos definir todos aquellos atributos dentro de la clase que queríamos que se pudiesen actualizar sin que esto provocase un renderizado del componente.

---
**Nota:** para obtener más información acerca de qué son los **functional components** y los **class components** se [recomienda leer la documentación oficial](https://reactjs.org/) de React.


## Componente final

El aspecto final del componente que hemos estado utilizando a lo largo de la explicación realizada en este punto es el que se muestra a continuación:

```javascript
import React, { useState, useEffect, useRef } from 'react'

export default function App() {
  const [name, setName] = useState('')
  const renderCount = useRef(0)
  const inputRef = useRef()
  const prevName = useRef('')

  useEffect(() => {
    renderCount.current = renderCount.current + 1
  })

  useEffect(() => {
    prevName.current = name
  }, [name])

  function focus() {
    inputRef.current.focus()
  }

  return (
    <>
      <input
        ref={ inputRef }
        type='text'
        value={ name }
        onChange={ e => setName(e.target.value) }
      />
      <div>Your name is { name } and it used to be { prevName.current }</div>
      <div>I rendered { renderCount.current }</div>
      <button onClick={ focus }>Focus</button>
    </>
  )
}
```
