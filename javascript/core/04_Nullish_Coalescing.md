# Nullish Coalescing

Lo primero que tenemos que entender es que cuando estamos hablando de **Nullish Coalescing** estamos haciendo referencia a un nuevo operador que JavaScript nos ofrece para poder trabajar en nuestras aplicaciones.

Como desarrolladores de JavaScript estaremos acostumbrados a trabajar con el operador ternario escribiendo instrucciones como la siguiente:

```javascript
let result = null ? 'yes' : 'no'
```

A modo de recordatorio el operador ternario lo que hace es evaluar la expresión lógica que precece al caracter `?` (interrogación) y en el caso de que sea `true` retorna el resultado de la evaluación de la instrucción situada entre el `?` y los dos puntos `:` mientras que si es `false` retornará el resultado de la evaluación de la instrucción situada tras los dos puntos.

Así, en el ejemplo anterior, nos encontramos con que se está evaluando la expresión lógica `null` y como sabemos que JavaScript este valor es evaluado como `false` (lo que se conoce como *falsy*) se evaluará la intrucción tras los dos puntos la cual retornará el string `no` que será asignado a la variable `result`.

Es bastante habitual que durante el desarrollo de una aplicación tengamos que consultar si algún valor es `null` o `undefined`. Conviene recordar qué diferencias existen entre uno y otro:

* `undefined` puede hacer referencia tanto a una variable que ha sido declarada pero a la que no se le ha asignado un valor o, en el caso de estar realizando una búsqueda en el DOM, siempre que busquemos un nodo del mismo que no existe.
* `null` hace referencia a un valor que es establecido de una forma explicita a una variable, es decir, que para que una variable tenga este valor primero tendremos que declararla (lo que hará que esté `undefined`) y posteriormente asignarle el valor `null`.

¿Qué es lo que queremos lograr al utilizar el operador **Nullish Coalescing**? De modo coloquial lo que queremos es que se ejecute solamente la instrucción que sería ejecutada en el caso de estar utilizando el operador ternario cuando el resultado de la condición sea `true`. Pero ¿podemos escribir cualquier condición para que sea evaluada? No, en este caso tenemos que escribir una condición que al ser evaluada retorne un valor `null` o `undefined` ya que únicamente en el caso de que esto sea así (la condición sea cierta) entonces se ejecutará la instrucción que indicaremos como parte del operador.

¿Y cómo se escribe este nuevo operador? Con un doble signo de interrogación `??`. Así la siguiente instrucción es correcta:

```javascript
let result = null ?? 'yes'
```

Y lo que hace será asignar el valor `yes` a la variable `result` porque, como es fácil entender, el valor `null` es `null`. Pero ¿y si el resultado de la condición no es `null` o `undefined`? Lo que sucederá en este caso es que el resultado de la evaluación de la condición es lo que se retornará como resultado de aplicación del operador. Esto quiere decir que tras la ejecución del siguiente código:

```javascript
let result = 5 ?? 'yes'
```

el valor que tendra asignado la variable `result` será `5`.

Vamos a tratar de ver la utilidad de este operador en un caso de uso más concreto. Supongamos que dentro del código de una nuestras funciones vamos a chequear si una variable tiene un valor asignado (que no sea `null` o `undefined`) y únicamente en el caso de que no sea así lo que haremos será llamar a otra función que se encargue de asignarle un valor:

```javascript
let current

function f() {
  let result = current ?? getNum()
  console.log(result)
}

function getNum() {
  current = Math.floor(Math.random() * 100)
  return current
}

f()
```

La primera vez que se llama a la función `f` el valor de la variable `current` será `undefined` (la variable está declarada pero no tiene un valor asignado) por que la condición del **Nullish Coalescing** se evalua a `true` lo que hace que se pase a ejecutar el función `getNum` dentor de la cual se obtiene un valor número aleatorio que será asignado a la variable `currrent` además de ser retornado como resultado de la ejecución de dicha función.

¿Qué sucederá la siguiente vez que se invoque a la función `f`? Pues que la variable `current` ya tiene un valor asignado y por lo tanto la condición del **Nullish Coalescing** será evaluada a `false` y no se asignará un nuevo valor a la variable.
Esto quiere decir que la única forma que tenemos en nuestro código de poder reasignar un nuevo valor a la variable `current` una vez se le ha asignado la primera será gracias a la invocación de la función `getNum`.

Si hemos entendido bien la explicación anterior ¿cuál será el resultado de la invocación del siguiente código?

```javascript
getNum()
console.log(current)
f()
```

La respuesta es que por la consola se escribirá dos veces el valor que haya sido asignado a la variable `current` ya que, en primer lugar, tras la invocación de `getNum` se habrá establecido el valor inicial y cuando se invoca a la función `f` la variable ya tiene un valor que no es `null` ni `undefined` y por lo tanto no se ejecutará la instrucción asociada al **Nullish Coalescing**.

El siguiente fragmento de código es una versión extendida del ejemplo que estamos siguiendo a lo largo de esta explicación.

```javascript
console.log(current)
getNum()
console.log(current)
f()
f()
f()
```

Si suponemos que el resultado de la invocación de la función `getNum` es 10 la información que obtenemos como salida por la consola será:

```javascript
undefined
10
10
10
10
```

Es decir, que la primera vez que se escribe por la consola el valor del variable `current` es `undefined` como consecuencia de que todavía no se le ha asignado un valor. Tras la invocación de la función `getNum` el valor que se le asigna a la variable es `10` lo que hace que en las posteriores tres invocaciones de la función `f` lo que sucede es que como la variable ya tiene un valor no se le asignará otro nuevo como resultado de la evaluación del operador **Nullish Coalescing**.

Por lo tanto, con el uso del operador **Nullish Coalescing** lo que estamos haciendo es una búsqueda explícita del valor `null` o `undefined` al evaluar una expresión lo que nos evitará el tener que realizar la comporación con la utilización de un operador ternario como sí que teníamos que hacer en las versiones anterior de JavaScript que no lo incluían.

---
**Nota:** al tratarse de una característica nueva del lenguaje si queremos que nuestro código JavaScript se pueda utilizar en versiones antiguas del lenguaje en las que no está soportado se recomienda la utilización de un transpilador de código como puede ser [Babel](https://babeljs.io/).
