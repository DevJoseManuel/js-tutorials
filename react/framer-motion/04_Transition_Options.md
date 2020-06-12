# Opciones para la transiciones

En los dos puntos anteriores hemos visto [cómo utilizar la prop `initial`](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/framer-motion/03_Initial_Animation_State.md) para establecer los valores de las propiedades CSS de partida para nuestras animaciones [cómo podemos utilizar la prop `animate`](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/framer-motion/02_Animating_Element.md) para indicar los valores de las propiedades CSS con las que queremos que finalice nuestra animación. 

Ahora bien ¿cómo podemos controlar la forma en la que se produce la animación? **Framer-motion** realizará las animaciones por nosotros pero aplicando sus propios criterios predeterminados para ello siendo estos válidos para la mayoría de las situaciones a las que nos vamos a enfrentar en nuestro día a día en el desarrollo pero ¿y si necesitamos un grado de control más fino? ¿cómo podemos controlar cosas como cuánto queremos que dure la animación, cuándo queremos que comience a aplicarse, etc.?

Para lograrlo el componente `motion` que nos proporciona la **framer-motion** pone a nuestra disposición otra prop: `transition` y la forma en la que funciona y los valores que admite es lo que vamos a estudiar en este punto del tutorial. Lo primero que tenemos que entender es que el valor que asignemos a esta prop servirá para describir cómo queremos que transcurra la animación que hayamos definido desde el punto inicial (entendiendo como punto el conjunto de propieadades CSS que hemos definido en la prop `initial`) hasta el punto final (entendiendo el conjunto de propiedades CSS que hemos recgido en la prop `animate`).

Tomando el componente `Home` de nuestra aplicación vamos a ver cómo utilizar esta prop. El código de partida de nuestro componente es el siguiente:

```javascript
import React from 'react'
import { Link } from 'react-router-dom'
import { motion } from 'framer-motion' 

const Home = () => (
  <motion.div 
    className='home container'
    initial={{ opacity: 0 }}
    animate={{ opacity: 1 }}
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

Supongamos que lo que queremos hacer es controlar la animación de la propiedad CSS `opactity` que tenemos definida para el componente `motion.div`. Recordando lo que hemos dicho hasta ahora en este caso, cuando se cargue la página, lo que sucederá es que se estará animando el `<div>` que tiene asociado el componente pasando de estar transparence (derivado de que en la prop `initial` se define el valor 0 para el atributo CSS `opacity`) a estar completamente opaco (ya que el valor para esta misma propiedad pero esta vez en la prop `animate` se establece a 1).

Vamos a ver ahora cómo podemos controla esta animación. Como ya hemos indicado lo primero que haremos será establecer la prop `transition` la cual esperará recibir como valor un objeto donde cada uno de los atributos servirá para controlr alguno de los aspectos que describen la animación:

```javascript
<motion.div 
  className='home container'
  initial={{ opacity: 0 }}
  animate={{ opacity: 1 }}
  transition={{}}
>
```

Uno de los atributos que podemos establecer en este objeto se denomina `delay` el cual espera recibir como valor un número que viene a indicar la cantidad de segundos que han de transcurrir desde que el elemento se renderiza en la página para comenzar a realizar la animación desde el punto inicial al punto final. Así si definimos:

```javascript
<motion.div 
  className='home container'
  initial={{ opacity: 0 }}
  animate={{ opacity: 1 }}
  transition={{ delay: 1.5 }}
