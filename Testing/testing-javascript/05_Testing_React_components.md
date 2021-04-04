# Testing React Components.

- Introduction.
- Render a Component for Testing.
- Use Jest DOM for Improving Assertions.
- Use DOM Testing Library to Write More Maintainable Test.

## Introduction.

En este capítulo vamos a centrarno en estudiar las estrategias y librerías que tenemos a nuestra disposición para poder testear nuestros componentes de React haciendo uso tanto de Jest como de react-testing-library. Aunque este capítulo estará centrado en React los procedimientos y mecanismos que vamos a estudiar son válidos también para probar componentes desarrollados en otros frameworks por lo que su conocimiento nos puede ayudar en nuestro día a día.

## Render a Component for Testing.

Supongamos que en nuestra aplicación de React tenemos un componente en el que simplemente se le mostrará al usuario un formulario formado por un único campo de texto en el que ha de introducir un número comprendido entre dos valores y en el caso de que no sea así se le muestre un mensaje de error. Una posible implementación para este componente es la que mostramos a continuación:

```js
import React from 'react'

function FavoriteNumber({ min = 1, max = 9 }) {
  const [number, setNumber] = React.useState(0)
  const [numberEntered, setNumberEntered] = React.useState(false)

  function handleChange(event) {
    setNumber(Number(event.target.value))
    setNumberEntered(true)
  }

  const isValid = !numberEntered || (number >= min && number <= max)

  return (
    <div>
      <label htmlFor='favorite-number'>Favorite Number</label>
      <input id='favorite-number' type='number' value={number} onChange={handleChange} />
      {isValid ? null : <div role='alert'>The number is invalid</div>}
    </div>
  )
}

export { FavoriteNumber }
```

En nuestra implementación estamos creando un Functional Component de React el cual recibe como props los valores mínimo y máximo entre los que ha de estar comprendida la entrada del usuario para determinar que se trata de un valor válido. Además estamos haciendo uso de dos variables de estados para recoger, por una parte, el valor que ha proporcionado el usuario (variable `number`) y por otro si se ha producido una entrada de datos por parte del mismo (variable `numberEntered`), asi, para determinar si se ha producido un error en la entrada de datos por parte del usuario lo que tendremos que ver es que se entran introduciendo datos y además que el valor introducido está comprendido en el rango de valores permitidos.

Con esto en mente si se ha producido un error el valor de la variable `isValid` será `false` lo que hace que cuando finalice el renderizado de nuestro componente la pantalla se podrá ver un mensaje de error mientras que si los datos son válidos no pasará nada. Por último destacar que hemos definido la función `handleChange` que será la encargada de gestionar cualquier cambio en la entrada de datos por parte del usuario estableciendo para ello tanto el valor de la variable del estado `number` con el número que ha sido introducido como el valor de la varriable del estado `numberEntered` a `true` indicando de esta manera que se está produciendo una entrada de datos por parte del usuario.

Vamos a contruir nuestro un test para probar nuestro componente en el que simplemente querremos comprobar que se está renderizando en la página del usuario un cuadro de texto (que solamente admita números) para realizar la entrada de datos y que además ese cuadro irá acompañdo de una etiqueta `label` con un mensaje concreto. Es más, siguiendo con la convención que hemos aplicado en el [capítulo anterior](./04_Configuring_Jest.md) vamos a crear un directorio `__tests__` dentro del directorio donde está situado nuestro componente y en el mismo un nuevo archivo para contener los test sobre nuestro componente. Partiremos de lo siguiente:

```js
test('renders a number input with a label "Favorite Number"', () => {})
```

Lo siguiente que vamos a necesitar es lo que queremos probar, que en nuestro caso es el componente `FavoriteNumber` por lo que, suponiendo que este está definido en el fichero `favorite-number.js`, lo haremos gracias al uso de la siguiente instrucción `import`:

```js
import { FavoriteNumber } from '../favorite-number'

test('renders a number input with a label "Favorite Number"', () => {})
```

