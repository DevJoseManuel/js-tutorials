# `useQuery` hook

En el punto anterior del tutorial hemos creado dos componentes dentro de nuestra aplicación: uno que nos va a permitir obtener la información de los planetas de Star Wars y otro para mostrar la información de los personajes de las películas. En este punto vamos a comenzar a hacer llamadas asíncronas a una API externa para poder obtener la información que precisamos. En concreto vamos a utilizar la API [https://swapi.dev/](https://swapi.dev/) para ello.

Al ser este el primer ejemplo del tutorial vamos a detenernos en explicar paso a paso cada una de las acciones que tendremos que llevar a cabo para utilizar correctamente **React Query** para lograr nuestro objetivo. En concreto, vamos a ver cómo podemos utilizar el hook `useQuery` para lograr el objetivo que estamos persiguiendo.

Como sucede con cualquier otro hook que vayamos a utilizar en nuestros componentes lo primero que tendremos que hacer será importarlo dentro del código de cada uno de nuestros componentes. Así en el componente `Planets` escribiremos el siguiente código:

```javascript
import React from 'react'
import { useQuery } from 'react-query'
```

Hecho esto lo siguiente que tendremos que hacer será utilizar el hook para obtener la información que de los planetas de la saga. Lo primero que tenemos que saber es que la invocación de `useQuery` va a retornar un objeto del que nos interesarán una serie de atributos por lo que haremos uso de la deconstrucción de JavaScript para obtenerlos, aunque en este punto todavía no tenemos que preocuparnos por ello:

```javascript
const Planets = () => {
  const {} = useQuery(...)
```

¿Y qué parámetros recibe `useQuery`? El primero de ellos será un string que nos ayude a identificar la llamada que se está llevando a cabo. En nuestro ejemplo vamos a denominarlo `planets` porque esperamos obtener la información de los planetas del universo de Star Wars).

```javascript
const Planets = () => {
  const {} = useQuery('planets', ...)
```

El segundo parámetro ha de ser una función, más concretamente una función asíncrona, que se encargará de recoger la información que consulta a la API externa va a proporcionarnos de forma asíncrona. En nuestro caso vamos a llamar a esta función `fetchPlanets` ya que será la encargada de recoger la información de los planetas:

```javascript
const Planets = () => {
  const {} = useQuery('planets', fetchPlanets)
```

El siguiente paso que tendremos que dar consistirá en definir la función que se encargará de recoger la información asíncrona. Como ya hemos dicho esta ha de ser una función asíncrona por lo que la definición de la misma será algo como lo siguiente:

```javascript
const fetchPlanets = async () => {}
```

En el cuerpo de la función lo que tenemos que hacer es escribir el código que nos permita recuperar la información de la API externa. Con el fin de no utilizar ninguna librería adicional dentro de nuestra aplicación vamos a utilizar la función `fetch` que nos proporciona el navegador para realizar este tipo de consultas. Además tenemos que saber que esta función espera recibir como parámetro la url completa de la API por lo que escribiremos algo como lo siguiente:

Así comenzamos definiendo lo siguiente:

```javascript
const fetchPlanets = async () => {
  const res = fetch('https://swapi.dev/api/planets/')
}
```

La función `fetch` es una función asíncrona que retorna una Promise de JavaScript por lo que tenemos que hacer que el código de la función no continúe con su ejecución hasta que la Promise sea resuelta. Como hemos estado usando `async` para definir la función vamos a escribir la palabra reservada `await` para que el código se detenga hasta que dicha promesa sea resuelta y se asigne el resultado de la llamada asíncrona a la variable `res`:

```javascript
const fetchPlanets = async () => {
  const res = await fetch('https://swapi.dev/api/planets/')
}
```

Una vez tenbemos nuestro objeto con la respuesta de la llamada a `fetch` utilizaremos el método `json` que este nos ofrece para transformar la información a formato JSON y retornalo como resultado de la invocación de la función `fetchPlanets`:

```javascript
const fetchPlanets = async () => {
  const res = await fetch('https://swapi.dev/api/planets/')
  return res.json()
}
```

Hasta este momento no hemos hecho nada diferente en lo que haríamos si no estuviésemos utilizando **React Query** pero lo que tenemos que pensar es que internamente la librería se encargará de realizar la llamada asíncrona por nosotros, cachear la información con la respuesta y, si fuera preciso, volver a realizar la llamada para tener la última información actualizada, lo que se traduce en que no vamos a tener que volver a llamar a `fetchPlanets` en más sitios dentro de nuestro componente.

Retomemos ahora el punto en el que estábamos deconstruyendo la el objeto que retorna la invocación del hook `useQuery`. En concreto vamos a quedarnos con dos atributos del mismo: `data` que es el que contiene los datos retornados por la función que hemos definido (en nuestro caso el resultado de invocar a la función `fetchPlanets`) y `status` que nos viene a indicar el estado en el que se encuentra la consulta siendo los posibles valores `loading`, `error` o `success`:

```javascript
const Planets = () => {
  const { data, status } = useQuery('planets', fetchPlanets)
```