>
```

lo que estaremos indicando es que la animación de la propiedad CSS `opacity` no ha de comenzar hasta que transcurra un segundo y medio desde el momento en el que el `<div>` es renderizado dentro de la página.

---
**Nota:** es importante entender que en el momento en el que el elemento es renderizado en la página este se mostrará con las propiedades CSS que estén definidas en el objeto `initial` lo que viene a decir, en nuestro ejemplo, que el `<div>` comenzará con un valor de `opacity` de 0 (es transparente).

Una vez renderizado es cuando comienza a contar la cantidad de tiempo (segundos) que han sido definidos en el atributo `delay` del objeto `transition` lo que hace, en nuestro ejemplo, que durante 1.5 segundos el `<div>` permanezca invisble. Transcurrido este tiempo es cuando sucede la animación lo que hace que el `<div>` pase poco a poco a estar opaco ya que el valor de propiedad `opacity` en el objeto asociado a la prop `animate` es 1.

---

Otro los atributos que podemos establecer en el objeto que pasamos a la prop `transition` es duration y viene a especificar la cantidad de tiempo (en segundos) que ha de durar la animación desde el punto inicial al punto final. Así si en nuestro ejemplo definimos:

```javascript
<motion.div 
  className='home container'
  initial={{ opacity: 0 }}
  animate={{ opacity: 1 }}
  transition={{ delay: 1.5, duration: 5 }}
>
```

lo que estaremos indicando es que queremos que la cantidad de tiempo que queremos que transcurra desde que el `<div>` está totalmente transparente (valor de la propiedad `opacity` a 0) a estar totalmente opaco (valor de la propiedad `opacity` a 1) será de 5 segundos.

---
**Nota:** al hilo de lo que hemos explicado en la nota anterior es importante comprender que estos 5 segundos empiezan a contar desde el momento en el que transcurre el valor que hemos establecido en el atributo `delay` del objeto que pasamos a la prop `transition`. En otras palabras, lo que tenemos que entender es que ahora mismo todo el trabajo que tiene que ver con la animación del `<div>` finalizará 6.5 segundos después del momento en el que es renderizado dentro de la página (1.5 segundos por el `delay` más 5 segundos por el `duration`).

---

Tomemos otro de los componentes de nuestra aplicación para ver cómo podemos controlar las animaciones que hemos definido dentro del mismo. En concreto vamos a tomar el componente `Header` cuyo código es el que se muestra a continuación:

```javascript
import React from 'react'
import { motion } from 'framer-motion'

const Header = () => (
  <header>
    <div className='logo'>
      <svg className='pizza-svg' xmlns='http://www.w3c.org/2000/svg' viewBox='0 0 100 100'>
        <path
          fill='none'
          d='M40 40 L80 40 C80 40 80 80 40 80 C40 80 0 80 0 40 C0 40 0 0 40 0Z'
        />
        <path
          fill='none'
          d='M50 30 L50 -10 C50 -10 90 -10 90 30 Z'
        />
      </svg>
    </div>
    <motion.div 
      className='title'
      initial={{ y: -250 }}
      animate={{ y: -10 }}
    >
      <h1>Pizza Joint</h1>
    </motion.div>
  </header>
)
```

Lo que queremos hacer en este caso es controlar la animación que está definida en el componente `motion.div` que hemos visto que aparecerá desde la fuera de la página por la parte superior (el valor del atributo `y` definido en el objeto `initial` es de -250, lo que supone que se renderizará inicialmente -250 píxeles por encima en el eje Y del lugar en el que le correspondería ser mostrado el elemento y, como se trata de la cabecera la página, esto supone que se "salga" de la página) hasta su posición donde le correspondería ser renderizado para finalmente desplazarse otros 10 píxeles hacia arriba (el valor del atributo `y` en el objeto `animate` es de -10) lo que provoca una sensación de reobte del componente una vez se muestra la página.

Vamos a suponer que en este caso lo que queremos hacer es que la animación no comience hasta que hayan transcurrido 0.2 segundos desde el momento en el que se muestra el elemento `<div>` por lo que definimos la prop `transition` esbleciendo su propiedad `delay` con el valor 0.2 de la siguiente manera:

```javascript
<motion.div 
  className='title'
  initial={{ y: -250 }}
  animate={{ y: -10 }}
  transition={{ delay: 0.2 }}