¿Qué es lo primero que queremos hacer en nuestro test? Pues la respuesta es que queremos comprobar que nuestro componente `FavoriteNumber` es renderizado en la página que será mostrada al usuario y para poder renderizar un componente dentro del DOM de la página vamos a necesitar hacer uso de la propia librería de React lo que significará que también vamos a tenerla que importar dentro de nuestros test:

```js
import React from 'react'
import { FavoriteNumber } from '../favorite-number'

test('renders a number input with a label "Favorite Number"', () => {
})
```

---
**Nota:** podemos explicar lo expresado anteriormente de otra manera sin más que aclarar que lo que buscamos es que dentro del código de nuestros test se pueda hacer uso de JSX y por lo tanto vamos a necesitar React para poder hacer uso de este lenguaje de marcado para que nos los transforme a código JavaScript. Cómo funciona JSX o cómo lo utiliza React es algo que queda fuera del ámbito de explicación de este manual.

---

Aún así no tenemos todo lo necesario para poder lograr que el componente sea mostrado dentro del DOM de nuestra página ya que el vincular un componente al DOM es algo que realiza ReactDOM por lo que vamos a tenerlo que importar también dentro del código de nuestro test como sigue:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { FavoriteNumber } from '../favorite-number'

test('renders a number input with a label "Favorite Number"', () => {
})
```

¿Y qué metodo de ReactDOM se utiliza para vincular a un componente con un elemento del DOM? La respuesta es que se utilizará el método `render` que nos proporciona el objeto donde como primer parámetro se recibirá el componente que se quiere vincular al DOM y como segundo el elemento del DOM a partir del cual realizar la vinculación. Por lo tanto en nuestro test escribiremos algo como lo que sigue:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { FavoriteNumber } from '../favorite-number'

test('renders a number input with a label "Favorite Number"', () => {
  ReactDOM.render(<FavoriteNumber />, div)
})
```

Ahora bien ¿qué es ese `div`? Pues el elemento html al que queremos vincular nuestro componente `FavoriteNumber` lo que pasa es que en el test no sabemos de cuál se trata por lo que preaviamente a la realización de la vinculación lo vamos a tener que crear gracias a la utilización del método `createElement` que nos ofrece el objeto `document` del DOM, método que simplemente recibirá como parámetro el nombre de la etiqueta html que queremos que sea creada y que en nuestro caso va a ser un `div`:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { FavoriteNumber } from '../favorite-number'

test('renders a number input with a label "Favorite Number"', () => {
  const div = document.createElement('div')
  ReactDOM.render(<FavoriteNumber />, div)
})
```

Antes de continuar con la realización nuestro test vamos a escribir por la consola qué es lo que está recogido dentro del atributo `innerHTML` del `div` que hemos construido para podernos hacer una idea mucho más clara de cúal ha sido la vinculación ya que en este atributo se recoge todo el código hmtl que está contenido dentro del elemento en cuestión. Por lo tanto escribiremos:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { FavoriteNumber } from '../favorite-number'

test('renders a number input with a label "Favorite Number"', () => {
  const div = document.createElement('div')
  ReactDOM.render(<FavoriteNumber />, div)
  console.log(div.innerHTML)
})
```

Si abrimos una terminal del sistema y ejecutamos nuestros test nos vamos a encontrar con algo como lo siguiente:

```bash
$ npm run test

 PASS  src/react/__tests__/favorite-number.js
  ✓ renders a number input with a label "Favorite Number" (18ms)

  console.log src/react/__tests__/favorite-number.js:8
    <div><label for="favorite-number">Favorite Number</label><input id="favorite-number" type="number" value="0"></div>
```

Es decir, que se nos está mostrando que el marcado que está asociado a nuestro componente es el que esperábamos tras analizar el código JSX que lo define, es decir, que tenemos un `div` que contiene un elemento `label` y un elemento `input` que es de tipo `number` y que inicialmente tiene asignado el valor 0.

