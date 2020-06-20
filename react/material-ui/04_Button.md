# `Button`

En este punto del tutorial vamos a estudiar en profundidad cómo podemos utilizar el componente `Button` que nos proporciona **Material UI**. Sin duda alguna, este es uno de los componentes más simples que pone a nuestra disposición la librería pero también uno de los que probablemente más vamos a utilizar miestras estamos desarrollando nuestras interfaces de usuario.

En general utilizando **Material UI** vamos a poder distinguir dos tipos de botones diferentes: los denominados **contained** y los denominados **text** (existe un tercer tipo que son los **outlined** pero que, como veremos, se pueden considerar un caso especial de los **text**). En los próximos apartados vamos a ver cómo podemos utilizar cada un de ellos dentro de nuestros componentes.

Un detalle que queremos aclarar antes de comenzar con la explicación de este punto es que todos los detalles se refieren al componente `Button` que nos proporciona **Material UI** por lo que siempre deberemos importarlo en nuestro componente para poderlo utilizar:

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import Button from '@material-ui/core/Button'
```

Además, importaremos el hook `makeStyles` siempre que queramos definir clases CSS propias que vayamos a utilizar en nuestros componentes aplicando lo que hemos hemos descrito [en el punto anterior](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/material-ui/03_makeStyles.md).

## Contained Button

Este tipo de botones los vamos a utilizar para definir las acciones principales que podrán ser llevadas a cabo dentro de nuestra aplicación. Se distinguen del resto de botones que podamos definir en nuestra interfaz en que se mostrarán con un color de fondo y ligeramente elevados (aunque esta elevación la podemos desactivar como veremos más adelante).

Para definir un **contained** buttón deberemos importar el componente `Button` de **Material UI** especificando siempre la prop `variant` junto con el valor `contained`. Así la forma más sencilla de definición de un **contained button** es la que recogemos a continuación:

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import Button from '@material-ui/core/Button'

const useStyles = makeStyles(theme => {})

export default function App() {
  const classes = useStyles()
  return <Button variant='contained'>Default</Button>
}
```

Además, el componente `Button` nos ofrece una serie de props adicionales que no son exclusivas de los **contained button** ya que las podremos utilizar también en los otros tipos de botones que describiremos más adelante. Estas props son:

* **color**: sirve para pasarle como valor el string que representa el tipo de color CSS que queremos que se aplique a nuestro botón. Así, podremos pasarle el string `primary` si queremos que se le aplique el color primario o `secondary` si queremos que se le aplique el color secundario.
* **disabled**: si indicamos la existencia de esta prop lo que estaremos haciendo es que el botón se muestre como deshabilitado dentro de la interfaz y por lo tanto el usuario no va a poder pulsar sobre el mismo.
* **href**: en el caso de que queramos que el botón lleve a un enlace (ya sea dentro de la propia página o un enlace externo) en esta prop especificaremos el lugar al que llevará el enlace de forma similar a como lo hacemos cuando estamos describiendo una etiqueta html `<a>`.

Por lo tanto, en el siguiente ejemplo mostramos una combinación de **contained buttons** en los que estamos utilizando diferentes utilizaciones de las props:

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import Button from '@material-ui/core/Button'

const useStyles = makeStyles(theme => {})

export default function App() {
  const classes = useStyles()
  return (
    <>
      <Button variant='contained'>Default</Button>
      <Button variant='contained' color='primary'>Primary</Button>
      <Button variant='contained' color='secondary'>Secondary</Button>
      <Button variant='contained' disabled>Disabled</Button>
      <Button variant='contained' color='primary' href='#link'>Link</Button>
    </>
  )
}
```

Pero hay una prop más que tenemos que tener en cuenta y que nos va a permitir desactivar el efecto visual que muestra el botón como si estuviesa elevado y no es otra `disableElevation` que al estar presente lo que hace es mostrar un botón sin sombreado alrededor y por lo tanto sin ese efecto de elevación.

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import Button from '@material-ui/core/Button'

const useStyles = makeStyles(theme => {})

export default function App() {
  const classes = useStyles()
  return <Button variant='contained' disableElevation>Disable elevation</Button>
}
```