>
```

Otra de las propiedades que podemos establecer en el objeto que pasamso a la prop `transition` es `type` pero en este caso el valor que le podemos asignar lo tendremos que escoger entre `tween`, `spring` o `inertia` haciendo que la animación desde el punto inicial al punto final se comporte de forma diferente:

* `spring` lo que va a hacer es que la forma en la que se visualiza la animación refleja una especie de *rebote* lo que puede hacerlo muy agradable a la vista, siendo este el tipo de animación que funcionará la mayoría de las veces (internamente lo que estará haciendo es que la escala de valores por la que pasará la propiedad CSS a animar no consumen el mismo tiempo el paso entre unos y otros, tardando más al principio y decrementando el tiempo necesario para avanzar en la escala con los últimos valores).
* `tween` por su parte lo que viene a hacer es un tipo de animación más *lineal* en el sentido de que la propiedad CSS que animaremos pase del punto A al B en un periodo de tiempo más o menos regular (en otras palabras, la escala de valores del atributo por el que ha de pasar la propiedad la cantidad de tiempo que transcurre entre cada uno de ellos es siempre la misma).
* `inertia` se trata del tipo de animación *contraria* a `spring` en el sentido de que el rebote será mucho más lento (en otras palabras, en vez de transicionar más rápidamente entre los valores de las propiedades CSS a animar al final lo que hace es transicionar más rápidamente al principio para que el final el efecto esté mucho más atenuado).

---
**Nota:** no es importante recordar los valores que le podemos asignar al atributo `type` del objeto `transition` ya que es fácil que nos olvidemos de ellos y porque siempre [podemos acudir a la documentación oficial](https://www.framer.com/api/motion/types/) de **Framer-motion** para obtener más información al respecto.

---
**Nota:** hay que entender que el `type` de animación que vamos a definir influirá de una manera u otra en cada una de las propiedes CSS que queramso animar por lo que siempre es recomendable experimentar con ellas hasta que encontremos el valor que mejor nos conviene.

Además tenemos que entender que el valor por defecto para esta propiedad no está definido de forma general para cada propiedad CSS ya que, por ejemplo, en el caso de las propiedades `x` e `y` se utiliza el tipo `spring`. Nuevamente es recomendable [acudir a la documentación oficial](https://www.framer.com/api/motion/types/) para obtener una explicación completa.

---

En nuestro ejemplo vamos a establecer el valor del atributo `type` a `spring` lo que nos dejará con el siguiente código:

```javascript
<motion.div 
  className='title'
  initial={{ y: -250 }}
  animate={{ y: -10 }}
  transition={{ delay: 0.2, type: 'spring' }}
>
```

Otra de las propiedades que podemos definir dentro del objeto `transition` es `stiffness` pero lo primero que tenemos que conocer sobre ella es que únicamente tendrá sentido aplicarla cuando el `type`de la animación sea `spring`. Esta propiedad esperará recibir como valor un número que viene a indicar cómo de *grande* queremos que sea el efecto del rebote. Así, un valor grande (por ejemplo 500) lo que hará no solamente que la animación se relace de forma muy rápida sino que el efecto de rebote sea muy grande, mientras que si es pequeño (por ejemplo 5) lo que viene a hacer es que la animación transcurra lentamente y el rebote sea prácticamente inapreciable. 

Vamos a definir en nuestro ejemplo el atributo `stiffness` al valor 120 ya que el valor por defecto para este atributo es de 100 y lo que vamos a pretender es que nuestra animación produzca un efecto de rebote algo más pronunciado que el valor por defecto. Así pues, definimos lo siguiente:

```javascript
<motion.div 
  className='title'
  initial={{ y: -250 }}
  animate={{ y: -10 }}
  transition={{ delay: 0.2, type: 'spring', stiffness: 120 }}
>
```

---
**Nota:** anteriormente hemost visto el atributo `duration` que podemos especificar para determinar cuánto queremos que dure nuestra animación. Pues bien, es importante que entendamos que el único `type` de animación donde tendrá sentido utilizarlo (donde de hecho únicamente tiene sentido) es cuando el valor de `type` es `tween`.

Es más, en el ejemplo para el componente `Home` no nos hizo falta especificar el valor de `type` ya que el la propiedad CSS que estábamos animando era `opacity` y es `tween` el valor por defecto que adquiere esta propiedad como el tipo de animación que tendrá asociada.

---

Vamos ahora con el componente `Base` que también [hemos definido en el punto anterior](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/framer-motion/03_Initial_Animation_State.md) del cual sabemos que su misión es mostrar el usuario un menú de opciones entre las cuáles ha de elegir la base para la pizza que quiere pedir. Una vez se selecciona un elemento un botón aparecerá desde la parte izquierda de la página que al ser pulsado hará que se transicione al siguiente menú para seguir eligiendo elementos con los que conformar la pizza. El código del que partimos es el siguiente:

```javascript
import React from 'react'
import { Link } from 'react-router-dom'
import { motion } from 'framer-motion'

