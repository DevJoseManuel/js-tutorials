# `Array.prototype.flat()`

En este punto vamos a ver para qué sirve y cómo podemos trabajar con el método `flat` que tenemos a nuestra disposición cuando trabajamos con arrays en JavaScript.

La funcionalidad que nos proporciona este método no es otra que la de aplanar (*flat*) un array multidimensional. Esto que es complejo de describir es relativamente fácil de entender si lo hacemos gracias a un ejemplo. Vamos a suponer que tenemos el siguiente array multidimensional:

```javascript
let numbers = [
  1,
  2,
  [3, 4, 5],
  [6, 7],
  8,
  9,
  [
    [10, 11],
    [12, 13]
  ]
]
``` 

Como podemos ver el array `numbers` tiene entradas que contienen valores simples (en concreto los valores que ocupan las posiciones 0, 1, 4 y 5), otros elementos son arrays en sí mismos (los que ocupan las posiciones 3 y 4) e incluso tenemos un elemento que a su vez es una array multidimensional (en este caso el elemento que ocupa la posición 6 que es un array de dos dimensiones).

¿Qué es lo que hará el método `flat` si lo llamamos sobre el array `numbers`? En otras palabras ¿cómo funciona el proceso de *aplanar* un array como el de ejemplo? Vamos a describirlo paso a paso. 

Lo primero que tenemos que entender es que como resultado de la invocación del método `flat` lo que obtendremos será un nuevo array por lo que en nuestra implementación en pseudocódigo declararíamos un array vació para que contenga el resultado así como un índice que indicará qué posición del array de entrada estaríamos procesando:

```javascript
flatten = []
position = 0
```

Ahora es cuando comenzaremos a aplicar las reglas que rigen el *aplanamiento* a cada uno de los elementos de nuestro array de ejemplo. El primero elemento del mismo es un valor simple (en este caso el valor numérico 1) por lo que `flat` lo que hará será simplemente añadirlo al array de salida:

```javascript
flatten = [1]
position = 1
```

El segundo de los elementos del array también es un valor simple por lo que es añadido al array de salida:

```javascript
flatten = [1, 2]
position = 2
```

Cuando vamos a procesar el tercer elemento nos encontramos con que este es a su vez un array por lo que `flat` lo que hace es tomar todos los elementos que forman ese array y uno a uno añadirlos al array de salida. Como en nuestro ejemplo el array que ocupa la posición 2 está formado por los valores 3, 4 y 5 lo que `flat` hará será coger cada uno de estos valores y añadir sin más al array de salida:

```javascript
flatten = [1, 2, 3, 4, 5]
position = 3
```

Cuando ahora vamos a procesar la posición 4 del array de partida nos volvemos a encontrar con un array formado por los números 6 y 7 por lo que `flat` lo que hace es simplemente añadirlos al array de salida:

```javascript
flatten = [1, 2, 3, 4, 5, 6, 7]
position = 4
```

En las posiciones 5 y 6 de nuestro array de entrada nuevamente volvemos a tener valores simples por lo que los añadimos sin más al array de salida:

```javascript
flatten = [1, 2, 3, 4, 5, 6, 7, 8, 9]
position = 6
```

Llegamos a la posición 7 que es donde tenemos como valor un array multidimensional. Aquí es donde tenemos que entender que `flat` lo que hace es coger este array multidimensional y sobre cada uno de los elementos que lo forman lo extraerá y lo situará a continuación en nuestro array de salida. Esto explicado en nuestro ejemplo supone que el array multidimensional que tenemos es:

```javascript
[[10, 11], [12, 13]]
```

y lo que hará `flat` será extraer el primer elemento (el array que está formado por los elementos 10 y 11) e insertarlos en el array de salida pero como un array reduciendo de esta manera una dimensión del array. Así, tras procesar el primero de los elementos tendríamos:

```javascript
flatten = [1, 2, 3, 4, 5, 6, 7, 8, 9, [10, 11]]
position = 6
```

