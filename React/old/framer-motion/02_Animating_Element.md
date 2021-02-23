# Animando un elemento

Vamos a comenzar a estudiar `framer-motion` explicando la acción más sencilla que podemos llevar a cabo con el mismo. En concreto vamos a ver qué es lo que tenemos que hacer para animar uno de los elementos de nuestra página. Para ello partiremos del siguiente componente de React que es el que se muestra cuando un usuario accede a nuestra aplicación:

```javascript
import React from 'react'
import { Link } from 'react-router-dom'

const Home = () => (
  <div className='home container'>
    <h2>Welcome to pizza Joint</h2>
    <Link to='/base'>
      <button>Create your pizza</button>
    </Link>
  </div>
)

export default Home
```

El objetivo que vamos a perseguir será animar por una parte el texto contenido por el elemento `<h2>`, el elemento `<button>` y también el elemento `<div>` que los encierra a todos ellos. Para ello, el primero paso que tenemos que dar es importar alguno de los elementos que pone a nuestra disposición **framer-motion**. En concreto lo que vamos a importar es el objeto `motion` de la siguiente manera:

```javascript
import React from 'react'
import { Link } from 'react-router-dom'
import { motion } from 'framer-motion' 
```

---
**Tip:** en realidad `motion` es un componente de React que nos proporciona **framer-motion**.

---

Básicamente la forma en la que **framer-motion** funciona es que siempre que queramos animar uno o más elementos de nuestras páginas deberemos mostrarlos contenidos dentro de un elemento `motion` al que seguirá concatenado con un punto el elemento html que vamos a animar. Esto, que es complejo de explicar, es muy sencillo de ver con un ejemplo. En nuestro caso hemos dicho que queremos animar el elemento `<h2>` que forma parte de nuestro componente por lo que la forma de indicarlo en **framer-motion** sería:

```javascript
<motion.h2>Welcome to Pizza Joint</motion.h2>
```

Es decir, que estamos indicando que en este punto en concreto va a haber una animación controlada por **framer-motion** al hacer uso del objeto `motion` pero además que el elemento html que se está contenido y que será animado es un elemento `<h2>`. Internamente **framer-motion** lo que estará haciendo es crear un nuevo componente de React propio de la librería que cuando es renderizado en la página mostrará un elemento `<h2>` con la particularidad de que tendrá una serie de propiedades que nos van a permitir realizar animaciones sobre el mismo.

Pero ¿cómo podemos hacer ahora para animarlo? Aquí es donde entra en juego la prop de componente `motion` deniminada `animate` de forma análoga a como se utilizar una prop en cualquier otro componente de React:

```javascript
<motion.h2 animate={}>Welcome to Pizza Joint</motion.h2>
```

Pero ¿qué tenemos que pasar a esta prop para conseguir nuestro objetivo? Si vemos el ejemplo anterior al hacer uso de las llaves para declararla lo que estamos indicando es que vamos a pasarle algo (un valor) que será determinado dinámicamente (no está predefinido). En concreto, lo que tenemos que especificar es un objeto en el que tenemos que definir todas las propiedades CSS del elemento html que contiene que vamos a animar. Siguiendo con nuestro ejemplo definimos lo siguiente:

```javascript
<motion.h2 animate={{ fontSize: 50 }}>Welcome to Pizza Joint</motion.h2>
```

Con esta definición lo que estamos expresando es que queremos animar el tamaño de la fuente del elemento `<h2>` hasta los 50 píxeles ya que hemos hecho uso de la propiedad CSS `fontSize`. De esta manera lo que estaremos indicando es que cuando se cargue el componente dentro del DOM (cuando se renderiza por primero vez) lo que le pedimos a **framer-motion** es que anime el tamaño de la fuente en la que se muestra el contenido del elemento `<h2>` hasta que tenga una tamaño de 50 píxeles.

---
**Nota:** a la hora de definir las propiedades CSS que queremos animar en el objeto que hemos pasado a la prop `animate` se utiliza la nomenclatura *camelCase* para los atributos CSS. [Aquí se puede obtener un listado completo](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Properties_Reference) de todos los atributos CSS y su equivalente en **camelCase**.

---
**Tip:** no hemos tenido que especificar que la unidad del tamaño de la fuente sean píxeles sino que hemos escrito directamente el valor que esperamos. Esto es así porque **framer-motion** interpretará por defecto que el valor será expresado en píxeles. De hecho para la librería cualquier valor que pueda ser expresado como pixeles y siempre que no se especifique la unidad siempre será expresando teniendo en cuenta esta medida.

---