## Text Button

Este tipo de botones se distinguen de los **contained** en que no muestran ningún tipo de color de fondo ni elevación, es decir, que básicamente son mostrados como un texto. De hecho, cuando en uno de los componentes de nuestra aplicación importamos el componente `Button` y lo utilizamos sin especificar ninguna de las props que tiene asociadas el tipo de botón que renderizará será un **text button**.

Esto quiere decir que el siguiente código de un componente de ejemplo acabará renderizando un **text button**:

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import Button from '@material-ui/core/Button'

const useStyles = makeStyles(theme => {})

export default function App() {
  const classes = useStyles()
  return <Button>Primary</Button>
}
```

Todas las props que hemos visto anteriormes (excepto `disableElevation`) se pueden utilizar también en un **text button** lo que quire decir que los siguientes butones serán renderizados de forma correcta con un efecto similar a como se renderizan los **contained button** aplicando las mismas props pero sin el color de fondo ni la elevación. Así, en el siguiente fragmento de código mostramos diferentes posibilidades de renderizado de **text button** dentro de una aplicación:

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import Button from '@material-ui/core/Button'

const useStyles = makeStyles(theme => {})

export default function App() {
  const classes = useStyles()
  return (
    <>
      <Button>Default</Button>
      <Button color='primary'>Primary</Button>
      <Button color='secondary'>Secondary</Button>
      <Button disabled>Disabled</Button>
      <Button color='primary' href='#link'>Link</Button>
    </>
  )
}
```

## Outlined Buttons

En este caso estaremso ante un botón similar a un **text button** con la particularidad de que se mostrará el texto que lo contiene junto con el borde que representa el botón. Para mostrar un **outlined button** simplemente tendremos que establecer el valor de la prop `variant` del botón como `outlined` además de saber que el conjunto de props que admitirá será el mismo que en el caso de un **text button**, lo que quiere decir que el siguiente código muestra los mismos botones que en el punto anterior pero esta vez como **contained buttons**:

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import Button from '@material-ui/core/Button'

const useStyles = makeStyles(theme => {})

export default function App() {
  const classes = useStyles()
  return (
    <>
      <Button variant='outlined'>Default</Button>
      <Button variant='outlined' color='primary'>Primary</Button>
      <Button variant='outlined' color='secondary'>Secondary</Button>
      <Button variant='outlined' disabled>Disabled</Button>
      <Button variant='outlined' color='primary' href='#link'>Link</Button>
    </>
  )
}
```

## Tamaño de los botones

A la hora de definir nuestros botones tenemos a nuestra disposición una prop más que podemos utilizar para definir el tamaño con el que queremos que sean renderizados dentro de la página (entendiendo como tamaño las dimensiones del botón). Dicha prop se denomina `size` y admitirá como valores `small`, `medium` o `large`. Pero ¿dónde están estos tamaños definidos? La respuesta es el tema (theme) que se esté aplicando para renderizar el componente (más adelante estudiaremos en profundidad los temas por lo que en este punto simplemente tenemos que conocer la existencia de estos valores).

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import Button from '@material-ui/core/Button'

const useStyles = makeStyles(theme => {})

export default function App() {
  const classes = useStyles()
  return (
    <>
      <Button >Default</Button>
      <Button size='small'>Small</Button>
      <Button size='medium'>Medium</Button>
      <Button size='large'>Large</Button>
    </>
  )
}
```

Lo que sí que nos interesará saber es que en el caso de que no se especifique el valor de la prop `size` el tamaño con el renderizará el botón será el que está definido en el tema que estamos utilizando como `medium`.

## Botones con iconos