Lo siguiente que querríamos hacer en nuestro test sería comprobar que el tipo de cuadro de texto que se ha renderizado en la página es `number` por lo que gracias a que todos los objetos que representan a los nodos html nos proporcionan el método `querySelector` al que se le puede pasar un tipo de selector CSS para localizar uno o más de sus nodos hijos. En nuestro caso vamos a aprovecharnos de esta característica para a partir del `div` en el que se ha vinculado el componente buscar el campo de texto (elemento `input`) y una vez lo tenemos comprobar que su atributo `type` (que recogerá el tipo de cuadro de texto del que se trata) es `number`. De hecho esta es la primera de las aserciones que vamos a hacer:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { FavoriteNumber } from '../favorite-number'

test('renders a number input with a label "Favorite Number"', () => {
  const div = document.createElement('div')
  ReactDOM.render(<FavoriteNumber />, div)
  expect(div.querySelector('input').type).toBe('number')
})
```

La siguiente aserción que nos gustaría realizar el que existe un elemento `label` y además que el texto que le acompaña es `Favorite Number` por lo que, siguiente la misma estrategia que en el caso anterior, lo que haremos será elegir el nodo hijo `label` y luego consultar el contenido del atributo `textContent` que tendrá el texto que está asociado a dicha etiqueta:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { FavoriteNumber } from '../favorite-number'

test('renders a number input with a label "Favorite Number"', () => {
  const div = document.createElement('div')
  ReactDOM.render(<FavoriteNumber />, div)
  expect(div.querySelector('input').type).toBe('number')
  expect(div.querySelector('label').textContent).toBe('Favorite Number')
})
```

Si ahora ejecutamos el test podemos comprobar que este pasa sin problemas:

```bash
$ npm run test

 PASS  src/react/__tests__/favorite-number.js
  ✓ renders a number input with a label "Favorite Number" (20ms)
```

## Use Jest DOM for Improving Assertions

¿Qué sucede en el caso de que alguno de los test anteriores falle como consecuencia de que, por ejemplo, nos hayamos equivocado a la hora de definir el tipo de elemento al que queremos acceder con la invocación del método `querySelector`. Dicho de otra manera, si escribimos algo como lo siguiente en nuestro test:

```js
expect(div.querySelector('nput').type).toBe('number')
```

Si ahora lo ejecutamos Jest nos informará de que el test falla ya que al no poder localizar el elemento html `nput` (el que hemos escrito mal) en consecuencia no podrá acceder a su atributo `type` ya que la invocación del mismo habrá retornado un `null` y por lo tanto se no puede realizar ninguna comprobación:

```bash
$ npm run test

 FAIL  src/react/__tests__/favorite-number.js
  ✕ renders a number input with a label "Favorite Number" (20ms)

  ● renders a number input with a label "Favorite Number"

    TypeError: Cannot read property 'type' of null
```

En otras palabras, vemos que Jest no nos ayuda demasiado a la hora de describir el error que se ha producido por lo que sería deseable disponer de un mecanismo de error que nos ayude a localizarlo más fácilmente. Afortunadamente tenemos una librería que nos puede ayudar a ello y es la que se conoce como `@testing-library/jest-dom` la cual nos va a proporcionar más aserciones que podemos llevar a cabo sobre nuestro código. Para utilizarla, como siempre, lo primero que vamos a tener que hacer es instalarla como una dependencia de desarrollo de la siguiente manera:

```bash
$ npm install --save-dev @testing-library/jest-dom
```

Una vez finalizada la instalación el siguiente paso consistirá en importar una de las funciones que nos proporciona para lograr nuestro objetivo, la función `toHaveAttribute` de la siguiente manera:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { FavoriteNumber } from '../favorite-number'
import { toHaveAttribute } from '@testing-library/jest-dom/matchers'
```

pero nos quedará un paso adicional si queremos que pueda ser utilizado como parte del objeto `expect` que de forma global pone a nuestra disposición Jest y es que si invocamos a su método `extend` pasándole como parámetro un objeto que tendrá un método formado por esta funció lo que va a permitir que pase a estar disponible para hacer las aserciones en nuestros test. Esto que es complejo de explicar es sencillo de ver en el código:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { FavoriteNumber } from '../favorite-number'
import { toHaveAttribute } from '@testing-library/jest-dom/matchers'

expect.extend({ toHaveAttribute })
```

