# `Array.prototype.flatMap`

En este punto vamos a ver para qué sirve y cómo podemos trabajar con el método `flatMap` que tenemos a nuestra disposición cuando trabajamos con arrays en JavaScript.

Anteriormente [hemos visto cómo funciona el método `flat`](https://github.com/DevJoseManuel/js-tutorials/blob/master/javascript/arrays/10_flat.md) que tenemos a nuestra disposición cuando en JavaScript estamos trabajando con arrays (recordemos que lo que hace es coger un array multidimensional y aplanarlo *flat* hasta lograr un array con menos dimensiones que el array original pero respetando el orden de los elementos).

Lo que tenemos que entender es que el objetivo de `flatMap` es el mismo que el de `flat` pero ofreciéndonos la posibilidad de realizar algún tipo de operación sobre cada uno de los elementos del array antes de que dicho elemento sea aplanado.

---
**Tip:** para entender mejor cómo funciona internamente `flatMap` podemos pensar en que es exactamente equivalente a si sobre el array en el que lo estamos aplicando previamente utilizamos el método `map` para transformar cada uno de sus elementos y sobre el array que obtenemos como resultado invocamos al método `flat` pasándole como parámetro el valor 1 indicando de esta manera que queremos aplanar una dimensión en el array (o sin indicar dicho valor porque recordemos que 1 es el valor por defecto como parámetro del método `flat`). En otras palabras, `flatMap` sobre un array `array` sería equivalente a:

```javascript
array.map(() => {}).flat(1)
```
----

Vamos a intentar explicar cómo funciona este método mediante un ejemplo. Supongamos que tenemos definido un array que contiene una lista de películas y que los elementos que lo forman han podido proceder de distintos orígenes de datos lo que hace que el formato de cada uno de los elementos del array no sea homogéneo:

```javascript
let movies = [
  'Dog Soldiers',
  ['In Brudges', 'From Paris with Love', 'Layer Cake'],
  'The Big Lebowski',
  '',
  '    ',
  'Memento, The Platform,Fight Club, ',
  'Hotel Rwanda, Moon, Under the Skin',
  'Lady Bird',
  ['Platoon', 'Wall-E']
]
```

Vemos que alguno de los elementos del array simplemente es un string con el nombre de la película, otros son a su vez un array donde cada uno de sus elementos es el título de una película, tenemos también elementos que simplemente son cadenas vacías (ya tengan espacios en blanco o no) e incluso tenemos elementos que son string pero que a su vez contienen el título de varias películas todas ellas separadas por comas (sin que esté definido que tras la coma tenga que ir un espacio en blanco o que tras la coma tenga que ir un título).

El objetivo que nosotros perseguiremos será conseguir de alguna manera (como es obvio con el uso del método `flatMap` que nos proporcionan los arrays) que el array con la los títulos de las películas que hemos mostrado anteriormente se convierta en una array donde cada uno de los elementos sea únicamente el título de una película lo que nos ayudará posteriormente a que lo podamos procesar de forma mucho más sencilla.

Lo primero que tenemos que saber es que `flatMap` espera recibir como parámetro una función la cual a su vez recibirá como parámetro el elemneto del array que se está procesando. Así la declaración de este método sería algo como lo siguiente:

```javascript
let arr = movies.flatMap(entry => {})
```

Teniendo esto en cuenta dentro de la función que establecemos como parámetro para `flatMap` lo primero que vamos a hacer es comprobar en primer lugar si se trata a su vez de un array o no. ¿Cómo podemos hacer esto? Pues simplemente invocando al método `isArray` del objeto `Array` que nos proporciona JavaScript:

```javascript
let arr = movies.flatMap(entry => {
  if (Array.isArray(entry)) {}
})
```

¿Y qué tendremos que hacer en el caso de que el elemento sea un array? Pues simplemente retornar ese mismo array ya que tenemos que recordar que posteriormente `flatMap` se encargará de aplanar un nivel el array de salida. Por lo tanto escribiremos lo siguiente:

```javascript
let arr = movies.flatMap(entry => {
  if (Array.isArray(entry)) {
    return entry
  }
})
```

Vamos a añadir tras la declaración de `arr` una sentencia del tipo `console.log` para ver qué es lo que estaremos obteniendo como resultado cuando se aplique el método:

```javascript
let arr = movies.flatMap(entry => {
  if (Array.isArray(entry)) {
    return entry
  }
})
console.log(arr)
```

Si ahora guardamos el código anterior y lo ejecutamos desde la terminal de comandos gracias a **Node.js** o desde la Consola de JavaScript dentro de las herramientas para desarrolladores que tenemos a nuestra disposición en un navegador web obtendremos:

```javascript
[
  undefined,
  "In Brudges",
  "From Paris with Love",
  "Layer Cake",
  undefined,
  undefined,
  undefined,
  undefined,
  undefined,
  undefined,
  "Platoon",
  "Wall-E"
]
```

Vemos que en el array de salida como resultado de la ejecución de `flatMap` únicamente tendremos la información de todos los elementos que en el array original eran a su vez un array y además se habrá aplicado sobre dichos arrays originales el método `flat` aplanándolos un nivel.

Así el elemento situado en la posición 1 del array original era el array:

```javascript
['In Brudges', 'From Paris with Love', 'Layer Cake']
```

y tras ser transformado en sí mismo (es decir, que no se aplica transformación) al final de la ejecución del método `flatMap` es aplanado un nivel lo que hace que se haya transformado en tres elementos en el array de salida:

```javascript
[
  undefined,
  "In Brudges",
  "From Paris with Love",
  "Layer Cake",
  ...
```

Esto mismo sucde con el elemento que ocupa la posición 9 en el array del que partimos:

```javascript
['Platoon', 'Wall-E']
```

que se transfoma en dos elementos en el array de salida:

```javascript
  ...
  "Platoon",
  "Wall-E"
]
```

El resto de elementos del array serán `undefined` porque la función que estamos haciendo no define qué es lo que se ha de hacer con ellos y JavaScript lo que está haciendo es introducir un retorno explícito que siempre es `undefined`.

Vamos a seguir trabajando ahora en cómo se ha de comportar la función que pasamos como parámetro al método `flatMap` pero esta vez centrándonos en qué es lo que tenemos que hacer cuando nos enfrentamos a un string. Esto se traduce en que vamos a tener que hacer una comprobación del tipo `typeof` para asegurarnos que estamos ante una entrada que contiene un string:

```javascript
let arr = movies.flatMap(entry => {
  if (Array.isArray(entry)) {
    return entry
  }
  if (typeof entry === 'string') {
    // resto de nuestro código
  }
})
console.log(arr)
```

En primer lugar vamos a ver qué es lo que tenemos que hacer ante todas aquellas entradas que contienen una cadena vacía por lo que lo primero que tenemos ver es ¿cómo vamos a determinar que un elemento es vacío? La respuesta es simplemente utilizando el método `trim` que nos proporciona la clase `String` (que elimina todos los espacios en blanco al principio y final de un string) y luego viendo si el valor que obtenemos es la cadena vacía.

```javascript
let arr = movies.flatMap(entry => {
  if (Array.isArray(entry)) {
    return entry
  }
  if (typeof entry === 'string') {
    if (entry.trim() === '') {
      // Estamos ante una cadena vacía
    }
  }
})
console.log(arr)
```

¿Qué deberemos retornar cuando estamos ante la cadena vacía? La respuesta es un array vacío ya que de esta manera cuando se vaya a aplanar este array simplemente no tendrá elementos y por lo tanto no ocupará ninguna posición en el array de salida. Por lo tanto escribiremos:

```javascript
let arr = movies.flatMap(entry => {
  if (Array.isArray(entry)) {
    return entry
  }
  if (typeof entry === 'string') {
    if (entry.trim() === '') {
      return []
    }
  }
})
console.log(arr)
```

La forma en la que esta implementación logra el objetivo que perseguimos tiene en cuenta que el método `map` sobre un array lo que hace es retornar el mismo número de elementos que el array original sobre el que se está implementando por lo que es necesario devolver un array vacío. ¿Y cómo se comporta `flat` cuando se encuentra un array vacío? Pues básicamente viene a decir que al tratarse de una array vacío no hay nada que se pueda aplanar y por lo tanto se podrá eliminar el elemento del array de salida.

---
**Nota:** al invocar el método `flat` lo que se consigue es eliminar todos aquellos elementos que fuesen el array vacío, es decir, que es como si se estuviera recorriendo el array gracias al método `filter` comprobando que si el elemento es el array vacío no se incluirá en el array que será retornado con `filter`.

---

Si ahora guardamos la modificación y volvemos a ejecutar el código veremos que el número de elementos del array que se ha obtenido como resultado tiene dos elementos menos que el resultado que hemos obtenido en la implementación derivado de los dos elementos que están recogidos en el array original que contienen sendas cadenas vacías:

```javascript
[
  undefined,
  "In Brudges",
  "From Paris with Love",
  "Layer Cake",
  undefined,
  undefined,
  undefined,
  undefined,
  "Platoon",
  "Wall-E"
]
```

El siguiente paso consistirá en tener en cuenta todos aquellos elementos que son un string pero que pueden contener más de un título de una película separados por comas ya que lo que queremos conseguir es que cada una de ellos se trate como un título independiente siendo un nuevo elemento del array que se retorna. Para ello la forma más sencilla de conseguir separar el string con los títulos de las películas es utilizar el método `split` al cual le pasamos como parámetro una coma `,` sabiendo que este método retornará un array donde cada uno de los elementos está formado por una porción del string hasta la coma.

Esto que es complicado de explicar con palabras es muy simple de entender si lo vemos con un ejemplo. Si aplicamos el métod `split` sobre el siguiente elemento de nuestro array con las películas:

```javascript
'Memento, The Platform,Fight Club, '
```

Obtendríamos el siguiente array:

```javascript
[
  "Memento",
  " The Platform",
  "Fight Club",
  " "
]
```

Es decir, que el string original se transforma en un array formado por cuatro elementos (tenemos tres comas `,` en nuestro string original) pero es importante observar, en primer lugar, que los espacios en blanco que pueda haber detrás de la coma se conservan y, en segundo lugar, que en el caso de la última coma al haber un espacio en blanco se convertirá en un elemento del array de pleno derecho lo que pasa es que su valor será la cadena vacía.

Ahora modificamos el código de nuestra función para que incluya la invocación del método `split`:

```javascript
let arr = movies.flatMap(entry => {
  if (Array.isArray(entry)) {
    return entry
  }
  if (typeof entry === 'string') {
    if (entry.trim() === '') {
      return []
    } else {
      return entry.split(',')
    }
  }
})
console.log(arr)
```

Algún apunte adicional que tenemos que conocer para entender nuestra implementación. ¿Qué sucede cuando el meodo `split` se llama sobre un string que no posee el caracter que se está buscando? En nuestro caso ¿qué sucede cuando se llama al método `split` sobre un string que representa únicamente el título de una película, es decir, que no contiene una coma `,`? La respuesta es que retornará un array con una única posición que posee únicamente un elemento formado por ese título de la película.

Si ahora volvemos a ejecutar el código con el que estamos trabajando la salida que obtendremos es la siguiente:

```javascript
[
  "Dog Soldiers",
  "In Brudges",
  "From Paris with Love",
  "Layer Cake",
  "The Big Lebowski",
  "Memento",
  " The Platform",
  "Fight Club",
  " ",
  "Hotel Rwanda",
  " Moon",
  " Under the Skin",
  "Lady Bird",
  "Platoon",
  "Wall-E"
]
```

Vemos que casi hemos conseguido nuestro propósito pero todavía existen un par de detalles que tenemos que tratar antes de dar por finalizado nuestro objetivo y es que, como podemos ver, tenemos títulos de pelóculas que presentan espacios en blanco al principio y además que tenemos un elemento del array que está vacío. La forma en la que podemos solucionarlo es gracia a la invocación del método `map` al array que nos retorna el método `split` con el fin de ejecutar el método `trim` sobre cada uno de los elementos y así eliminar los posibles espacios en blanco y posteriormente llamar al método `filter` para quedarnos con aquellos elementos que no son la cadena vacía:

```javascript
let arr = movies.flatMap(entry => {
  if (Array.isArray(entry)) {
    return entry
  }
  if (typeof entry === 'string') {
    if (entry.trim() === '') {
      return []
    } else {
      return entry.split(',')
          .map(item => item.trim())
          .filter(item => item !== '')
    }
  }
})
console.log(arr)
```

Si ahora guardamos esta modificación y volvemos a ejecutar nuestro código obtendremos la siguiente salida:

```javascript
[
  "Dog Soldiers",
  "In Brudges",
  "From Paris with Love",
  "Layer Cake",
  "The Big Lebowski",
  "Memento",
  "The Platform",
  "Fight Club",
  "Hotel Rwanda",
  "Moon",
  "Under the Skin",
  "Lady Bird",
  "Platoon",
  "Wall-E"
]
```

Con esta última ejecución ya hemos logrado el objetivo que perseguíamos cuando planteábamos el ejemplo pero para que el código pueda tener en consideración todas las posibilidades habría que ver qué hacer cuando el resultado sea de tipo number, boolean, etc. Nosotros vamos a simplificar al máximo el desarrollo haciendo que lo que retorne un array vacío en el caso de que se encuentre un tipo de datos que no sea un string con el fin de que dicho elemento del array que se retorne sea eliminado, como hemos explicado anteriormente:

```javascript
let arr = movies.flatMap(entry => {
  if (Array.isArray(entry)) {
    return entry
  }
  if (typeof entry === 'string') {
    if (entry.trim() === '') {
      return []
    } else {
      return entry.split(',')
          .map(item => item.trim())
          .filter(item => item !== '')
    }
  }
  return []
})
console.log(arr)
```

A modo de conclusión simplemente decir que `flatMap` es una combinación de los métodos `map` (aplicando una función sobre cada uno de los elementos del array) para posteriormente aplicar el método `flat` con un nivel de profundidad.

---
**Nota:** se recomienda leer la [documentación recogida en la MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap) para todos aquellos lectores que quieran ampliar conocimientos sobre este método.

---