Otra de las posibilidades que nos ofrece **Material UI** es definir un botón que contenga en su interior un icono con el fin de mostrar de forma gráfica el tipo de acción que se llevará a cabo si es pulsado. Para ello `Button` nos ofrece dos props adicionales que podemos utilizar:

* `startIcon` al cual le podemos asignar el icono que queremos que se muestre a la izquierda del botón, es decir, que si nuestro botón contiene un texto primero se mostrará el icono para posteriormente mostrar el texto del botón.
* `endIcon` similar al anterior pero en este caso el icono será mostrado tras el texto que está contenido en el botón, es decir, a la derecha.

Pero ¿cómo se representa en **Material UI** un icono? Pues lo que tenemos que saber es que cada uno de ellos es a su vez un componente de React dentro de la propia librería y es el componente el que tendremos que pasarle como valor a las props `startIcon` o `endIcon`. ¿Qué quiere decir esto a efectos prácticos? Pues que antes de utilizarlos los tendremos que importar en el código de nuestro componente:

```javascript
import DeleteIcon from '@material-ui/icons/Delete'
import CloudUploadIcon from '@material-ui/core/CloudUpload'
```

Y ahora ya podemos utilizarlos en nuestro componente como cualquiera de los botones que hemos visto hasta ahora:

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import Button from '@material-ui/core/Button'
import DeleteIcon from '@material-ui/icons/Delete'
import CloudUploadIcon from '@material-ui/icons/CloudUpload'

const useStyles = makeStyles(theme => {})

export default function App() {
  const classes = useStyles()
  return (
    <>
      <Button
        variant='contained'
        startIcon={ <DeleteIcon /> }
      >
        Delete
      </Button>
      <Button
        variant='contained'
        startIcon={ <CloudUploadIcon /> }
      >
        Upload
      </Button>
    </>
  )
}
```

---
**Nota:** si nos fijamos en el código anterior los iconos que estamos utilizando estarán recogidos dentro de la `@material-ui/icons` por lo que para que los podamos utilizar dentro de nuestro componente no solamente los tendremos que importar sino que además tendremos que asegurarnos de que la librería esté instalada en la aplicación.

---
**Nota:** para obtener más información de los iconos que tenemos a nuestra disposición gracias a **Material UI** se recomienda leer la [documentación oficial](https://material-ui.com/components/icons/#icons) de la librería.

---

¿Qué sucede en el caso de que queramos mostrar un botón que únicamente contenga un icono en su interior? Pues que no deberíamos utilizar el componente `Button` con el que hemos estado trabajando hasta ahora sino que **Material UI** pone a nuestra disposición el componente `IconButton` para ello. Este componente espera recibir como hijo un componente que represente a un icono lo que hará nuestro código mucho menos verboso.

Así si queremos representar el conjunto de botones anteriores sin el texto asociado a los mismos haríamos uso del componente `IconButton` de la siguiente manera:

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import IconButton from '@material-ui/core/IconButton'
import DeleteIcon from '@material-ui/icons/Delete'
import CloudUploadIcon from '@material-ui/icons/CloudUpload'

const useStyles = makeStyles(theme => {})

export default function App() {
  const classes = useStyles()
  return (
    <>
      <IconButton>
        <DeleteIcon />
      </IconButton>
      <IconButton>
        <CloudUploadIcon />
      </IconButton>
    </>
  )
}
```

Así pues podemos concluir que lo que está haciendo el componente `IconButton` es convertir un icono normal de **Material UI** que podemos estar utilizando en cualquier lugar de nuestra aplicación en un botón y por lo tanto dotarlo de la capacidad de iteractuar con el usuario.

## API

En este apartado vamos a ver alguna de las props adicionales que tenemos que a nuestra disposición cuando estamos trabajando con el componente `Button`:

### `component`

