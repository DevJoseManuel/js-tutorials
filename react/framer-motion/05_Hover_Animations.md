# Animaciones `hover`

Hasta ahora hemos estado viendo cómo animar los elementos de nuestra página en el momento en el que estós son mostrados pero gracias a **Framer-motion** tenemos la posibilidad de que las animaciones que definiamos se lleven a cabo cuando el ratón pase por encima del elemento que queremos animar (lo que se conoce como un evento `hover`).

Vamos a ver cómo podemos utilizar esta técnica volviendo para ello sobre el componente `Home` de nuestra aplicación de ejemplo. El código que hemos desarrollado hasta ahora para este componente es el siguiente:

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

Vamos a fijarnos en el botón que se muestra en la parte inferior del componente el cual está etiquetado como *Create your pizza*. Por lo tanto partimos del siguiente código JSX:

```javascript
<motion.button animate={{ scale: 1.5 }}>
  Create your pizza
</motion.button>
```

Como podemos ver la animación que tenemos definida lo que hace es incrementar la escala (las dimensiones de elemento `<div>` que estamos renderizando) hasta que sea un 1.5 su tamaño normal (entendiendo como tal el tamaño con el que se renderizaría el elemento si no lo estuviésemos animando). Nuestro objetivo será conseguir el mismo efecto de animación pero no cuando se carga el elemento en la página sino cuendo el ratón se sitúe sobre el botón (`hover`).

¿Cómo podemos conseguirlo? La respuesta es muy sencilla ya que simplemente tendremos que sustituir la prop `animate` por la prop `whileHover` la cual esperará recibir como valor el mismo objeto que describa la animación que queremos que se lleva a cabo. Así pues escribimos lo siguiente:

```javascript
<motion.button whileHover={{ scale: 1.5 }}>
  Create your pizza
</motion.button>
```

Además con esto no solamente conseguimos que cuando el ratón se sitúe sobre el botón la escala con que este se muestra en la página se incremente hasta un tamaño 1.5 veces superior a su tamaño original sino que además cuando el ratón "sale" del botón este recuperará su tamaño original.

Como sucede con el objeto que se le pasa a la prop `animate` al objeto `whileHover` podrá contener cuantos atributos necesitemos animar. Por ejemplo, vamos a suponer que queremos añadir un sombreado al texto que contiene el `<div>` que representa a nuestro botón únicamente cuando el ratón está situado sobre el mismo. Así pues escribiremos lo siguiente:

```javascript
<motion.button
  whileHover={{
    scale: 1.5,
    textShadow: '0px 0px 8px rgb(255, 255, 255)'
  }}
>
  Create your pizza
</motion.button>
```

---
**Nota:** para obtener más información acerca de cómo funciona la propiedad `textShadow` de CSS se recomienda [consultar la MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/text-shadow).

---

Con esto hemos conseguido añadir una sombra al texto que está contenido dentro del botón pero ¿cómo podemos añadir este mismo efecto al botón en sí mismo? La respuesta es aplicando los mismos valores pero esta vez al atributo `boxShadow`. Por lo tanto escribiremos:

```javascript
<motion.button
  whileHover={{
    scale: 1.5,
    textShadow: '0px 0px 8px rgb(255, 255, 255)',
    boxShadow: '0px 0px 8px rgb(255, 255, 255)'
  }}
>
  Create your pizza
</motion.button>
```

---
**Nota:** para obtener más información acerca de cómo funciona la propiedad CSS `boxShadow` se recomienda [consultar la MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/box-shadow).

---

Una vez creado el efecto vamos a suponer que se trata del que queremos aplicar a todos los botones que tenemos definidos hasta ahora en nuestra aplicación de ejemplo. Hasta ahora hemos visto que tenemos que otro botón dentro del componente `Base` el cual se muestra al usuario una vez que ha escogido la base de la pizza que desea. El código que tenemos hasta ahora de este componente es el siguiente:+

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
              <li key={ base } onClick={ () => addBase(base) }>
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