Pero ¿qué contiene realmente la variable `data` que acabamos de deconstruir? Una forma de verlo es escribiendo el resultado de la misma por la consola de JavaScript y viendo qué es lo que contiene. Así pues si escribimos lo siguiente:

```javascript
const Planets = () => {
  const { data, status } = useQuery('planets', fetchPlanets)
  console.log(data)
```

Y nos dirigimos a la consola de JavaScript dentro de las herramientas para desarrolladores de nuestro navegador web veremos que se escriben las siguientes líneas:

```javascript
> undefined
> undefined
> undefined
> undefined
> Object {
    count: 60,
    next: "http://swapi.dev/api/planets/?page=2",
    previous: null,
    results: Array[10]
  }
> Object {
    count: 60,
    next: "http://swapi.dev/api/planets/?page=2",
    previous: null,
    results: Array[10]
  }
```

Lo que puede parecer aquí es que realmente se están haciendo un total de seis llamadas a la API externa pero la razón por la que se están escribiendo esas líneas tiene que ver con el número de veces que se renderiza nuestro componente y tenemos que entender que internamente **React Query** está haciendo la llamada externa a la API y que dicha llamada solamente la realiza una vez, cosa que podremos comprobar en la pestaña *Network* dentro de las herramientas para desarrolladores. De hecho, podemos observar que las cuatro primeras veces que se renderiza el coponente no se ha obtenido la información de la API (razón por la que se renderiza el mensaje `undefined`) mientras que en las dos últimas ya se tiene la respuesta y por lo tanto se muestra por la consola.

Vamos ahora a estudiar la respuesta de la API. Como podemos ver tenemos un atributo `count` que nos viene a indicar el número total de planetas para los cuales se dispone de información, el atributo `result` que está formado por un array con la información de los planetas que se ha seleccionado siendo el número de elementos del mismo 10 lo que es una cantidad inferior al número total de planetas disponibles. De hecho podemos deducir que la consulta de los planetas está paginada y que cada vez que solitamos una de las páginas de resultados se nos estarán devolviendo como mucho 10 resultados de tal manera que si queremos obtener la información de una página anterior o siguiente tendríamos que hacer uso de los atributos `previous` y `next` respectivamente. Más adelante veremos cómo podemos trabajar con la información paginada.

¿Y qué sucede con el atributo `status` del objeto que nos retorna `useQuery`? En este caso vamos a escribir el valor del mismo no por la consola de JavaScript si no que lo vamos a hacer como un elemento más del DOM asociado al componente. Así, escribimos lo siguiente:

```javascript
const Planets = () => {
  const { data, status } = useQuery('planets', fetchPlanets)
  return (
    <div>
      <h2>Planets</h2>
      <p>{ status }</p>
    </div>
```

Si ahora recargamos la página en nuestro navegador veríamos como inicialmente el valor de esta variable sería `loading` para finalmente pasar a `success` una vez se tiene la respuesta de la API. ¿Y qué pasa en el caso de que suceda un error derivado, por ejemplo, de que hayamos escrito mál la url de la API? La respuesta es que **React Query** va a tratar de traerse la información durante al menos tres veces antes de indicar que se ha producido el error (estando la respuesta en el estado `loading`) y únicamente cuando se produce la tercera con error cuando el estado pasará a ser `error`.

Con esto en mente parece bastante evidente que dentro del código JSX del componente lo que deberíamos hacer es ver el valor que tienen asociado el atributo `status` del objeto respuesta de `useQuery` y en función del mismo mostrar un mensaje u otro al usuario. Así si el estado es `loading` podríamos mostrar un mensaje indicándolo o un *spinner*, si es `error` un mensaje de error informando del problema y si es `success` mostrar la información con la respuesta al usuario:

```javascript
const Planets = () => {
  const { data, status } = useQuery('planets', fetchPlanets)
  return (
    <div>
      <h2>Planets</h2>
      { status === 'loading' && <div>Loading data...</div> }
      { status === 'error' && <div>Error fetching data</div> }
      { status === 'success' && ... }
    </div>
```

¿Qué tendríamos que mostrar en el caso de que el `status` fuese `success` pues la información de cada uno de los planetas que nos ha devuelto la API y que es propia de esta consulta. En concreto lo que vamos a hacer es mostrar el nombre de cada uno de los planetas que nos retorna la consulta. Sabiendo que, en nuestro ejemplo concreto, dentro del objeto `data` que retorna `useQuery` en su atributo `results` tenemos un array donde cada uno de los elementos es un objeto con la información de un planeta podremos escribir lo siguiente:

```javascript
const Planets = () => {
  const { data, status } = useQuery('planets', fetchPlanets)
  return (
    <div>
      <h2>Planets</h2>
      { status === 'loading' && <div>Loading data...</div> }
      { status === 'error' && <div>Error fetching data</div> }
      { status === 'success' && (
          <div>
            { data.results.map(planet => <div>{ planet.name }</div>) }
          </div>
        )
      }
```