Esta props (si se define) sirve para especificar con qué elementos html diferentes de `<button>` queremos que se renderice el botón (podría ser, por ejemplo, el elemento `<a>`). Si precisamos especificarlo simplemente indicaremos como un string el elemento (en el caso del elemento `<a>` especificaríamos como valor `a`). 

```javascript
<Button component='a'>My Button</Button>
```

También puede recibir como valor un componente de React que sea el que envuelva al marcado del botón pero indicado como un componente de la siguiente manera:

```javascript
<Button component={ <MyComponent /> }>My Button</Button>
```

### `disableFocusRipple` y `disableRipple`

Cuando se pulsa sobre cualquiera de los botones que se renderizan gracias a **Material UI** por defecto se produce un efecto de honda que acompaña al click y hace que el botón se vea más vistoso de cara al usuario. Ahora bien, si este efecto no nos gusta o simplemente queremos desactivarlo lo que tendremos que hacer es simplemente pasarle estas dos props al botón:

```javascript
<Button disableFocusRipple disableRipple>No Ripple</Button>
```

### `fullWidth`

Cuando establecemos esta prop en uno de los botones de **Material UI** lo que estaremos indicando es que queremos que se renderice ocupando todo el ancho que tiene disponible para ello. La forma de indicarlo es tan sencilla como lo siguiente:

```javascript
<Button fullWidth>No Ripple</Button>
```

## CSS

Vamos a ver ahora cómo podemos apliar estilos a nuestros botones de forma parecido a [como hemos explicado anteriormente](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/material-ui/03_makeStyles.md) gracias al uso del hook `makeStyles`. Vamos a partir de un ejemplo básico en el que queremos aplicar unas propiedades CSS básicas a un botón concreto:

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import Button from '@material-ui/core/Button'

const useStyles = makeStyles(theme => ({
  root: {
    color: 'red',
    border: 'none'
  }
}))

export default function App() {
  const classes = useStyles()
  return (
    <>
      <Button
        className={ classes.root }
        variant='outlined'
        color='primary'
        size='small'
      >
        Small Icon
      </Button>
    </>
  )
}
```

Con la declaración anterior lo que hemos logrado es sobreescribir los estilos que se aplicarn a un botón de tipo `outlined` haciendo que no se muestre el border que normalmente lo rodea ya que hemos definido atributo `border` con el valor `none` en el objeto `classes.root` además de que el color del texto sea rojo en lugar del color `primary` que estará asociado al tema de **Material UI** que se está definiendo por defecto.

Ahora bien, en **Material UI** tenemos otra prop dentro del componente `Button` que nos va a permitir asignar directamente un objeto para especificar las clases CSS que queremos que se aplique en el renderizado del mismo y que se denomina `classes`. ¿Cómo podemos utilizarlo para lograr el mismo renderizado que en el caso anterior? Pues con el siguiente marcado JSX:

```javascript
<Button
  classes={{
    root: classes.root
  }}
  variant='outlined'
  color='primary'
  size='small'
>
  Small Icon