const Base = ({ addBase, pizza }) => {
  const bases = ['Classic', 'Thin & Crispy', 'Thick Crust']

  return (
    <div className='base container'>

      <h3>Step 1: Choose Your Base</h3>
      <ul>
        {
          bases.map(base => {
            let spanClass = pizza.base === base ? 'active' : ''
            return (
              <li key={ base } onClick={ () => addBase(base)}>
                <span className={spanClass}>{ base }</span>
              </li>
            )
          })
        }
      </ul>
      {
        pizza.base && (
          <motion.div 
            className='next'
            initial={{ x: '-100vw' }}
            animate={{ x: 0 }}
          >
            <Link to='/toppings'>
              <button>Next</button>
            </Link>
          </motion.div>
        )
      }
    </div>
  )
}

export default Base
```

En concreto nos vamos a centrar en cómo vamos a definir la animación que queremos que se lleve a cabo sobre el botón de la parte inferior donde se muestra la opción para pasar al siguiente menú. Así pues lo primero que quetenemos que hacer es definir la prop `transition` dentro del componente `motion.div` y ahora vamos a especificar de forma explícita que queremos que la animación sea del tipo `spring` (cosa que hacemos utilizando el atributo `type`):

```javascript
<motion.div 
  className='next'
  initial={{ x: '-100vw' }}
  animate={{ x: 0 }}
  transition={{ type: 'spring' }}
>
```

---
**Tip:** una práctica muy común a la hora de trabajar con la prop `transition` dentro de **Framer-motion** es especificar el valor del atributo `type` y no dejar que el código se base en los valores por defecto para cada una de las propiedades ya que podrían cambiar entre versión y versión de la librería o simplemente que no nos acordemos de cuáles son haciendo que nuestro código sea mucho más complejo de leer, de leer y de mantener.

---

Gracias a que hemos especificado que el `type` de la animación es `spring` podremos añadir el atributo `stiffness` al que le vamos a asignar el valor 120:

```javascript
<motion.div 
  className='next'
  initial={{ x: '-100vw' }}
  animate={{ x: 0 }}
  transition={{ type: 'spring', stiffness: 120 }}
>
```

Con esto acabaríamos por animar el botón de la parte inferior del componente pero vamos a ir un paso más adelante ya que si nos fijamos en el código JSX del componente `Base` vemos que todo está contenido dentro de un elemento `<div>`:

```javascript
<div className='base container'>
```

Nuestra intención será animar este elemento por lo que, como ya sabemos, lo primero que tenemos que hacer es convertirlo en un componente `motion` de **Framer-motion** utlizando la notación punto `.` de la siguiente marena:

```javascript
<motion.div className='base container'>
```

Nos centramos en la animación que queremos crear y que va a consistir en que el menú con las diferentes bases a partir de las cuáles podemos construir nuestra pizza ha de aparecer desde fuera de la parte derecha de la pantalla hasta situarse en su posición dentro de la página:

```javascript
<motion.div 
  className='base container'
  initial={{ x: '100vw' }}
  animation={{ x: 0 }}