Evidentemente la cantidad de propiedades CSS que podemos animar no está restringida a una única ya que en el objeto que pasamos como valor a la prop `animate` podemos especificar todas aquellas propiedades CSS que necesitemos animar. Por ejemplo, podríamso animar también el color en el que se muestra la fuente de la siguiente manera:

```javascript
<motion.h2 animate={{ fontSize: 50, color: '#ff2994' }}>
  Welcome to Pizza Joint
</motion.h2>
```

---
**Tip:** animar el color de la fuente lo que provocará que el color final en el que se mostrará el texto será el que indicaremos en el atributo `color` del objeto con las propiedades CSS que queremos animar (objeto que pasamos como valor a la prop `animate`).

---

¿Y cómo podemos hacer para lograr que el elemento que estamos animando se mueva en horizontal o en vertical en la página? El patrón es el mismo, es decir, que tendremos que indicarlo como un atributo del objeto que se pasa valor a la prop `animate` pero ¿qué propiedad CSS es la que tenemos que utilizar? La respuesta es **ninguna** ya que cuando queramos hacer una tipo de animación que implique una traslación de un elemento dentro de la página tendremos que hacer uso, dentro de este objeto, de alguno de los atributos que sí que tienen un significado para **framer-motion**.

Así utilizaremos el atributo `x` cuando queramos realizar una traslación de izquierda a derecha y el atributo `y` cuando lo que queremos hacer sea una traslación de arriba hacia abajo (o viceversa en ambos casos):

* para el caso de atributo `x` un valor positivo va a indicar que queremos hacer una traslación de izquierda a derecha mientras que un valor negativo nos viene a indicar que la traslación que vamos a llevar a cabo será de derecha a izquierda.
* para el caso del atributo `y` un valor positivo indicará que el desplazamiento que queremos llevar a cabo será hacia abajo mientras que un valor negativo será hacia arriba.

Con esto en mente podemos indicar que queremos que una vez cargado el componente en el DOM se realice un desplazamiento de 100 píxeles hacia la izquierda de la siguiente manera:

```javascript
<motion.h2 animate={{ fontSize: 50, color: '#ff2994', x: 100 }}>
  Welcome to Pizza Joint
</motion.h2>
```

Si además quisiésemos desplazar el elemento que estamos animando 100 píxeles hacia arriba en la página escribiríamos lo siguiente:

```javascript
<motion.h2 animate={{ fontSize: 50, color: '#ff2994', x: 100, y: -100 }}>
  Welcome to Pizza Joint
</motion.h2>
```

Siguiendo estos pasos que acabamos de describir vamos a animar ahora el elemento `<button>`. Recordemos que el primer paso consiste en hacer uso del componente `motion` seguido de un punto y el elemento html que vamos a animar. Esto quiere decir que vamos a pasar de:

```javascript
<button>Create your pizza</button>
```

a lo siguiente:

```javascript
<motion.button>Create your pizza</motion.button>
```

El siguiente paso consistirá en definir la prop `animate` asignándole como valor un objeto en el que vamos a definir cómo ha de ser nuestra animación.

```javascript
<motion.button animate={}>Create your pizza</motion.button>
```

Vamos a suponer ahora que en el caso de este botón queremos incrementar la escala en al que se muestra en la página, es decir, que queremos incrementar las proporciones como debería renderizarse para que se ajusten a una determinada escala lo cual quiere decir que, si por ejemplo, el elemento se mostraba como un cuadrado de 100 píxeles de lado e indicamos una escala de 1.5, cuando finalice la animación, el cuadrado pasará a tener un lado de 150 píxeles. 

Debido a que este tipo de animaciones relacionadas con la escala en al que se muestra un elemento es bastante frecuente cuando trabajamos con **framer-motion** la librería nos proporciona un atributo propio que podemos utilizar en el objeto que pasamos como valor al atributo `animate`, el atributo `scale`. Así si queremos incrementar la escala hasta un valor 1.5 del tamaño original escribiremos:

```javascript
<motion.button animate={{ scale: 1.5 }}>
  Create your pizza
</motion.button>
```

Vamos ahora con el último de los elementos del componente de ejemplo que queremos animar: el elemento `<div>` que engloba al resto del contenido de la página. Como hemos hecho anteriormente lo primero que tenemos que hacer es sustituirlo por el objeto `motion` seguido de un punto y el elemento html que vamos a animar. En este caso partimos de:

```javascript
<div className='home container'>
  // Resto del contenido del componente.
</div>
```

que lo sustituimos por:

```javascript
<motion.div className='home container'>
  // Resto del contenido del componente.
</motion.div>
```

