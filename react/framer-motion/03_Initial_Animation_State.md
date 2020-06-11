# Estado inicial de la animación.

En el punto anterior hemos visto cómo podemos utilizar la prop `animate` del componente `motion` que nos ofrece **framer-motion** para animar cualquiera de los elementos html que forman nuestros componentes. Además tenemos que recordar que la prop `animate` espera recibir como valor un objeto formados por aquellos atributos y valores que representan las propiedades CSS del elemento que queremos animar.

Otra de las props que podemos establecer en el componente `motion` es la que nos va a permitir definir el estado inicial del que partirá nuestra animación. Esta prop recibe el nombre de `initial` y esperará recibir un objeto cuyos atributos representarán las propiedades CSS para el elemento sobre el que se define la animación de las cuáles partiremos y como valores de los mismos, como podemos intuir, los valores para dichas propiedades.

Para ver cómo funciona esta prop vamos a partir de otro de los componentes que tenemos definido dentro de nuestras aplicación: el componente `Header` cuyo cometido no es otro que mostrar la cabecera que es común a todo nuestra aplicación. El código del que partiremos será el siguiente:

```javascript
import React from 'react'

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
    <div className='title'>
      <h1>Pizza Join</h1>
    </div>
  </header>
)
```

Lo que pretendemos conseguir en este punto será animar el elemento `<div>` que contiene la información del título de la página. Para ello lo primero que tendremos que hacer será importar el componente `motion` de la librería **framer-motion**:

```javascript 
import React from 'react'
import { motion } from 'framer-motion'
```

Como vimos en el [punto anterior](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/framer-motion/02_Animating_Element.md), lo siguiente que tendremos que hacer será convertir el `<div>` que queremos animar en un elemento controlado por **framer-motion** sin mas que utilizar el componente `motion` seguido de la notación del punto `.` junto con el nombre del elemento html que vamos a animar. En nuestro caso esto nos deja lo siguiente:

```javascript
<motion.div className='title'>
  <h1>Pizza Joint</h1>
</motion.div>
```

Ahora es el momento en el que tendremos que establecer el valor de la prop `animate` para este componente. En nuestro caso lo que vamos a predenter es qeu el elemento en cuestión se desplace verticalmente una cantidad de 10 píxeles hacia la parte superior de la página por lo que haremos uso del atributo `y` que **framer-motion** nos proporciona en el objeto que se le pasa como valor a la prop `animate` además de indicarle como valor `-10` porque tenemos que recordar que por defecto la unidad de medida que toma la librería serán píxeles. Por lo tanto escribiremos:

```javascript
<motion.div 
  className='title'
  animate={{ y: -10 }}
>
  <h1>Pizza Joint</h1>
</motion.div>
```

Lo que vamos a intentar que suceda es que la animación que se llevará a cabo no tenga en cuenta la posición final del elemento dentro de la página sino que lo que queremos es que el elemento 'entre' de la parte superior y finalice en la posición que acabamos de definir. Es aquí donde entra en juego la prop `initial` teniendo en cuanta que en el objeto que le pasamos como valor estableceremos su atributo `y` a un valor todavía más negativo del que hemos establecido dentro objeto que pasamos a la prop `animate`. Así pues podemos escribir:

```javascript
<motion.div 
  className='title'
  initial={{ y: -250 }}
  animate={{ y: -10 }}
>
  <h1>Pizza Joint</h1>
</motion.div>
```

Pero ¿qué es lo que estará sucediendo realmente en esta animación? ¿Nuestro `<div>` se estará desplazando desde la posición `y` -250 a la posición `y` -10? La respuesta es que sí pero con un matiz ya que lo que realmente sucederá es que la animación se ejecutará desde la posición -250, irá a la posición 0 (es decir, la posición donde realmente se mostrará el elemento en la página lo que provoca que la animación transcurra hacia abajo) y posteriormente se desplazará nuevamente hacia arriba para acabar en la posición -10.

Con esto lo que queremos decir es que en la prop `initial` siempre deberemos especificar todas aquellas propiedades CSS de partida para la realización de la animación mientras que en la prop `animate` lo que tenemos que especificar son los valores de las propiedades CSS con las que ha de finalizar el elemento tras la secuencia de animación. Esto se traduce en que vamos a tener un mayor control sobre lo que queremos que suceda en una animación ya que vamos a poder definir el punto de partida y finalización de la misma.

Cojamos otro de los componentes que estamos utilizando en nuestra aplicación de ejemplo, en este caso el componente `Home` que recordemos que es con el que hemos trabajado en el [punto anterior](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/framer-motion/02_Animating_Element.md):

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

En este caso lo que vamos a hacer es animar la propiedad CSS `opacity` que estará asociada al primero de los `<div>` del componente. En concreto lo que querremos hacer es un efecto de aparición del elemento por lo que partiremos de una valor para este atributo de 0 (lo que significará que el objeto es completamente transparente) a un valor de 1 (que significa que el objeto es completamente opaco) lo que nos deja el siguiente código:

```javascript
<motion.div 
  className='home container'
  initial={{ initial: 0 }}
  animate={{ opactity: 1 }}
>
```

---
**Nota:** hemos eliminado el resto de atributos CSS que teníamos definidos en el objeto asociado a la prop `animate` porque no son relevantes para la explicación ademaś de que no nos van a ser útiles para conseguir las animaciones ni efectos que estamos persiguiendo.