El botón que tenemos que animar se encuentra en la parte inferior del código JSX del componente `Home`, es decir, que nos centraremos en el siguiente código:

```javascript
<button>Next</button>
```

¿Qué es lo que tendremos que hacer? En primer lugar tenemos que convertir el elemento `<button>` en componente de **Framer-motion** por lo que, como ya hemos hecho otras veces, simplemente utilizamos el componete `motion` junto con la notación punto `.` asociada al elemento html que vamos a animar:

```javascript
<motion.button>Next</motion.button>
```

El siguiente paso que tendremos que dar es realmente sencillo porque lo único que tenemos que hacer es copiar el valor que hemos definido a la prop `whileHover` del botón definido en el botón del componente `Home` como el valor de la prop `whileHover` para este botón:

```javascript
<motion.button
  whileHover={{
    scale: 1.5,
    textShadow: '0px 0px 8px rgb(255, 255, 255)',
    boxShadow: '0px 0px 8px rgb(255, 255, 255)'
  }}
>
  Next
</motion.button>
```

Esto mismo lo tenemos que aplicar en el componente `Toppings` que es el encargo de mostrar las opciones de los ingredientes que el usuario puede seleccionar para generar su pizza y, lo que es más importante, es el que renderizará cuando se pulsa sobre el botón *Next* que está situado en el componente `Base` que acabamos de ver. El código de este componente es el que mostramos a continuación:

```javascript
import React from 'react'
import { Link } from 'react-router-dom'

const Toppings = ({ addTopping, pizza }) => {
  let toppings = [
    'mushrooms', 'peppers', 'onions',
    'olives', 'extra cheese', 'tomatoes'
  ]

  return (
    <div className='toppings container'>
      <h3>Step 2: Choose Toppings</h3>
      <ul>
        {
          toppings.map(topping => {
            let spanClass = pizza.toppings.includes(topping)
              ? 'active'
              : ''
          return (
              <li 
                key={ topping } 
                onClick={ () => addTopping(topping) }
              >
                <span className={ spanClass }>{ topping }</span>
              </li>
            )
          })
        }
      </ul>
      <Link to='/order'>
        <button>
          Order
        </button>
      </Link>
    </div>
  )
}

export default Toppings;
```

Como este componente es la primera vez que aparece en tutorial para poder utilizar **Framer-motion** lo primero que tendremos que hacer es importar el componente `motion` para que lo podamos utilizar. Por lo tanto lo primero que añadiremos a nuestro componente será la siguiente:

```javascript
import React from 'react'
import { Link } from 'react-router-dom'
import { motion } from 'framer-motion`
```

Una vez hecho esto ya podemos dirigir nuestra atención al elemento `<button>` situado en la parte inferior del código JSX con el fin de animarlo. Partimos, por lo tanto, del siguietne marcado:

```javascript
<button>Order</button>
```

Como en otras ocasiones lo primero es utilizar el componente `motion` junto con el punto `.` seguido del elemento html con el que vamos a trabajar. Así en el caso de nuestro ejemplo escribiremos:

```javascript
<motion.button>Order</motion.button>
```

Ya solamente nos queda asignar el mismo objeto que en los casos anteriores a la prop `whileHover` con el fin de tener la misma animación cuando el ratón se sitúe sobre este botón. Por lo tanto escribiremos:

```javascript
<motion.button
  whileHover={{
    scale: 1.5,
    textShadow: '0px 0px 8px rgb(255, 255, 255)',
    boxShadow: '0px 0px 8px rgb(255, 255, 255)'
  }}
>
  Order