---
**Nota:** es importante observar que los atributos 'originales' que poseía el elemento html que se va a animar (en nuestro ejemplo el atributo `className`) se conservarán ya que **framer-motion** lo que hará será pasárselos al elemento html que renderizará.

---

El siguiente paso consistirá en definir la prop `animate` pasándole como valor un objeto en el que especificamos la propiedad `marginTop` para indicar que queremos que se anime el elemento `<div>` de tal manera que cuando finalice su carga se muestre dejando un margen de 200 píxeles con respecto al elemento que lo precede en el DOM por la parte superior. Así pues escribiremos lo siguiente:

```javascript
<motion.div 
  className='home container'
  animate={{ topMargin: 200 }}
>
  // Resto del contenido del componente.
</motion.div>
```

Si además queremos reducir la opacidad con la que se muestra el contenido del `<div>` podemos hacer uso de la propiedad CSS `opacity` de la siguiente manera:

```javascript
<motion.div 
  className='home container'
  animate={{ topMargin: 200, opacity: 0.2 }}
>
  // Resto del contenido del componente.
</motion.div>
```

---
**Nota:** `opacity` es la propiedad CSS que nos permiti especificar el *grado de transparencia* de un elemento dentro de la página. Por defecto todos los elementos se muestran con un valor de 1 que viene a indicar que el contenido es totalmente opaco mientras que un valor de 0 indicaría que el contenido es transparente. Los valores para se le pueden asignar a `opacity` han de oscilar entre 0 y 1 siendo los más próximos a cero más transparentes y los más próximos a 1 más opacos. Se recomienda [consultar la MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/opacity) para poder obtener más información.

---

Supongamos ahora que queremos que el elemento `<div>` rote sobre en el sentido de las agujas del reloj ¿cómo podemos hacer esto? Pues en este caso utilizaríamos el atributo `rotateZ` del objeto con las propiedades CSS que se le pasan a la prop `animate`. En este caso un valor positivo indicará que queremos que la rotación se lleve a cabo en el sentido de las agujas del reloj (sentido horario) mientras que un valor negativo indicará que la rotación la queremos llevar a cabo en el sentido contrario a las agujas del reloj (sentido anti-horario).

Así pues si queremos animar el elemento `<div>` para que rote 180 grados en el sentido de las agujas del reloj escribiremos:

```javascript
<motion.div 
  className='home container'
  animate={{ topMargin: 200, opacity: 0.2, rotateZ: 180 }}
>
  // Resto del contenido del componente.
</motion.div>
```

---
**Nota:** el atributo `rorateZ` del objeto que se le pasa a la prop `animate` se basa en al utilización de la función `rotateZ()` de CSS. Podemos encontrar más información al respecto [en la MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/rotateZ).

---

## Resumen

Llegamos a este punto podemos ver que gracias a **framer-motion** será muy sencillo el poder animar los diferentes elementos de nuestras páginas simplemente siguiendo una serie de pasos:

1. En primer lugar tendremos que importar el componente `motion` de la librería **framer-motion** como importamos cualquier otro componente, función, etc. para ser utilizado en nuestro código.
2. Para cualquiera de los elementos de la página que queramos animar haremos uso de la notación `motion.` seguida del nombre del elemento html que vamos a animar. Así, por ejemplo, si vamos a animar un elemento `<h2>` escribiríamos `<motion.h2>`.
3. Tendremos que definir la prop `animate` para el componente `motion` la cual espera recibir como valor un objeto formado por una serie de atributos y valores que recogen básicamente las propiedades CSS (siguiente notación *camelCase*) y los valores finales de las mismas que queremos que sean modificadas con la animación además de poder utilizar como atributos todas aquellas propiedades que **framer-motion** nos proporciona por conveniencia (como desplazamientos en el eje X con el atributo `x`, desplazamientos en el eje Y con el atributo `y`, etc.)

## Componente final

El aspecto final de nuestro componente `Home` tras la finalización del estudio de este punto del tutorial es el que se muestra a continuación:

```javascript
import React from 'react'
import { Link } from 'react-router-dom'
import { motion } from 'framer-motion' 

const Home = () => (
  <motion.div 
    className='home container'
    animate={{ topMargin: 200, opacity: 0.2, rotateZ: 180 }}
  >
    <motion.h2 animate={{ fontSize: 50, color: '#ff2994', x: 100, y: -100 }}>
      Welcome to Pizza Joint
    </motion.h2>
    <Link to='/base'>
      <motion.button animate={{ scale: 1.5 }}>
        Create your pizza
      </motion.button>
    </Link>
  </motion.div>
)

export default Home
```