---
**Nota:** todos aquellos lectores que quieran obtener más información acerca de cómo funciona el atributo CSS `opacity` les recomendamos [leer la MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/opacity) para obtener una información completa.

---

En este punto si ahora grabamos nuestro trabajo y recargamos nuestra aplicación podemos ver cómo la animación de la opacidad se realiza de forma muy rápida y prácticamente es inapreciable pero ahi está (dejaremos para más adelante el ver cómo podemos controlar la velocidad de nuestras animaciones).

Vamos ahora con otro de los componentes de nuestra aplicación, en concreto el omponente `Base` que nos sirve para mostrar las opciones que el usuario tendrá a su disposición entra las cuales podrá elegir cuál será la base que querrá aplicar a su pizza. El aspecto de partida de este componente es el siguiente:

```javascript
import React from 'react'
import { Link } from 'react-router-dom'

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
          <div className='next'>
            <Link to='/toppings'>
              <button>Next</button>
            </Link>
          </div>
        )
      }
    </div>
  )
}

export default Base
```

En este caso lo que querremos animar será animar el botón que se muestra al final del marcado del componente. Ahora bien, ¿qué peculiaridad tiene este botón? Pues para entenderlo tenemos que mirar detenidamente el código JSX queestá relacionado con el:

```javascript
pizza.base && (
  <div className='next'>
    <Link to='/toppings'>
      <button>Next</button>
    </Link>
  </div>
)
```

Por lo tanto ¿qué es lo que tiene de especial? Pues aquí es donde tenemos que darnos cuenta que el marcado que mostrará el botón no se muestra siempre sino que lo hará en el caso de que el usuario haya seleccionado una base para la pizza que quiere pedir (acción que sabemos que habrá sida llevada a cabo porque el atributo `base` del objeto `pizza` tendrá un valor asociado). 

Sabien esto el otro aspecto que tenemos que considerar es determinar el tipo de animación que queremos llevar a cabo que en este caso será que el botón aparezca desde la parte izquierda hasta situarse en la posición que le corresponda dentro de la página.

Con esto en mente, el primer paso que tendremos que dar será importar el componente `motion` de la librería **framer-motion** dentro del componente como ya hemos hecho otras veces:

```javascript
import React from 'react'
import { Link } from 'react-router-dom'
import { motion } from 'framer-motion'
```

En segundo lugar aplicamos la notición del punto sobre el componente `motion` y el elemento `<div>` que queremos animar:

```javascript
pizza.base && (
  <motion.div className='next'>
    <Link to='/toppings'>
      <button>Next</button>
    </Link>
  </motion.div>
)
```

Ahora es el momento en el que tenemos que definir el valor de las props `initial` y `animate` sobre el componente `motion`. Vamos a comenzar con el objeto asociado a la prop `initial` donde estableceremos que el valor del atributo `x` (que recordemos que es un atributo que tiene un significado especial para **framer-motion** ya que viene a indicar el posicionamiento del eleemnto en el eje X) pero esta vez le asignaremos como valor el string `-100vw` ya que en este caso no queremos que se aplique como medida el píxele sino los viewport width.

```javascript
pizza.base && (
  <motion.div 
    className='next'
    initial={{ x: '-100vw' }}
  >
    <Link to='/toppings'>
      <button>Next</button>
    </Link>
  </motion.div>
)
```

---
**Nota:** para todo aquel lector que quiera obtener más información acerca de cómo funciona la unidad **viewport view** para CSS le recomendamos [leer la MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Viewport_concepts) para obtener más información al respecto.

---

Nos queda por definir dónde queremos que finalice la animación y en este caso vamos a hace que sea en el punto en el que se muestra renderizado el componente dentro de la página lo que supone que tengamos que establecer el valor del atributo `x` del objeto que se le pasa a la prop `animate` con el valor 0:

```javascript
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
```

---
**Nota:** es importante recalcar que los atributos `x` e `y` que nos proporciona **framer-motion** no recogen valores absolutos con respecto al eje de coordenadas de la página (donde el eje X comienza a contar desde la parte izquieda de la página y el eje Y desde la parte superior de la página) sino que siempre tomarán como valor un desplazamiento (offset) con respecto a la posición real dentro de la página en la que se mostraría el componente si no fuese animado.

---

¿Y cuándo sucederá la animación del botón? Pues cuando el botón vaya a ser renderizado dentro de la página (dentro del DOM) lo que significa que cuando el usuario elija una base para su pizza es cuando el botón aparecerá en la página entrando desde la parte izquierda y situándose en la posición que le corresponderá ser reenderizado.

## Resumen

En este punto hemos visto que podemos utilizar la prop `initial` para especificar todos aquellas propiedades CSS que nos ayudarán a definir el lugar desde el que se realizará la animación. Por otra parte tenemos la prop `animate` que nos servirá para definir todas las propiedades CSS que ha de tener el elemento en el momento en el que finalice la animación.

## Componente final

El aspecto final de nuestro componente `Header` tras la finalización del estudio de este punto del tutorial es el que se muestra a continuación:

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

En el caso del otro componente con el que hemos estado trabajando a lo largo de este punto (el componente `Home`) el aspecto final del mismo sería:

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

Por último, el código final del componente `Base` tras las modificaciones que hemos ido realizando en este punto tendrá el siguiente aspecto:

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