Tras esta modificación si ahora volvemos a recargar la página en nuestro navegador veremos que se escribirán los nombres de los 10 primeros planetas con los que nos estará respondiendo la API. Ahora bien, en la consola de JavaScript veremos que **React** nos estará emitiendo un warning indicándonos que cada uno de los elementos `<div>` que engloban los nombres de los planetas deberían poseer un atributo `key` con el fin de que se pueda trazar su estado de forma independiente dentro del Shadow DOM. No obstante, en este punto no vamos a preocuparnos por ello ya que vamos a crear un nuevo componente `Planet` que será el encargado de mostrar la información de cada uno de los planetas de la lista:

```javascript
import React from 'react'

const Planet = ({ planet }) => {
  return (
    <div className='card'>
      <h3>{ planet.name }</h3>
      <p>Population - { planet.population }</p>
      <p>Terrain - { planet.terrain }</p>
    </div>
  )
}

export default Planet
```

Sin entrar en los detalles de cómo funciona este nuevo componente vamos a utilizarlo para renderizar la información de cada uno de los planetas que nos ha devuelto la llamada a la API además de añadirle la prop `key` de tal manera que así **React** pueda controlar cada una de las instancias del componente dentro del Shadow DOM:

```javascript
const Planets = () => {
  const { data, status } = useQuery('planets', fetchPlanets)
  return (
    <div>
      <h2>Planets</h2>
      { status === 'loading' && <div>Loading data...</div> }
      { status === 'error' && <div>Error fetching data</div> }
      { status === 'success' && (
          <div>
            {
              data.results.map(planet => (
                <Planet key={ planet.name } planet={ planet } />
              ))
            }
          </div>
        )
      }
```

El siguiente detalle que tendríamos que realizar en nuestra aplicación sería crear la clase CSS `card` para mostrar de una forma más limpia la información de cada uno de los planetas:

```css
.card {
  padding: 8px 16px;
  background: #1b1b1b;
  margin: 16px 0;
  border-radius: 20px;
}
.card p {
  margin: 6px 0;
  color: #999;
}
.card h3 {
  margin: 10px 0;
  color: #ffff57;
}
```

El código completo del componente `Planets` una vez hechas todas las modificaciones que hemos estado narrando hasta ahora es el que se muestra a continuación:

```javascript
import React from 'react'
import { useQuery } from 'react-query'
import Planet from './Planet'

const fetchPlanets = async () => {
  const res = await fetch('https://swapi.dev/api/planets/')
  return res.json()
}

const Planets = () => {
  const { data, status } = useQuery('planets', fetchPlanets)
  return (
    <div>
      <h2>Planets</h2>
      { status === 'loading' && <div>Loading data...</div> }
      { status === 'error' && <div>Error fetching data</div> }
      { status === 'success' && (
          <div>
            {
              data.results.map(planet => (
                <Planet key={ planet.name } planet={ planet } />
              ))
            }
          </div>
        )
      }
    </div>
  )
}

export default Planets
```

Siguiendo esta misma explicación vamos a ver cómo podemos obtener la información de los personajes de Star Wars haciendo las modificaciones necesarias en el componente `People` sin extendernos demasiado en la explicación porque es similar a todo lo que hemos comentado hasta ahora:

```javascript
import React from 'react'
import { useQuery } from 'react-query'
import People from './People'

const fetchPeople = async () => {
  const res = await fetch('https://swapi.dev/api/people/')
  return res.json()
}

const People = () => {
  const { data, status } = useQuery('people', fetchPeople)
  return (
    <div>
      <div>
      <h2>People</h2>
      { status === 'loading' && <div>Loading data...</div> }
      { status === 'error' && <div>Error fetching data</div> }
      { status === 'success' && (
          <div>
            {
              data.results.map(people => (
                <People key={ people.name } people={ people } />
              ))
            }
          </div>
        )
      }
    </div>
    </div>
  )
}

export default People
```

Y el código del componente `People` será el que se muestra a continuación:

```javascript
import React from 'react'

const Person = ({ person }) => {
  return (
    <div className='card'>
      <h3>{ person.name }</h3>
      <p>Gender - { person.gender }</p>
      <p>Birth year - { person.birth_year }</p>
    </div>
  )
}

export default Person
```

Con todo esto habremos construido las llamadas asíncronas para cada uno de los componentes de nuestra aplicación de ejemplo. Lo realmente intenresante aquí es que la primera vez que accedamos a cada uno de ellos para obtener la información de los planetas y de los personajes en la página que se mostrará se podrá ver el mensaje de carga de la información (mensaje asociado al estado `loading`) pero las siguientes veces que lo hagamos dicho mensaje no se mostrará porque la información asociada estará cacheada lo que dará una sensación de mayor fluidez y rendimiento dentro de nuestra aplicación.

No obstante no deberemos olvidarnos de que **React Query** seguirá consultando la API por nosotros de tal manera que siempre que intentará tener la información actualizada
lo que en última instancia se traduce en una mejora del rendimiento de nuestras aplicaciones.

Ahora bien, **React Query** es mucho maś de lo que acabamos de ver en este punto por lo que es necesario seguir profundizando en la librería para ver todas las posibilidades que nos está ofreciendo cosa que haremos en los siguientes puntos.