¿Y ahora cómo podemos utilizarlo? Pues simplemente tenemos que saber que esta aserción (función) va a recibir dos parámetros siendo el primero de ellos el nombre del atributo del elemento html que se quiere verificar y el segundo el valor que se espera que tenga dicho atributo. Así en nuestro ejemplo podemos escribir nuestro código como sigue:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { FavoriteNumber } from '../favorite-number'
import { toHaveAttribute } from '@testing-library/jest-dom/matchers'

expect.extend({ toHaveAttribute })

test('renders a number input with a label "Favorite Number"', () => {
  const div = document.createElement('div')
  ReactDOM.render(<FavoriteNumber />, div)
  expect(div.querySelector('nput')).toHaveAttribute('type', 'number')
  expect(div.querySelector('label').textContent).toBe('Favorite Number')
})
```

Así ahora cuando estemos ejecutando nuestro test el mensaje de error debido a que nos hemos equivocado a la hora de escribir el nombre de la etiqueta que vamos a buscar es mucho más descriptivo que en los casos anteriores, siendo algo similar a lo siguiente:

```bash
$ npm run test

 FAIL  src/react/__tests__/favorite-number.js
  ✕ renders a number input with a label "Favorite Number" (21ms)

  ● renders a number input with a label "Favorite Number"

    expect(received).toHaveAttribute()

    received value must be an HTMLElement or an SVGElement.
    Received has value: null
```

Ahora el mensaje que nos proporciona Jest es mucho más claro que en el caso anterior ya que no estará indicando qué la aserción `toHaveAttribute` espera ser llamado sobre un objeto `HTMLElement` o bien sobre un objeto `SVGElement` y sin embargo se está llamando sobre un objeto `null` lo que nos tiene que hacer pensar en que el parámetro que hemos proporcionado al método `querySelector` no estará retornando ningún valor y por lo tanto no se puede llevar a cabo la comprobación.

De forma análoga a como tenemos la función `toHaveAttribute` tenemos la librearía `toHaveTextContent` que también podemos incluir a nuestros test de la misma forma que hemos hecho anteriormente (es decir, importando la función y pasándola como un método más al objeto que se le pasa como parámetro al método `extend` del objeto `expect`) además de corriengo el error en el test anterior, lo que nos deja el siguiente código:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { FavoriteNumber } from '../favorite-number'
import { toHaveAttribute, toHaveTextContent } from '@testing-library/jest-dom/matchers'

expect.extend({ toHaveAttribute, toHaveTextContent })

test('renders a number input with a label "Favorite Number"', () => {
  const div = document.createElement('div')
  ReactDOM.render(<FavoriteNumber />, div)
  expect(div.querySelector('input')).toHaveAttribute('type', 'number')
  expect(div.querySelector('label')).toHaveTextContent('Favorite Number')
})
```

Si ahora volvemos a ejecutar los test desde la terminal de nuestro sistema veremos que este pasa sin ningún tipo de problema:

```bash
$ npm run test

 PASS  src/react/__tests__/favorite-number.js
  ✓ renders a number input with a label "Favorite Number" (21ms)
```

Teniendo en cuenta que son muchas las funciones que pone a nuestra disposición la librería `jest-dom` una técnica que se suele seguir es importarlas todas dentro del código de nuestra aplicación como sigue:

```js
import * as jestDOM from '@testing-library/jest-dom/matchers'
```

de tal manera que todas las funciones quedan asignados a un nuevo objeto que hemos decidido llamar `jestDOM` y que podremos pasar directamente como parámetro al método `extend` del objeto `expect`:

```js
expect.extend(jestDOM)
```

y sin tener que hacer ningún tipo de cambio adicional más nuestros test seguirán funcionando sin que se produzca ningún tipo de error dejando nuestro código mucho más legible.

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { FavoriteNumber } from '../favorite-number'
import * as jestDOM from '@testing-library/jest-dom/matchers'

expect.extend(jestDOM)

