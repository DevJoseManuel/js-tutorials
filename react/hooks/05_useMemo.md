# `useMemo`

En este punto vamos a estudiar el hook `useMemo` explicando cómo funciona y cuándo deberíamos utilizarlo. Para ello vamos a basarnos en el siguiente componente de React:

```javascript
import React, { useState } from 'react'

export default function App() {
  const [number, setNumber] = useState(0)
  const [dark, setDark] = useState(false)
  const doubleNumber = slowFunction(number)
  const themeStyle = {
    backgroundColor: dark ? 'black' : 'white',
    color: dark ? 'white' : 'black'
  }

  return (
    <>
      <input 
        type='number'
        value={ number }
        onChange={ e => setNumber(parseInt(e.target.value)) }
      >
      <button onClick={ e => setDark(prevDark => !prevDark) }>
        Change Theme
      </button>
      <div style={ themeStyle }>
        { doubleNumber }
      </div>
    </input>
  )
}

function slowFunction(num) {
  console.log('Calling Slow Function`)
  for (int i = 0; i <= 1000000000; i++>) {}
  return num * 2
}
```

Vamos a hacer una descripción rápida de cómo está construido este componente para poder entender la problemática que queremos solucionar con el uso del hook `useMemo`. Como podemos observar dentro del componente estamos definiendo dos variables dentro del estado (la variable `number` y la variable `dark`) gracias al uso del hook `useState`. 

A continuación estamos definiendo la variable `doubleNumber` que contendrá el resultado de la invocación de la función `slowFunction` a la que se le pasa como parámetro el valor de la variable del estado `number`. Esta función lo único que hace es devolver el valor que recibe como parámetro multiplicado por 2 con la particularidad que dentro de la misma se está ejecutando un bucle que implica un total de 1.000.000.000 iteraciones dentro de las cuáles no se hace nada, lo que se traduce en una gran cantidad de tiempo de computación lo que hace que nuestro componente tarde más tiempo en renderizarse.

---
**Nota:** lo que estamos tratando de simular con nuestra función `slowFunction` es una función dentro de una aplicación real la cual implica un gran tiempo de computación para obtener su resultado. Simplemente con este bucle tan grande tratamos de reflejar un conjunto de instrucciones costosas en lo que respecta al tiempo de CPU que precisan para terminar.

---

Tras la definición y asignación de la variable `doubleNumber` se define la variable `theStyle` asignándola a un objeto en el que se definen las propiedades CSS que posteriormente querremos aplicar en el atributo `style` de alguno de los elementos del código JSX de nuestro componente. Los valores que se le asignarán a cada uno de estos atributos dependerán del valor de la variable del estado `dark` de tal manera que si tiene el valor `true` el color de fondo del elemento será `black` y el texto `white` mientras que si es `false` el color de fondo será `white` y el color del texto `black` (el color de fondo lo asignamos con la propiedad CSS `backgroundColor` y el color del texto con la propiedad CSS `color`). Con esto lo que pretendermos es crear un rudimentario sistema de temas CSS para nuestro componente.

Tras la declaración de todas las variables nos encontramos con el código JSX que renderizará nuestro componente. Vemos que en primer lugar renderiza un campo de texto que admitirá como valor un número (por el hecho de especificar el campo `<input>` definido como `type='number'`) y que siempre que se cambia este valor se dispará el evento `onChange` haciendo que se establezca el valor de la variable del estado `number` y por lo tanto renderizando de nuevo el componente.

---
**Nota:** si se quiere obtener más información acerca de cómo funcionan los campos de texto `<input>` que son definidos como `type='number'` se comienda [leer la documentación de la MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/number) al respecto.

---

Tras el campo `<input>` se renderiza un botón que al ser pulsado simplemente lo que se hará será cambiar el valor de la variable de estado `dark` pasando de `true` a `false` y viceversa. Al tratarse de una variable del estado del componente lo que provocará cada una de las pulsaciones del botón será un nuevo renderizado del componente.

Por último se muestra un elemento `<div>` al que se aplicará como estilo CSS el valor que tiene asociado la variable `themeStyles` y cuyo contenido será el valor de la variable `doubleNumber` que contiene el resultado de la invocación de la función `slowFunction` recibiendo como parámetro el valor de la variable de estado `number`.

Inicialmente no existe ningún tipo de problema con el código de nuestro componente ya que funcionará bien y cumplirá el cometido para el que ha sido creado pero, si nos paramos a pensar un poco vemos que la función `slowFunction` se ejecutará en cada nuevo renderizado de nuestro componente pese a que no haya cambiado el valor de la variable de estado `number`. ¿Y cómo puede suceder esto? Pues pensemos en el botón etiquetado como *Change Theme*; cuando se pulse sobre el mismo se cambiará el valor de la variable de estado `dark` lo que provocará un nuevo renderizado del componente y por lo tanto una nueva invocación a la función `slowFunction` con el mismo valor que tenía la variable de estado `number` pese a que ya sabemos el resultado de su invocación, lo cual no es del todo eficiente teniendo en cuenta la gran cantidad de tiempo de CPU que requiere la función para devolver su resultado.

Por lo tanto tenemos tres razones por las que se volverá a ejecutar la función `slowFunction` todas ellas relacionadas con las causas que derivan en que nuestro componente tiene que volver a ser renderizado:

* porque cambia el valor de la variable de estado `number`. 
* porque cambia el valor de la variable de estado `dark`.
* porque otro de los componentes de nuestra aplicación provoca que este componente `App` deba ser renderizado de nuevo.

Ahora bien, ¿en cuál de los tres casos parece correcto que tenga que volver a ejecutarse la función `slowFunction`? La respuesta correcta es que únicamente debería volverse a ejecutar cada vez que cambiase el valor de la variable de estado `number` ya que la salida de la funcion (el valor que retorna) depende en su totalidad de este valor de entrada.

Por lo tanto estamos ante un problema de rendimiento que puede presentearse de forma bastante habitual en nuestras aplicaciones cuando estamos trabajando con funciones que conllevan una gran cantidad de tiempo CPU para poder finalizar su trabajo. Afortunadamente React nos ofrece el hook `useMemo` para solventarlo.

Lo primero que tendremos que hacer para poder utilizar este hook dentro de nuestro componente es importarlo, por lo que escribiremos lo siguiente:

```javascript
import React, { useState, useMemo } from 'react'
```

En este contexto la palabra *memo* viene de un conceto de JavaScript que se denomina **memoization** que básicamente consiste en mantener cacheado el valor que retorna una función ante unos mismos datos de entrada de tal manera que no sea necesario volverlo a recalcular cada vez que se invoque a la dicha función con los mismos parámetros de entrada. En nuestro ejemplo la función `slowFunction` siempre nos va devolver el mismo resultado de salida ante el mismo parámetro de entrada por lo que podríamos ahorrarnos en determinadas ocasiones su ejecución (que recordemos que es costosa en lo que repecta a tiempo de CPU) lo que incrementará el rendimiento de nuestro componente.

Ahora bien ¿cómo funciona el hook `useMemo`? La respuesta nuevamente vuelve a ser sencilla. Siempre tenemos que pensar en al función cuyo resultado queremos cachear (en nuestro caso `slowFunction`) y hacer que en el lugar en el que se estaba invocando ahora se invoque al hook `useMemo` pasándole como parámetro una nueva función en la que se ejecutará la función a cachear. Esto que parece complejo es muy simple de ver en nuestro código de ejemplo. El lugar en el que estamos invocando a nuestra función es el siguiente:

```javascript
const doubleNumber = slowFunction(number)
```

Pues este código lo tenemos que sustituir por una llamada al hook `useMemo` y en al función que especificaremos como parámetro ejecutaremos la invocación a la función `slowFunction` como hacíamos anteriormente, retornando el resultado de dicha función, como el resutlado de la función que se ha pasado como parámetro al hook `useMemo`. Es decir, que tendríamos:

```javascript
const doubleNumber = useMemo(() => {
  return slowFunction(number)
})
```

Ahora bien, con esta definición la invocación de la función que pasamos a `useMemo` se ejecutará con cada nuevo reenderizado de nuestro componente por lo que tenemos que especificar como segundo parámetro de la invocación un array donde cada uno de los elementos serán las variables cuyo valor ha de cambiar para que se tenga que volver a ejecutar la función. Como en nuestro caso, el resultado de la función `slowFunction` dependerá del valor de la variable del estado `number` indicaremos como dependencia para la ejecución los cambios que se produzcan sobre dicha variable:

```javascript
const doubleNumber = useMemo(() => {
    return slowFunction(number)
  }, [number]
)
```

Así la forma en la que podemos leer el código anterior sería la siguiente: únicamente cada vez que cambie el valor de la variable `number` se ha de ejecutar el código de la función que se ha establecido en el primer parámetro de la invocación del hook `useMemo` y no solamente eso, sino que se ha de cachear el resultado de su ejecución para que posteriores llamadas al código sin que cambie el valro de dicha variable se tome la salida de la función como el valor que ha sido cacheado.

¿Con esto qué estaremos consiguiendo? Pues que cada vez que cambiemos el valor que introducimos en nuestro cuadro de texto se estará modificando el valor de la variable de estado `number` y por lo tanto se producirá un nuevo renderizado de nuestro componente en el que se volverá a invocar a la función `slowFunction` como consecuencia de un cambio en dicho valor. Ahora bien, cuando pulsemos sobre el botón *Change Theme* lo que ocurrirá es que cambiará el valor de la variable de estado `dark` y en el nuevo ciclo de renderizado no se invocará a la función `slowFunction` puesto que la variable del estado `number` no ha cambiado. Y no solamente eso sino que además en todos los lugares donde se estuviese invocando a dicha función se tomará como resultado el valor de salida que se ha obtenido en su última invocación.

La pregunto que nos puede surgir ahora es ¿esta técnica parece genial así que por qué no hemos de usarla en todas las partes de nuestra aplicación? La respuesta es que siempre que nos enfrentemos ante un problema de rendimiento en nuestra aplicación tendremos que estudiar profundamente cuál es la naturaleza del mismo antes de decidir que tenemos que usar un hook como `useMemo`. Así, en nuestro ejemplo, el problema de rendimiento se produce como consecuencia de que el resultado de la ejecución de la función `slowFunction` la estamos guardando en una variable en cada uno de los renderizados de nuestro componente pero ¿sería necesario defenir esa variable (y obtener su valor) en cada uno de estos renderizados? ¿No sería más correcto comprobar si el valor de la variable de estado `number` ha cambiar entre un ciclo de renderizado y el anterior y únicamente en el caso de que así fuese invocar a la función `slowMotion` evitando el tener que utilizar `useMemo`?

La recomendación que se hace es que utilicemos `useMemo` solamente cuando el tiempo de ejecución de las funciones que vayamos a ejecutar sean muy grandes y hagan que nuestros componentes se rendericen de una forma muy lenta. En cualquier otro caso se debería tratar de encontrar una solución mejor utilizando otro tipo de soluciones.

No obstante existe otro caso de uso en el que la utilización del hook `useMemo` se vuelve realmente intenresante y es algo que se conoce como **referential equality**. Este mecanismo se basa en el hecho de que en JavaScript cada vez que se va a comparar si dos objetos o arrays son los mismos lo que se hace es comparar las referencias que en memoria están asociadas a las variables a comparar para ver si apuntan a las mismas direcciones de memoria.

Vamos a verlo con un ejemplo. Tomando como ejemplo el objeto que sirve para definir los atributos CSS que se aplicarán a los estilos del `<div>` que muestra el valor de la ejecución de la función `slowFunction`:

```javascript
const themeStyle = {
  backgroundColor: dark ? 'black' : 'white',
  color: dark ? 'white' : 'black'
}
```

Y simplemente lo que hacemos es crear una nueva variable a la que le asignamos un objeto que posee los mismos atributos con los mismos valores:

```javascript
const themeStyle = {
  backgroundColor: dark ? 'black' : 'white',
  color: dark ? 'white' : 'black'
}
const themeStyle2 = {
  backgroundColor: dark ? 'black' : 'white',
  color: dark ? 'white' : 'black'
}
```

En este caso podríamos estar tentados a asegurar que la variable `themeStyle` y `themeStyle2` son la misma ya que ambas hacer referencia a un objeto y dichos objetos poseen los mismos atributos con los mismo valores. Pero no es así, porque en JavaScript lo que tendremos serán dos referenicas a sendos objetos en memoria con la peculiaridad de que ambos objetos tienen los mismos atributos y valores estando situados en posiciones de la memoria diferentes. Por lo tanto podemos concluir que en JavaScript:

```javascript
themeStyle !== themeStyle2
```

Vamos a ver qué supone esto en el caso de que queramos constrolar cuando está cambiando un objeto dentro de nuestro componete. Así pues, vamos a suponer que queremos que por la consola de JavaScript se escriba un mensaje cada vez que se cambie el valor del variable `themeStyle` dentro del código de nuestro componente. Así pues lo primero que vamos a hacer es importar el hook `useEffect`:

```javascript
import React, { useState, useMemo, useEffect } from 'react'
```

Y ahora mediante el uso de `useEffect` vamos a definir la función que queremos que se ejecute cada vez que cambie el valor de la variable `themeStyle` de la siguiente manera:

```javascript
useEffect(() => {
    console.log('Theme Changed')
  }, [themeStyle]
)
```

Si ahora guardamos el código de nuestro componente y volvemos a cargarlo dentro de un navegador web veremos que en la consola de JavaScript se estará escribiendo el mensaje *Theme Chaged* la primera vez que se carga el componente además de hacerlo cada vez que pulsamos sobre el botón *Change Theme* como podríamos esperar que sucedería. 

¿Pero qué ocurre si cambiamos el valor del cuadro de texto `<input>` con un nuevo número? Pues que nuevamente se volverá a escribir por la consola el mensaje *Theme Changed* ya que en el nuevo ciclo de renderizado se volverá a recalcular la variable `themeStyle` creando una nueva posición en la memoria con los mismos valores que tenía anteriormente (antes de cambiar el número en el cuadro de texto) pero a todos los efectos siendo un objeto diferentes del que habia en el ciclo anterior de renderizado (ambos están situados en posiciones de memoria diferentes).

Entonces ¿qué podemos hacer para asegurarnos de que la función que está incluida dentro del hook `useEffect` se ejecute únicamente cuando el objeto `themeStyle` cambia los valores de sus atributos? La respuesta pasa por hacer uso nuevamente del hook `useMemo`. En este caso la función que le pasaremos como parámetro simplemente se encargará de devover el objeto con las propiedades CSS que representan el estilo con el que queremos mostrar el contenido la variable `doubleNumber` y en las dependecias para indicar cuando se deberá ejecutar esta función simplemente estableceremos que ha de hacerse cuando la variable de estado `dark` cambie (es decir, cuando se pulse sobre el botón *Change Theme*):

```javascript
const themeStyle = useMemo(() => {
    return {
      backgroundColor: dark ? 'dark' : 'white',
      color: dark ? 'white' : 'dark'
    }
  }, [dark]
)
```

Si ahora volvemos a grabar el código de nuestro componente y lo recargamos en el navegador observaremos que cada vez que pulsemos sobre el botón *Change Theme* se estará mostrando por la consola de JavaScript el mensaje *Theme Changed* (cosa que esperábamos) mientras que si lo que estamos haciendo es cambiar el valor introducido en el cuado de texto `<input>` no se mostrará el mensaje porque esta modificación no provoca un cambio en la variable de estado `dark` y por lo tanto no provocará que se tenga que volver a recalcular los valores de los atributos del objeto `themeStyle`.

## Resumen

A lo largo de este punto hemos visto los dos casos de uso en los que tiene sentido utilizar el hook `useMemo`:

* el primero de ellos tiene que ver con la ejecución de una función que implica una gran cantidad de tiempo de CPU para finalizar de tal manera que se cachee el resultado de su ejecución para una misma entrada y no sea necesario el tener que volverla a ejecutar cada vez que se renderiza el componente más que cuando sea extrictamente necesario.
* el segundo caso de uso tiene que ver con lo que se conoce como **referencial equality** y que será utilizada para deteminar que la referencia que está asignada a una variable (y que apuntará a un objeto o a un array) es exactamente la misma en cada nuevo renderizado de nuestro componente asegurando que únicamente se deberá actualizar dicha referencia cuando los atributos (o elementos del array) realmente cambian.

## Componente final

El código final del componente con el que hemos estado trabajando en este punto tras realizar todas las modificaciones que hemos explicado a lo largo de este punto es el que se muestra a continuación:

```javascript
import React, { useState, useMemo } from 'react'

export default function App() {
  const [number, setNumber] = useState(0)
  const [dark, setDark] = useState(false)
  const doubleNumber = useMemo(() => {
      return slowFunction(number)
    }, [number]
  )

  const themeStyle = useMemo(() => {
      return {
        backgroundColor: dark ? 'dark' : 'white',
        color: dark ? 'white' : 'dark'
      }
    }, [dark]
  )

  useEffect(() => {
      console.log('Theme Changed')
    }, [themeStyle]
  )

  return (
    <>
      <input 
        type='number'
        value={ number }
        onChange={ e => setNumber(parseInt(e.target.value)) }
      >
      <button onClick={ e => setDark(prevDark => !prevDark) }>
        Change Theme
      </button>
      <div style={ themeStyle }>
        { doubleNumber }
      </div>
    </input>
  )
}

function slowFunction(num) {
  console.log('Calling Slow Function`)
  for (int i = 0; i <= 1000000000; i++>) {}
  return num * 2
}
```