</motion.button>
```

Vamos a crear una nueva animación adicional que queremos que se aplique cuando el ratón se sitúe sobre cada una de las bases que podemos elegir para crear nuestras pizzas además de cuando nos situemos sobre cada uno de los ingredientes que queremos utilizar. En concreto la animación que queremos que se produzca consistirá en incrementar el tamaño de la fuente en la que se muestra la opción además de mostrar el color del texto a un tono amarillo. 

Ya que el último componente con el que hemos estado trabajando es el que muestra los ingredientes (el componente `Toppings`) vamos a comenzar animando cada uno de los elementos entre los que podemos elegir. Revisando el código JSX de este componente vemos que la parte del mismo que se encarga de crear el listado de los ingredientes es el siguiente:

```javascript
<li 
  key={ topping } 
  onClick={ () => addTopping(topping) }
>
  <span className={ spanClass }>{ topping }</span>
</li>
```

El primero paso que tendremos que dar será utilizar el componente `motion` junto con el punto `.` sobre el elemento html que vamos a animar. En este caso podríamos pensar que tendríamos que animar el elemento `<span>` pero si así lo hiciésemos no lograríamos que el efecto fuese consistente porque el tamaño de cada uno de los elementos dependerá del tamaño que ocupa las letras. Nuestro objetivo es que la animación sea consistente por lo que vamos a aplicar el componente `motion` sobre cada uno de los elementos de la lista:

```javascript
<motion.li 
  key={ topping } 
  onClick={ () => addTopping(topping) }
>
  <span className={ spanClass }>{ topping }</span>
</motion.li>
```

El siguiente paso consistirá en definir el valor que queremos proporcionarle al atributo `whileHover` que vamos a utilizar. Suponiendo que queremos incrementar su tamaño vamos a suponer que queremos que se incremente hasta 1.3 veces su tamaño original (cosa que vamos a lograr definiendo el atributo `scale` del objeto que le pasaremos a `whileHover`) y además querremos que el color de la opción pase a ser de una tonalidad amarilla (lo que lograremos gracias al atributo `color` del objeto que le pasaremos a `whileHover`). Así pues definimos los siguiente:

```javascript
<motion.li 
  key={ topping } 
  onClick={ () => addTopping(topping) }
  whileHover={{ scale: 1.3, color: '#f8e112' }}
>
  <span className={ spanClass }>{ topping }</span>
</motion.li>
```

Si en este momento guardamos los cambios y recargamos el componene en nuestro navegador veremos que el crecimiento en escala de cada uno de los elementos que representan a los ingredientes entre los que podemos elegir muestran un incremento hacia la izquierda lo cual puede resultar algo extraño. ¿Qué podemos hacer para solventarlo? Aquí es donde entra en juego otro de los atributos que podemos utilizar en los objetos que definen nuestras animaciones, la pr algo extraño. ¿Qué podemos hacer para solventarlo? Aquí es donde entra en juego otro de los atributos que podemos utilizar en los objetos que definen nuestras animaciones, la propiedad `originX` la cual espera recibir un valor numérico en el que podemos decir desde qué coordenada X (con respecto a la posición en la que renderiza el elemento html) queremos que se realice la animación. Como en nuestro caso queremos que la animación de la escala se lleve a cabo con respecto al lugar en el que se muestra el elemento establecermos el valor de `originX` a 0:

```javascript
<motion.li 
  key={ topping } 
  onClick={ () => addTopping(topping) }
  whileHover={{ scale: 1.3, color: '#f8e112', originX: 0 }}
>
  <span className={ spanClass }>{ topping }</span>
</motion.li>
```

---
**Nota:** si se quiere obtener más información acerca de cómo funciona la propiedad `originX` de **Framer-motion** se [recomienda leer la documentación oficial](https://www.framer.com/api/motion/component) de la librería.

---

Pero no nos vamos a quedar aquí ya que vamos a poder definir cómo queremos que se lleve a cabo la transición en la animación. Aunque en nuestro componente no hemos definido las props `initial` ni `animate` es importantae que entendamos que podemos definir el atributo `transition` para describir cómo queremos que se lleve a cabo la transición cuando el ratón pasa por encima del elemento que estamos animando. En nuestro ejemplo podríamos definir lo siguiente:

```javascript
<motion.li 
  key={ topping } 
  onClick={ () => addTopping(topping) }
  whileHover={{ scale: 1.3, color: '#f8e112', originX: 0 }}
  transition={{ type: 'spring', stiffness: 300 }}