test('renders a number input with a label "Favorite Number"', () => {
  const div = document.createElement('div')
  ReactDOM.render(<FavoriteNumber />, div)
  expect(div.querySelector('input')).toHaveAttribute('type', 'number')
  expect(div.querySelector('label')).toHaveTextContent('Favorite Number')
})
```

Pero incluso sería mucho mejor si tuviésemos una forma en la que con una única instrucción pudiésemos importar todas las funcione sque nos proporciona `jest-dom` y ejecutar el método `extend` del objeto `expect`. Para lograrlo la librería nos ofrece importar directamente un archivo que se encargará de realizar los dos pasos tal y como sigue:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { FavoriteNumber } from '../favorite-number'
import '@testing-library/jest-dom/extend-expect'

test('renders a number input with a label "Favorite Number"', () => {
  const div = document.createElement('div')
  ReactDOM.render(<FavoriteNumber />, div)
  expect(div.querySelector('input')).toHaveAttribute('type', 'number')
  expect(div.querySelector('label')).toHaveTextContent('Favorite Number')
})
```

es decir que la importación de `@testing-library/jest-dom/extend-expect` nos proporciona lo que estábamos persiguiendo de una forma mucho más sencilla y sin que se nos olvide alguno de los pasos para lograrlo.

>
> Se recomienda visitiras el [página en GitHub](https://github.com/testing-library/jest-dom) de la librería para poder obtener información de todas las aserciones que se pueden llevar a cabo sobre nuestro código.
>

## Use DOM Testing Library to Write More Maintainable Test

Cuando estamos realizando los test de nuestras aplicaciones nos podemos encontrar en casos en los que por alguna razón hayamos roto algo dentro del código de nuestros componentes pero esto no se vea recogido en nuestros test, lo que se acaba convirtiendo en un falso positivo. Por poner un ejemplo, supongamos que en nuestro componente `FavoriteNumber` rompemos la relación que vincula a la etiqueta `label` con el campo del texto por un error tipográfico como el siguiente:

```js
<label htmlFor='favorie-number'>Favorite Number</label>
<input id='favorite-number' type='number' value={number} onChange={handleChange} />
```

Como podemos deducir nuestros test pasarán sin problemas (no estamos comprobando un caso de uso como el anterior) pero sin embargo nuestra aplicación quedará rota para todos aquellos usuarios que estén utilizando herramientas como lectores de pantalla ya que no podrán resolver el elemento con el que está relacionado la etiqueta `label`. Esto lo que nos viene a decir es que deberíamos hacer algún tipo de aserción que nos permita verificar que existe esa relación entre ambas etiquetas y, como es evidente, esto lo podríamos hacer de forma directa en el código de nuestro test sin más que obtener el valor del atributo `for` del elemento `label` que se renderiza y posteriormente comprobar que su valor es el mismo que el del atributo `id` del elemento `input` que se ha renderizado. Sin embargo, la desventaja de esta aproximación es que puede haber más formas de realizar esta vinculación entre estos dos formularios y no nos gustaría tener que preocuparnos por ello cuando estamos probando nuestro código ya que lo que realmente nos importa es comprobar que la vinculación existe. Así, lo ideal sería el disponer de algún tipo de abstracción que nos permita obtener cuál es el `input` que está asociado a una `label`. Y esto es precisamente una de las funcionalidades que nos ofrece la librería `@testing-library/dom`.

Como siempre cada vez que vayamos a utilizar una librería es instalarla dentro del proyecto en este caso como una dependencia de desarrollo porque solamente la vamos a utilizar cuando estemos haciendo los test por lo que escribiremos:

```bash
$ npm install --save-dev @testing-library/dom
```

El siguiente paso será importar el objeto `queries` que nos ofrece esta librería para poderla utilizar más adelante. Así escribiremos lo siguiente:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { FavoriteNumber } from '../favorite-number'
import '@testing-library/jest-dom/extend-expect'
import { queries } from '@testing-library/dom'
```

Lo primero que vamos a comprobar es que si el `div` al que hemos vinculado el componente de React que estamos testeando (más concretamente si el DOM formado por los hijos que forman el marcado del componente) si existe una hijo que sea del tipo `label` y que posea un texto determinado. Para ello tenemos que saber que el objeto `queries` nos ofrece el método `getByLabelText` donde el primer elemento será el nodo a partir del cual realizar la búsqueda y como segundo parámetro el texto que recoge la etiqueta `label`, lo que nos permite escribir hacer lo siguiente:

```js
queries.getByLabelText(div, 'Favorite Number')
```

pero ¿qué retorna este método? pues el elemento html que representa al campo del formulario que está asociado a dicho `label` (si es que existe, lo que sucederá es que se lanzará un error haciendo que el test no pase). Por lo tanto la salida de la invocación del método se lo asignamos a una variable:

```js
const input = queries.getByLabelText(div, 'Favorite Number')
```

Una vez tenemos el elemento del formulario podríamos seguir refactorizando nuestro test simplemente haciendo la verificación de que el elemento posee el atributo `type` y que es del tipo `number` como sigue:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { FavoriteNumber } from '../favorite-number'
import '@testing-library/jest-dom/extend-expect'
import { queries } from '@testing-library/dom'

test('renders a number input with a label "Favorite Number"', () => {
  const div = document.createElement('div')
  ReactDOM.render(<FavoriteNumber />, div)
  const input = queries.getByLabelText(div, 'Favorite Number')
  expect(input).toHaveAttribute('type', 'number')
})
```

Si ahora nos equivocamos a la hora de realizar la asignación entre el identificador del campo de texto o la etiqueta `label` a la hora de ejecutar el test nos aparecerá un error como el siguiente:

```bash
$ npm run test

 FAIL  src/react/__tests__/favorite-number.js
  ✕ renders a number input with a label "Favorite Number" (26ms)

  ● renders a number input with a label "Favorite Number"

    TestingLibraryElementError: Found a label with the text of: Favorite Number, however no form control was found associated to that label. Make sure you're using the "for" attribute or "aria-labelledby" attribute correctly.
```

Si corregimos el error a la hora escribir la asociación el test pasará a funcionar sin problemas. No obstante un apunte que conviene realizar es que la búsqueda de la etiqueta que se muestra en el elemento `label` es sensible a mayúsculas y minúsculas cuando le estamos pasando un string directamente (busca que tenga exactamente el mismo valor) pero es posible que el mensaje que está contenido en el etiqueta, al menos desde el punto de vista del usuario, le de igual si está en mayúsculas o minúsculas por lo que para estas situaciones se recomienda pasar una expresión regular en vez de un string cuando se está invocando al método. En nuestro caso si no queremos distinguir entre minúsculas y mayúsculas escribiríamos algo como lo siguiente:

```js
const input = queries.getByLabelText(div, /favorite number/i)
```

donde el sufijo `i` lo que viene a indicar dentro de una expresión regular de que ha de ignorar las diferencias entre minúsculas o mayúsculas.

Otra de las opciones que nos pueden ser interesantes para el desarrollo de nuestros test tiene que ver con el hecho de que puede existir la posibilidad de que sobre un mismo elemento del DOM tengamos que realizar más de una comprobación posteriormente aplicando todas las queries que nos proporciona la librería `jest-dom`. En este caso importaríamos la función `getQueriesForElement` que la librería como sigue:

```js
import { queries, getQueriesForElement } from '@testing-library/dom'
```

y ahora lo que podemos hacer es invocar a la misma pasándole como parámetro un elemento del dom y nos retornará un objeto con todos las queries que podemos lanzar sobre dicho elemento. Así, en nuestro ejemplo, si únicamente nos quisiésemos quedar con la función `getByLabelText` escribiríamos algo como sigue:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { FavoriteNumber } from '../favorite-number'
import '@testing-library/jest-dom/extend-expect'
import { getQueriesForElement } from '@testing-library/dom'

test('renders a number input with a label "Favorite Number"', () => {
  const div = document.createElement('div')
  ReactDOM.render(<FavoriteNumber />, div)
  const { getByLabelText } = getQueriesForElement(div)
  const input = getByLabelText(/favorite number/i)
  expect(input).toHaveAttribute('type', 'number')
})
```

>
> Se recomienda leer el documentación en la [página de GitHub](https://github.com/testing-library/dom-testing-library) de la librería `@testing-library/dom` para tener el listado completo de los métodos que tenemos a nuestra disposición para poder realizar las búsqueda de los elementos hijos a partir de un nodo contenedor.
>