Al procesar el segundo nuevamente nos encontramos con un array y `flat` simplemente lo que hace es añadirlo al array de salida:

```javascript
flatten = [1, 2, 3, 4, 5, 6, 7, 8, 9, [10, 11], [12, 13]]
position = 6
```

Esto que hemos explicado en teoría a través de un pseodocódigo es muy sencillo de ver si ahora escribimos en nuestro código lo siguiente:

```javascript
let arr = numbers.flat()
console.log(arr)
```

Si ahora ejecutamos nuestro código desde la terminal de comandos utilizando **Node.js** o bien desde la consola de JavaScript dentro de la las herramientas para adminitradores obtendremos el siguiente resultado:

```console
[1, 2, 3, 4, 5, 6, 7, 8, 9, [10, 11], [12, 13]]
```

Este comportamiento que acabamos de describir es el que la implementación del método `flat` nos ofrece por defecto pero es importante que conozcamos que `flat` puede recibir un parámetro numérico el cual servirá para identificar el nivel de aplanamiento que queremos lograr siendo este valor 1 por defecto.

Pero ¿qué es esto del nivel de aplanamiento? Pues simplemente indicar cómo queremos que actúe `flat` en el caso de que si se encuentra con una array en un determinado nivel de elementos tiene que aplicarse? Pero antes de nada ¿qué es eso del nivel? Pues podemos pensar en ello como la dimensión del array hasta la que queremos que actúe el método. ¿Lioso, verdad? Vamos a verlo en nuestro ejemplo:

En nuestro caso cuando hemos llamado a `flat` sobre el array `numbers` sin indicar un nivel de aplanamiento lo que estamos indicando es que queremos que todos los elementos que sean a su vez un array se aplanen también sin importar del tipo de que se trate. Esto se ha traducido en que:

* los elementos que eran valores numéricos se han copiado tal cual en el array de salida.
* los elementos que eran un array simple (unidimensional) han sido aplanados y añadidos al array de salida.
* el elemento que era un array bidimensional ha sido aplanado una vez reduciendo su dimensión en una unidad. Es decir, que unicialmente teníamos un array bidimensional formado por dos elementos y ahora lo que hemos conseguido son dos arrays unidimensionales donde el primero de ellos se corresponde con el primer elemento del array bidimensional original y el segundo con el segundo elemento del array bidimensional.

Ahora bien, ¿qué resultado obtendríamos si ahora indicamos que el nivel de aplanamiento que queremos aplicar sea de 2 en lugar de 1 (valor por defecto)? Simplemente escribiríamos lo siguiente:

```javascript
let arr = numbers.flat(2)
console.log(arr)
```

Lo que tenemos que entender es que al indicar el valor 2 como nivel de aplanamiento lo que se estaría haciendo es ejecutar el método `flat` una vez obteniendo la siguiente salida:

```javascript
[1, 2, 3, 4, 5, 6, 7, 8, 9, [10, 11], [12, 13]]
```

Y sobre ella volve a ejecutar el método `flat` para obtener el resultado de la invocación del método. Como ya sabemos cómo funciona el método `flat` aplicándolo al array anterior nos dejaría lo siguiente:

```javascript
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]
```

Si ahora volvemos a ejecutar este código ya sea desde la terminal de comandos de nuestro sistema o desde la consola de JavaScript dentro de las herramientas para desarrolladores de un navegador web obtendríamos:

```console
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]
```

Lo que hemos conseguido ha sido transformar nuestro array original de apartida en un array donde todos sus elementos son valores simples.

A modo de conclusión simplemente decir que `flat` es un método que los arrays de JavaScript nos ofrecen para reducir el número de dimensiones que lo forman conservando los elementos y el orden de los mismos lo cual suele ser conveniente en determinados casos de uso.

---
**Nota:** se recomienda leer la [documentación recogida en la MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat) para todos aquellos lectores que quieran ampliar conocimientos sobre este método.

---