>
  <span className={ spanClass }>{ topping }</span>
</motion.li>
```

lo que hace que se produzca un efecto de rebote para la realizar la animación de la escala del elemento como [hemos explicado](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/framer-motion/04_Transition_Options.md) anteriormente en este tutorial.

Siguiendo esto mismo para definir las animaciones relacionadas con la lista que muestra las opciones con las distintas bases que podemos elegir como base para nuestras pizzas y sin extendernos demasiado en la explicación ya que el conjunto de pasos será exactamente el mismo que para los ingredientes, partiremos del siguiente código JSX:

```javascript
<li key={ base } onClick={ () => addBase(base) }>
  <span className={spanClass}>{ base }</span>
</li>
```

Para finalizar con el siguiente código JSX:

```javascript
<motion.li 
  key={ base } 
  onClick={ () => addBase(base) }
  whileHover={{ scale: 1.3, color: '#f8e112', originX: 0 }}
  transition={{ type: 'spring', stiffness: 300 }}
>
  <span className={spanClass}>{ base }</span>
</motion.li>
```

## Resumen

En este punto hemos visto cómo podemos hacer uso de la prop `whileHover` que nos proporciona el componente `motion` para describir todas aquellas animaciones que queremos que se lleven a cabo cuando el ratón se sitúa sobre uno de los elementos que estemos animando. Además hemos visto que una vez que el ratón abandona el elemento el elemento en cuestión vuelve a su estado inicial (es decir, que se "deshace" la animación).

## Componentes finales

El aspecto final del componente `Home` tras las modificaciones que hemos llevado a cabo en este punto es el siguiente:

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
      <motion.button
        whileHover={{
          scale: 1.5,
          textShadow: '0px 0px 8px rgb(255, 255, 255)',
          boxShadow: '0px 0px 8px rgb(255, 255, 255)'
        }}
      >
        Create your pizza
      </motion.button>
    </Link>
  </motion.div>
)

export default Home
```

En el caso del componente `Base` el código final que tendremos tras la realización de las modificaciones que hemos descrito en este punto será el que se muestra a continuación:

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
              <motion.li 
                key={ base } 
                onClick={ () => addBase(base) }
                whileHover={{ scale: 1.3, color: '#f8e112', originX: 0 }}
                transition={{ type: 'spring', stiffness: 300 }}
              >
                <span className={spanClass}>{ base }</span>
              </motion.li>
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

En el caso del componente `Toppings` el código final para el mismo tras los pasos que hemos recogido en este punto tendrá el siguiente aspecto:

```javascript
import React from 'react'
import { Link } from 'react-router-dom'
import { motion } from 'framer-motion`

const Toppings = ({ addTopping, pizza }) => {
  let toppings = [
    'mushrooms', 'peppers', 'onions',
    'olives', 'extra cheese', 'tomatoes'
  ]

  return (
    <div className='toppings container'>
      <h3>Step 2: Choose Toppings</h3>
      <ul>
        {
          toppings.map(topping => {
            let spanClass = pizza.toppings.includes(topping)
              ? 'active'
              : ''
          return (
              <motion.li 
                key={ topping } 
                onClick={ () => addTopping(topping) }
                whileHover={{ scale: 1.3, color: '#f8e112', originX: 0 }}
                transition={{ type: 'spring', stiffness: 300 }}
              >
                <span className={ spanClass }>{ topping }</span>
              </motion.li>
            )
          })
        }
      </ul>
      <Link to='/order'>
        <motion.button
          whileHover={{
            scale: 1.5,
            textShadow: '0px 0px 8px rgb(255, 255, 255)',
            boxShadow: '0px 0px 8px rgb(255, 255, 255)'
          }}
        >
          Order
        </motion.button>
      </Link>
    </div>
  )
}

export default Toppings;
```