</Button>
```

Pero ¿qué diferencia existe entre utilizar `className` y `classes`? ¿Por qué utilizar la segunda prop en lugar de la primera? ¿Cuándo conviene utilizarla? Pensemos en el caso de uso en el que únicamente queremos definir un determinado conjunto de propiedades CSS para un determinado botón de nuestro componente. Si no hacemos uso de `classes` tendríamos que contruir el caso en concreto mediante la invocación de `makeStyles` cuando solamente lo vamos a utilizar una vez cosa que, en principio, no parece muy razonable. Por eso es mejor que en casos como este definamos mediante el objeto que se asigna a la prop `classes` tenga los valores de las propiedades CSS que queremos modificar.

Otro caso de uso en el que es interesante utilizar la prop `classes` en vez de `classNames` es que en aquellos en los que queremos que por la razón que sea ante un determinado tipo de botón que cumple con una serie de condiciones (como que esté en un lugar en concreto dentro de la página, o sea de un determinado `variant` o con un `color`) querramos espeficar unos determinados valores para las propiedades CSS.

---
**Nota:** para obtener la lista de propiedades CSS que podemos modificar cuando estamos trabajando con un `Button` siempre podemos consultar [la documentación oficial]](https://material-ui.com/api/button/#css) para conocer el conjunto de ellas y los valores que permiten.

---

## `ButtonGroup`

**Material UI** nos ofrece el componente `ButtonGroup` para conseguir agrupar varios botones formando una fila de ellos con la característica adicional de que todos ellos compartirán las mismas características (props). Un ejemplo de su utilización sería la siguiente:

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import ButtonGroup from '@material-ui/core/ButtonGroup'
import Button from '@material-ui/core/Button'

const useStyles = makeStyles(theme => {})

export default function App() {
  const classes = makeStyles()
  return (
    <>
      <ButtonGroup color='primary'>
        <Button>One</Button>
        <Button>Two</Button>
        <Button>Three</Button>
      </ButtonGroup>
    </>
  )
}
```

Vemos por lo tanto que el componente `ButtonGroup` sirve para agrupar uno o más componentes `Button` a los que con las props que especificamos a `ButtonGroup` se aplicarán a todos los botones que aglobarán. Para lograrlo tenemos a nuestra disposición las siguientes props:

* **size**: que nos va a permitir especificar el tamaño de los botones asignándole los valores `small`, `medium` y `large` (siendo la que está especificada por defecto el valor `medium`).
* **color**: el color que queremos que se aplique siendo los valores posibles `primary`, `secondary` o `default` (siendo este último el valor que está especificado por defecto).
* **orientation**: sirve para indicar si queremos que el grupo de botones ha de ser mostrado en horizontal (valor `horizontal`, que es el valor por defecto si no se especifica nada) o en vertical, es decir, en columna (valor `vertical`).

---
**Nota:** se puede obtener más información del componente `ButtonGroup` [consultando la documentación oficial](https://material-ui.com/components/button-group/) de **Material UI**.

---

## Gestión de los click

Hasta aquí todo lo que hemos comentado nos permitirá renderizar un botón dentro de la página pero ¿cómo hacemos para gestionar los click que se puedan hacer en el botón? Para ello tendremos que hacer uso de la prop `onClick` la cual espera recibir como valor una función que queremos que se ejecute cada vez que se produzca un click en el botón. Así podríamos definir lo siguiente:

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import Button from '@material-ui/core/Button'

const useStyles = makeStyles(theme => {})

export default function App() {
  const classes = makeStyles()
  return <Button onClick={ () => console.log('clicked') }>Click me</Button>
}
```

Vemos, por lo tanto, que la forma en la que definir la función que se encargará de gestionar los clicks que se llevan a cabo sobre un `Button` no sea distinta de cómo se define para un elemento `<button>` tradicionalmente en una aplicación de React.

## Aspectos adicionales

En la documentación del componente `Button` dentro de **Material UI** tenemos algunos ejemplos adicionales indicando casos de uso algo más complejos de cómo podemos utilizar los botones dentro de nuestra aplicación::

* [Botón para subir un fichero](https://material-ui.com/components/buttons/#upload-button).
* [Propiedades CSS personalizadas](https://material-ui.com/components/buttons/#customized-buttons).
* [Botones más complejos](https://material-ui.com/components/buttons/#complex-buttons).
* [Limitaciones](https://material-ui.com/components/buttons/#limitations).

## Enlaces de interés

* [Botones en Material UI](https://material-ui.com/components/buttons/).
* [API de `Button`](https://material-ui.com/api/button/).
* [API de `ButtonBase`](https://material-ui.com/api/button-base/).
* [API de `IconButton`](https://material-ui.com/api/icon-button/).
* [API de `ButtonGroup`](https://material-ui.com/components/button-group/#button-group).
