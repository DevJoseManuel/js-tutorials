# `Typography`

En este punto vamos a aprender a utilizar el componente `Typography` de **Material UI**. Desde el punto de vista de un desarrollador que empieza a trabajar con esta librería será un de los componentes más básicos en los que se refiere a su utilización pero aún así lo desarrolladores más expertos suelen apoyarse en el mismo para resolver algunos de los casos de uso más complejos que se nos pueden presentar en nuestro día a día a la hora de diseñar una interfaz gráfica de usuario.

Si nos basamos [en la documentación oficial](https://material-ui.com/components/typography/#typography) de **Material UI** no vamos a encontrarnos con ninguna aplicación de este componente que lo pueda hacer especialmente destacable más allá de cuando queremos mostrar un texto dentro de la pantalla.

El componente `Typography` pone a nuestra disposición la prop `variant` que viene a decirnos la forma en la que se ha de mostrar el texto que contendrá el componente. Pero ¿qué es esto de la forma del texto? Pues básicamente el tamaño del texto o la etiqueta html que lo rodeará. Así tenemos como posibles valores para esta prop:

* `inherit` que viene a indicar que el tamaño con el que se renderiza el texto será heredado del elemento padre en la jerarquía de componentes html dentro del DOM de la página.
* `h1` hasta `h6` que lo que van a hacer es mostrar el contenido del componente entre las etiquetas que representan a los encabezados html `<h1>` hasta `<h6>`.
* `subtitle1` o `subtitle2` que van a servirnos para renderizar subtítulos dentro de la nuestra página. Hay que tener en cuenta que la principal diferencia entre estos dos valores es precisamente el tamaño del texto que se renderiza siendo mayor el primero que el segundo.
* `body1` o `body2` que servirá para mostrar texto que se extiendan por varias líneas de nuestra página representando el cuerpo. Al igual que en el caso anterior, la principal diferencia entre estos textos es el tamaño en el que se renderiza siendo mayor el primero que el segundo.
* `button` que sive para renderizar un texto como si se tratase de un texto que **Material UI** renderiza dentro de un botón (básicamente un texto en mayúsculas).
* `caption` que sirve para renderizar el texto con el tamaño más pequeño que nos ofrece el componente a través de la prop `variant`.
* `overline` que sirve para renderizar de tal manera que el espacio entre las letras es un poco mayor que en el caso del variant `body2` (pese a que el tamaño de las letras es el mismo en ambos casos) y además el texto se muestra siempre en mayúsculas.
* `srOnly` caso muy especial que sirve para que el texto se muestre renderizado dentro de un elemento `<span>` que tiene un tamaño de 1x1 píxeles lo que garantizará que no se verá el contenido en la página.

El valor por defecto que se le aplica a la propiedad `variant` será `body1` ya que los creadores de **Material UI** han pensado que la mayoría de las veces vamos a utilizar este componente para renderizar el contenido de los componentes `Typography`.

Un ejemplo muy sencillo para ver cómo podemos aplicar este componente dentro de nuestra aplicación sería el siguiente componente:

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import Typography from '@material-ui/core/Typography'

const useStyles = makeStyles(theme => ({
  typographyStyle: {
    color: 'blue'
  }
}))

export default function App() {
  const classes = useStyles()
  return (
    <Typography className={ classes.typographyStyle }>
      Hello world
    </Typography>
  )
}
```

La idea importante que nos tenemos que quedar aquí es que cualquier cosa que pasemos como hija (`children`) del componente `Typography` será renderizado como un párrafo. Esto quiere decir que en nuestro ejemplo cuando el código JSX es traducido a marcado html nos encontraremos algo como lo siguiente:

```javascript
<div id='root'>
  <p>Hello World</p>
</div>
```

¿Qué quiere esto decir? Pues que si queremos renderizar más de una línea dentro de componente `Typography` tenemos que tener en cuenta que todo ello será renderizado dentro de un elemento `<p>` por lo que nos tendremos que basar en el uso de otras etiquetas html como hijas del componente para lograr el efecto que estuviésemos persiguiendo. Así, si por ejemplo, queremso renderizar dos líneas con el mismo componente escribiríamos lo siguiente:

```javascript
<Typography>
  <span style={{ display: 'block' }}>Line 1</span>
  <span style={{ display: 'block' }}>Line 2</span>
</Typography>
```
## API

Vamos a echarle un vistazo a algunas de las props adicionales que podemos establecer al componente `Typography`. Para obtener una listado completo de todas las que tenemos a nuestra disposición se deberá [consultar la documentación oficial](https://material-ui.com/api/typography/#props) del componente.

### `align`

Con esta prop lo que haremos será determinar la alinación del texto dentro del componente. Esto quiere decir que si asignamos el valor `right` el texto se alineará a la derecha, si asignamos `center` al centro, `left` a la izquierda e `inherit` (que es el valor por defecto) lo que hará será heredar esta propiedad del elemento html padre que lo contendrá. Así, si queremos que el texto que contiene este componente esté alineado a la derecha simplemente escribiríamos:

```javascript
<Typography align='right'>
  Hello world
</Typography>
```

### `classes`

Se trata de la prop que nos va a permitir definir las clases CSS que queremos que se apliquen de forma específica para una instancia del componente `Typography` concreta. La forma de utilizarla junto con los valores que puede recibir es exactamente igual que en el caso de elemento `Button` que hemos visto en el [punto anterior de este manual(](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/material-ui/03_makeStyles.md) por lo que nos remitimos a ella para obtener más información al respecto.

### `color`

Se trata de la prop que podemos utilizar para definir el color con el que queremos que se muestre el texto que contiene el componente. Los valores que podemos utilizar serán los strings que se corresponden con valores de los colores que podemos definir en el tema (theme) de **Material UI** que estemos utilizando en nuestra aplicación:

* `initial` esto quiere decir que se aplicará el color por defecto que está establecido en el tema.
* `primary` o `secondary` se utilizará el color primario o secundario definido en el tema.
* `textPrimary` o `textSecondary` se tratan de dos atributos del tema que se está aplicando que están pensando exclusivamente para definir un color primerio y secundario para nuestros textos ya que no tienen por qué coincidir con los colores que hayan sido configurados como `primary` o `secondary` dentro del tema.
* `error` queremos que el texto se muestre en el color que está definido como el que se utilizará para mostrar los mensajes de error en la aplicación.

Lo que tenemos que enteder es que siempre que utilizamos **Material UI** dentro de nuestros componentes de React tendremos a nuestra disposición un que contiene las propiedades que definen el tema (theme) con todas las propiedas y valores CSS que se han de aplicar por defecto. Estos valores los [podemos consultar en la documentación oficial](https://material-ui.com/customization/default-theme/#default-theme) pero centrándonos exclusivamente en los colores veremos que sus valores están recogidos dentro del atributo `palette` que a su vez tiene recogido un atributo por cada uno de los colores que podemos utilizar.

Por ejemplo, si queremos ver qué colores están asociados a `primary` simplemente acudiremos a la documentación oficial, accedermos al atributo `palette` y dentro del mismo buscamos el atributo `primary` donde nos encontramos con los siguientes valores:

```javascript
palette: {
  // ... other attributes.
  primary: {
    light: #7986cb
    main: #3f51b5
    dark: #303f9f
    contrastText: #fff
    }
  }
```

Pero ¿cuál de todos estos valores es el que se estará aplicando cuando definimos el color en uno de los componentes? Pues por defecto el valor que estará asociado al atributo `main` lo que viene a decir que si escribimos lo siguiente:

```javascript
<Typography color='primary'>
  Hello world
</Typography>
```

Cuando este componente sea renderizado en la página el color en el que se mostrará será el #3f51b5 que es el que se corresponde con el valor asociado al atributo `main` del objeto `primary` que está asociado al atributo `palette` dentro del tema de **Material UI** que se está utilizando en la aplicación.

### `component`

En esta prop podemo asignarle como atributo un componente de React que queremos que se muestre como raíz en la jerarquía de componentes que contendrán al marcado html que está asociado al componente `Typography`. Su funcionamiento es exactamente el mismo que para el uso de [esta misma prop `component` cuando hemos explicado el componente `Button`](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/material-ui/04_Button.md) por lo que nos remitimos a ese punto del tutorial para obtener más información al respecto.


### `display`

Esta prop nos va a servir para determinar cómo queremos que se muestre el elemento html con el que se renderiza el componente desde el punto de vista del atributo CSS
`display`. Los valores que le podemos aplicar a esta prop serán:

* `initial` (valor por defecto) cuando se establece este valor no se utilizará la propiedad CSS `display` a la hora renderizar el elemento.
* `block` que servirá para definir que el elemento tenga asociado a la propiedad CSS `display` el valor `block` que lo que provoca es que el elemento cree un cuadro de elemento de bloque, es decir, que se rompe el flujo normal de renderizado de elementos de la página creando una nueva línea en la que se mostrará el contenido del elemento dentro de una caja de CSS que ocupará todo el espacio horizontal que tenga disponible a su disposición.
* `inline` lo que hace es mostrar el contenido del elemento con el valor `inline` para la propiedad CSS `display` lo que se traduce en que no se rompe el flujo de renderizado de los elementos del página pero sí que vamos a poder aplicar las propiedades de altura y anchura contenido renderizado ya que se tratará a vez como un elemento de caja de CSS (en otras palabras, el contenido se muestra como una caja CSS dentro de la línea).

---
**Nota:** para obtener más información acerca de cómo funciona la propiedad CSS `display` se recomienda [leer la documentación que la MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/display) tiene recogida al respecto.

---

### `gutterBottom`

Cuando se establece esta propiedad lo que estaremos haciendo es que el contenido del elemento `Typography` se muestre con un margen inferior con respecto al siguiente elemento html que se vaya a mostrar. Se trata de una prop binaria y en el caso de que aparezca (como en el siguiente ejemplo) la primera línea establece un valor `0.35em` con respecto a la segunda línea:

```javascript
<>
  <Typography gutterBottom>Line 1</Typography>
  <Typography>Line 2</Typography>
</>
```

### `noWrap`

Cuando se establece esta prop en el componente `Typography` (se trata de una propiedad booleana por lo que simplemente con establecer que está presente nos serviría) lo que conseguiremos es que el contenido que se muestra dentro de componente no se expanda en varias líneas en el caso de que fuese necesario ya que no cabe en dentro de una única línea, lo que puede hacer que descoloque el diseño de nuestra página si no lo controlamos. Con la prop `noWrap` lo que hacemos es que **Material UI** renderice el contenido hasta donde pueda en anchura pero además que añada tres puntos al final `...` indicando que hay más. Así si tenemos:

```javascript
<Typography noWrap>
Lorem ipsum dolor sit amet, consectetur adipiscing elit.
Aenean maximus eu turpis posuere euismod.
Nulla pharetra lorem quis luctus vehicula.
Vestibulum eleifend dolor ut sapien venenatis, nec malesuada sem aliquet.
</Typography>
```

En el caso de que no se pueda renderizar dentro del ancho que tiene asociado se muestre, por ejemplo, lo siguiente:

```javascript
<p class='MuiTypography-root MuiTypography-body1 MuiTypography-noWrap'>
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean maximus eu turpis posuere euismod. Nulla pharetra lorem quis luctus vehicula. Vestibulum eleifend dolor ut sapien venenatis, nec malesuada sem aliquet.
</p>
```

Y si acudimos a las herramientas para desarrolladores para ver cuáles son los atributos CSS que están asociados a la clase `MuiTypography-noWrap` vemos que son los siguientes:

```javascript
.MuiTypography-noWrap {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
```

### `paragraph`

En cierta medida el comportamiento de esta prop es similar a `gutterBottom` ya que lo que va a hacer es añadir un margen inferior al elemento html que se va a utilizar para renderizar el contenido del componente `Typography` aunque en este caso el valor del margen inferior que se añade es de 16 píxeles.

### `variant`

Aunque al principio de este punto hemos explicado los diferentes valores que se pueden utilizar para asignar a esta prop vamos a detenernos ahora unos instantes en estudiar con un poco más de detalle de dónde sale la información de las propiedades CSS que se utilizarán para cada uno de ellos.

Al no ser esta la primera vez que nos hacemos este tipo de pregunta en este manual lo que ya sabemos es que todos los aspectos que por defecto tienen que ver con los estilos CSS están recogidos dentro del objeto que representa el tema (theme) de **Material UI** que se está aplicando en nuestra aplicación. En el caso del texto si nos vamos [a la documentación oficial del tema por defecto](https://material-ui.com/customization/default-theme/#default-theme) podemos ver que los valores que se aplicarán están recogidos dentro del atributo `typography` donde en sus atributos están recogidos los valores por defecto que se aplicarán:

```javascript
typography: {
  htmlFontSize: 16
  pxToRem: f ()
  round: f S()
  fontFamily: ""Roboto", "Helvetica", "Arial", sans-serif"
  fontSize: 14
  fontWeightLight: 300
  fontWeightRegular: 400
  fontWeightMedium: 500
  fontWeightBold: 700
  // ... other attributes
```

lo que nos viene a decir que, por ejemplo, el tamaño por defecto de la fuente es de 14, que la familia será 'Roboto', etc. Pero no solamente eso sino que el se definen un atributo por cada una de los valores que se pueden aplicar a la prop `variant` donde se definen los valores de las propiedades CSS. En el caso, por ejemplo, del valor de la variant `h1` se establecen los siguientes valores:

```javascript
typography: {
  // ...other attributes
  h1: {
    fontFamily: ""Roboto", "Helvetica", "Arial", sans-serif"
    fontWeight: 300
    fontSize: "6rem"
    lineHeight: 1.167
    letterSpacing: "-0.01562em"
  }
}
```

### `variantMapping`

Esta prop sirve para determinar a qué elemento html queremos que se mapee cada uno de los valores que podemos aplicar a la prop `variant`. Esto lo que nos va a permitir es cambiar, si así lo necesitamos, el mapeo entre ambas entidades. Así, por ejemplo, el valor que recibe por defecto esta prop es el siguiente:

```javascript
{
  h1: 'h1',
  h2: 'h2',
  h3: 'h3',
  h4: 'h4',
  h5: 'h5',
  h6: 'h6',
  subtitle1: 'h6',
  subtitle2: 'h6',
  body1: 'p',
  body2: 'p'
}
```

Que lo que nos viene a indicar es que el `variant` `h1` es renderizado con el elemento html `<h1>`, el `variant` `h2` con el elemento html `<h2>`, etc. Si por la razón que sea necesitamos cambiar este tipo de mapeos no hace falta que especifiquemos el resto de atributos del objeto, lo que viene a decir, por ejemplo, que si queremos que el `variant` `h1` sea renderizado, por ejemplo, como un elemento html `<span>` lo que haríamos sería:

```javascript
<Typography variant='h1' variantMapping={{ h1: 'span' }}>
  Hello World
</Typography>
```

Lo que hará que cuando se renderice la página el texto *Hello World* se mostrará dentro de una etiqueta `<span>` pero, lo que es muy importante, es que a dicha etiqueta se le aplicarán todas las clases CSS que tiene asociadas el `variant` `h1`. Esto quiere decir que si:

```javascript
<Typography variant='h1'>
  Hello World
</Typography>
```

Se renderiza como:

```javascript
<h1 class='MuiTypography-root MuiTypography-h1'>Hello World</h1>
```

Si ahora escribimos lo siguiente:

```javascript
<Typography variant='h1' variantMapping={{ h1: 'span' }}>
  Hello World
</Typography>
```

Se renderizará como sigue:

```javascript
<span class='MuiTypography-root MuiTypography-h1'>Hello World</span>
```

## CSS

En la propia [documentación del componente `Typography`](https://material-ui.com/api/typography/#css) tenemos la lista de propiedades CSS que podemos sobreescribir con el uso de `makeStyles` con el fin de que definamos los estilos CSS que queremos que se apliquen para nuestros componentes `Typography` o a alguno de ellos.

[Anteriormente en este manual](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/material-ui/03_makeStyles.md) hemos visto qué es lo que podemos hacer para sobreescribir estos estilos por lo que no vamos a extendernos mucho en este apartado pero sí a realizar un pequeño ejemplo que sirva para recordar los conceptos que se explicaron anteriormente.

Supongamos, por ejemplo, que queremos cambiar el valor que se está asociado al margen inferior que añade la prop `gutterBottom` para lo cual acudimos a la documentación oficial que la propiedad CSS que nos ofrece el componente para controlar el CSS vinculado con esta prop se llama exactamente igual, es decir, `gutterBottom`. Lo que ahora deberíamos hacer es cambiar el valor de esta propiedad cuando estamos llamando al hook `makeStyles` de la siguiente manera:

```javascript
const useStyles = makeStyles({
  gutterBottom: {
    marginBottom: '0.6em'
  }
})
```

Es decir, que lo que estamos haciendo es redefinir el valor que se le asigna a la propiedad CSS `marginBottom` para que ahora pase a tener el valor 0.6em sobreescribiendo el que pudiese tener inicialmente (el valor por defecto, como hemos visto anteriormente, es de 0.35em por lo que estamos aplicando un margen inferior mayor lo que separá mucho más los elementos html).

¿Qué tenemos que hacer ahora si queremos aplicar este nuevo estilo en nuestro componente? Pues simplemente indicar en el componente que tiene la prop `gutterBottom` y además que dentro del conjunto de clases CSS que se han de aplicar que incluya la clase CSS que acabamos de definir. En definitiva, escribiremos:

```javascript
const classes = useStyles()
return (
  <Typography
    gutterBottom
    className={ classes.gutterBottom }
  >
    Hello World
  </Typography>
```

Si ahora renderizamos el componente y nos vamos a ver los estilos CSS que se están aplicando al mismo veremos que efectivamente el margen inferior ahora será de 0.6em como habíamos definido.

Conviene también recordar que tenemos a nuestra disposición un segundo mecanismo para determinar los estilos CSS que queremos que se sobreescriban únicamente para un caso en concreto que es mediante la utilización de la prop `classes` que nos ofrece varios de los componentes que **Material UI** pone a nuestra disposición como es el caso de `Typography`. Así, si la regla CSS anterior únicamente queremos que se aplique a una determinada instancia de este componente escribiríamos:

```javascript
<Typography
  gutterBottom
  classes={{
    gutterBottom: classes.gutterBottom
  }}
>
  Hello World
</Typography><>
```

## Creando nuestro propio tema (theme)

Hasta ahora hemos hablado varias veces de que **Material UI** nos proporciona un tema (theme) CSS por defecto para nuestras aplicaciones sin entrar en demasiados detalles en cómo hacerlo. En este punto vamos a crear un ejemplo muy sencillo de creación de un propio tema para nuestra aplicación dejando para más adelante un punto entero del manual para ver qué opciones tenemos para ello.

Supongamos que tenemos definido el siguiente componente dentro de nuestra aplicación en el cual lo único que estaremos haciendo es utilizar un componente `Typography` con el `variant` `h1`. Si llamamos a este componente `TypographyDemo` el código del mismo podría ser algo como lo siguiente:

```javascript
import React from 'react'
import Typography from '@material-ui/core/Typography'

export default function TypographyDemo() {
  return (
    <Typography color='primary' variant='h1'>
      Hello World
    </Typography>
  )
}
```

Si no hacemos nada más y renderizamos el componente `TypographyDemo` el color del texto que se muestra es el que está establecido como `primary` en el tema por defecto de **Material UI** que es de un color azulado.

Si ahora modificamos el valor del atributo `primary` asociado al atributo `palette` del objeto `theme` para establecer el nuevo valor para el color primario gracias al uso de la función `createMuiTheme` que nos ofrece la libería y lo aplicamos en el componente `App` (raíz de la aplicación) con un código como el siguiente:

```javascript
import React from 'react'
import TypographyDemo from './TypographyDemo'
import { ThemeProvider, createMuiTheme } from '@material-ui/core/styles'
import purple from '@material-ui/core/colors/purple'
import green from '@material-ui/core/colors/green'

const theme = createMuiTheme({
  palette: {
    primary: purple,
    secondary: green
  }
})

export default function App() {
  return (
    <ThemeProvider theme={ theme }>
      <TypographyDemo />
    </ThemeProvider>
  )
}
```

Con esto lo que estamos logrando es que cuando se renderiza el componente `TypographyDemo` en vez de que el color azulado que está establecido por defecto se muestre en un color morado ya que así lo hemos modificado a la hora de definir el tema que queremos utilizar en nuestra aplicación.

---
**Nota:** la razón por la que utilizamos colores como `purple` definidos dentro de **Material UI** es porque son objetos que la librería nos ofrece con distintos atributos en función de las propiedades CSS que se deberá aplicar. Más adelante veremos cómo se aplican estos conceptos por lo que ahora lo que nos interesará es quedarnos con la idea.

---

Pero no tenemos que quedarnos con que a la hora de definir únicamente podemos cambiar el color sino que podemos definir el tamaño asociado a los distintos `variant` que podemos utilizar con el componente `Typography`. Así, si queremos cambiar el tamaño de la fuente que se utiliza para renderizarlo utilizaríamos algo como lo siguiente:

```javascript
const theme = createMuiTheme({
  palette: {
    primary: purple,
    secondary: green
  },
  typography: {
    h1: {
      fontSize: '1em'
    }
  }
})
```

Con esto lo que estamos haciendo es que el tamaño de la fuente asociado al componente `Typography` que tiene asociado la prop `variant` con el valor `h1` pasará a ser `1em` en vez de `6rem` que es el que está establecido en el material en el tema por defecto que se aplica a **Material UI**.

## Roboto

La fuente de letra por defecto que utiliza el componente `Typography` (y la librería **Material UI**) es *Roboto* por lo que tenemos que asegurarnos de tenerla disponible dentro de la aplicación. Para ello tenemos dos mecanismos distintos: el primero de ellos consistirá en incluir una etiqueta `<link>` hacia un CDN con la información necesaria:

```javascript
<link
  rel='stylesheet'
  href='https://fonts.googleapis.com/css?family=Roboto:300,400,500,700&display=swap'
/>
```

O bien instalarla como una librería más dentro de nuestra aplicación desde la terminal de comandos:

```javascript
npm install fontsource-roboto
```

Para posteriormente realizar la siguiente instrucción `import` en el componente raíz de la jerarquía de componentes de nuestra aplicación de la siguiente manera:

```javascript
import 'fontsource-robot'
```

Cada una de estas aproximaciones tiene sus ventajas e inconvenientes por lo que se recomienda [consultar la documentación oficial](https://material-ui.com/es/components/typography/#general) para poder profundizar más sobre ello.

## Enlaces de interés

* [API de `Typography`](https://material-ui.com/api/typography/).