>
```

Con esto lo que estamos diciendo es que queremos que inicialmente el `<div>` se sitúe fuera de la página por la parte de la derecha gracias a que se está estbleciendo el valor del atributo `x` del objeto con el que se establece la prop `initial` con un valor de 100vw y que el uso del tipo de medida `vw`, en lugar de píxeles, lo que nos garantiza que un valor de 100 sacará al elemento de la página por la parte derecha. 

Además estamos definiendo que el valor dentro del eje X en el que finalizará la animación (en la prop `animate`) ha de ser 0 por lo que al final el elemento será renderizado en la posición que le correspnde dentro de la página logrando así la animación que pretendíamos.

---
**Nota:** para todo aquel lector que quiera obtener más información acerca de cómo funciona la unidad **viewport view** para CSS le recomendamos [leer la MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Viewport_concepts) para obtener más información al respecto.

---

Ahora simplemente nos queda definir cómo queremos que se lleve a cabo la animación para lo cual en la prop `transition` definiremos el atributo `type` como `spring` (lo hacemos de forma explícita garantizando de esta manera que queremos que se produzca ese efecto de rebote) y además que queremos que la animación comience 0.2 segundos después de que se haya cargado el elemento en el DOM por lo que haremos uso de la prop `delay` para lograrlo:

```javascript
<motion.div 
  className='base container'
  initial={{ x: '100vw' }}
  animation={{ x: 0 }}
  transation={{ type: 'spring', duration: 0.2}}
>
```

## Resumen

En este apartado hemos aprendido cómo utilizar la prop `transition` del componente `motion` de **Framer-motion** para poder controlar las animaciones que definamos dentro de nuestros componentes de React. 

No obstante, nos hemos quedamos en la superficie en lo que respecta al conocimiento de las posibilidades que tenemos a nuestra disposición para realizar las animaciones tener un control sobre las mismas y es precisamente a ello a lo que vamos a dedicar los siguientes punto de este tutorial.

## Componentes finales

El aspecto final que tendrá el componente `Home` tras aplicar todo lo explicado en este punto es el que se muestra a continuación:

```javascript
import React from 'react'
import { Link } from 'react-router-dom'
import { motion } from 'framer-motion' 

const Home = () => (
  <motion.div 
    className='home container'
    initial={{ opacity: 0 }}
    animate={{ opacity: 1 }}
    transition={{ delay: 1.5, duration: 5 }}
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

Veamos ahora el aspecto final del código que posee el componente `Header` tras las modificaciones que hemos llevado a cabo:

```javascript
import React from 'react'
import { motion } from 'framer-motion'

const Header = () => (
  <header>
    <div className='logo'>
      <svg className='pizza-svg' xmlns='http://www.w3c.org/2000/svg' viewBox='0 0 100 100'>
        <path
          fill='none'
          d='M40 40 L80 40 C80 40 80 80 40 80 C40 80 0 80 0 40 C0 40 0 0 40 0Z'
        />
        <path
          fill='none'
          d='M50 30 L50 -10 C50 -10 90 -10 90 30 Z'
        />
      </svg>
    </div>
    <motion.div 
      className='title'
      initial={{ y: -250 }}
      animate={{ y: -10 }}
      transition={{ delay: 0.2, type: 'spring', stiffness: 120 }}
    >
      <h1>Pizza Joint</h1>
    </motion.div>
  </header>
)
```

Por último, el código del componente `Base` tras aplicar todas las modificaciones que hemos descrito en este punto del tutorial será el que se muestra a continuación:

```javascript
import React from 'react'
import { Link } from 'react-router-dom'
import { motion } from 'framer-motion'

const Base = ({ addBase, pizza }) => {
  const bases = ['Classic', 'Thin & Crispy', 'Thick Crust']

  return (
    <motion.div 
      className='base container'
      initial={{ x: '100vw' }}
      animation={{ x: 0 }}
      transation={{ type: 'spring', duration: 0.2}}
    >
      <h3>Step 1: Choose Your Base</h3>
      <ul>
        {
          bases.map(base => {
            let spanClass = pizza.base === base ? 'active' : ''
            return (
              <li key={ base } onClick={ () => addBase(base)}>
                <span className={spanClass}>{ base }</span>
              </li>
            )
          })
        }
      </ul>
      {
        pizza.base && (
          <motion.div 
            className='next'
            initial={{ x: '-100vw' }}
            animate={{ x: 0 }}
            transition={{ type: 'spring', stiffness: 120 }}
          >
            <Link to='/toppings'>
              <button>Next</button>
            </Link>
          </motion.div>
        )
      }
    </motion.div>
  )
}

export default Base
```