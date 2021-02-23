# `makeStyles`

En este punto de tutorial vamos a centrarnos en el hook `useStyles` que nos proporciona **Material UI** viendo tanto aspectos básicos de su utilización como aquellos casos de uso más avanzados además de estudiar las diferencias con respecto a soluciones anteriores que teníamos a nuestra disposición para aplicar los estilos CSS en nuestros componentes como podría ser el uso del *Higher Order Component* denomindo `withStyles`.

El mecanismo en que se basa **Material UI** para poder aplicar estilos CSS dentro de nuestros componentes pasa por lo que se conoce como *CSS in JS* que no viene más que a decir que es dentro del código JavaScript con el que definimos nuestros componentes donde también declararemos los estilos que queremos aplicar. Esto combinado con la forma que tenemos dentro del código JSX para definir qué clases CSS queremos que se apliquen a cada elemento de nuestra página (es decir, con saber que tenemos que utilizar el atributo `className`) es todo el conocimiento de partida para nuestro estudio.

Lo primero que vamos a hacer es presentar un ejemplo muy sencillo en el que mostramos el componente de React que está haciendo uso del hook `makeStyles`:

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import Button from '@material-ui/core/Button'

const useStyles = makeStyles({
  buttonStyle: {
    color: 'red',
    border: 'none'
  }
})

export default function App() {
  const classes = useStyles()
  return (
    <Button className={ classes.buttonStyle }>
      Small Button
    </Button>
  )
}
```

El código anterior es muy sencillo de entender. Lo que estamos haciendo es renderizar un `Button` de **Marial UI** al que le estamos aplicando como `className` el atributo `buttonStyle` que está asociado al objeto `classes`. Ahora bien, ¿qué es el objeto `classes`? Pues no es más que una referencia al objeto que ha sido llamado al invocar a la función `useStyles`.

¿Y `useStyles` dónde está definida? Pues en el propio código de nuestro componente una líneas más arriba siendo esta la función que retorna la invocación del hook `makeStyle` (propio de **Material UI**) la cual al ser a su vez invocada retornará un objeto que contiene un atributo `buttonStyle` como consecuencia de que el objeto que pasamos como parámetro a `makeStyle` tiene definido ese atributo. ¿Lioso verdad? Veámoslo desde otro punto de vista:

1. Estamos invocando al hook `makeStyle` pasándole como parámetro un objeto que contendrá un atributo por cada uno de los estilos CSS que luego queremos tener a nuestra disposición.
2. El resultado de la invocación anterior (que será una función) se lo asignamos a un variable.
3. Dentro del código de nuestro componente invocamos a la función definida en el punto 2 la cual nos retornará un objeto que tendrá un atributo por cada uno de los atributos que han sido pasados como parámetro a la hora de invocar al hook en el paso 1.
4. Cada uno de los atributos del objeto que tenemos en 3 lo podemos utilizar como valor para el atributo `className` en cualquiera de los elementos de nuestro componente.

Al final en nuestro ejemplo lo que estaremos logrando es que cada vez que se renderice nuestro componente se muestre un botón en el que las letras se mostrarán en un texto de color rojo y además que no tenga un borde asociado.

La idea que hay detrás del hook `makeStyles` es que se le pase como parámero un objecto donde como atributos se definirán el nombre de las clases CSS que luego queramos aplicar como valores de los atributos `className` de los elementos de la página. Asignadas a cada una de estas clases tendremos un objeto donde se definirán como atributos las propiedades CSS que describirán la clase (en notación *camelCase*).

Es importante recordar que la llamada al hook `makeStyles` lo que provocará es el retorno de una función que tradicionalemente recibirá el nombre de `useStyles` por lo que es bueno que desde este momento adoptemos este patrón de nomenclatura.

En el momento en el que dentro de nuestro componente estamos llamando a la función `useStyles` lo que estaremos logrando es que se creen todas las clases que definen las deferentes clases CSS que tenemos a nuestra disposición. Pero ¿qué es lo que veríamos en el caso de que escribir por la consola el objeto? Es decir, ¿qué veríamos ante un código como el siguiente?

```javascript
const classes = useStyles()
console.log(classes)
```

La respuesta es que tendremos algo como lo siguiente:

```javascript
{ buttonStyle: 'makeStyles-buttonStyle-2' }
```

Es decir, qeu el valor que tiene asociado el atributo `buttonStyle` no es establecido por nosotros directamente en nuestro código sino que será **Material UI** el que se encargará de generar un nombre lo suficientemente único como para que no pueda coincidir con otro que esté asociado a otra clases CSS `buttonStyle` garantizando de esta manera que será único.

Es más, si inspeccionásemos el código html que ha sido generado a partir del marcado JSX anterior ¿dónde aparecerá el nombre de la clase que acabamos de crear? La respuesta la podemos ver a continuación:

```html
<button
  class='MuiButtonBase-root MuiButton-root MuiButton-text makeStyles-buttonStyle-2'
  tabindex='0'
  type='button'
>
  <span class='MuiButton-label'>Small Button</span>
  <span class='MuiTouchRipple-root'></span>
</button>
```

Es decir, que el nombre de la clase que se ha construido y asignado al atributo `buttonStyle` del objeto que define nuestros estilos personalizados se añade al atributo `class` del elemento html `<button>` que representa al botón que estaremos renderizando. De hecho, si produndizamos un poco más en el nombre de esta clase CSS veremos que sigue el siguiente patrón: `makeStyles-[atributo_objeto]-[número]`.

Pero ¿qué atributos CSS vamos a poder sobreescribir con la llamada al hook `makeStyles`? Pues dependerá del componente de **Material UI** con el que estemos trabajando. La respuesta correcta siempre estará consultando la documentación oficial en el web de [**Material UI**](https://material-ui.com/) donde, por ejemplo, para el caso del componente `Button` en la [documentación dentro de la API](https://material-ui.com/api/button/) podemos ver todas los atributos CSS con los podemos interactuar así como las clases CSS que la librería asígna a cada una de las clases CSS que por defecto la librería le asignará.

Por lo tanto, lo que nos tiene que quedar claro, es que en cualquier otro componente de nuestra aplicación vamos a poder volver a invocar al hook `makeStyles` y definir como atributo del objeto que retorna `buttonStyle` con la garantía de que no va a sobreescribir ni va a entrar en conflicto con las propiedades CSS que hemos definido anteriormente ya que durante el proceso de renderizado de ambos componentes se crearán sendas clases CSS `makeStyles-buttonStyle-[número]` pero **Material UI** nos garantiza que el número que irá como sufijo en cada una de ellas será diferente.

¿Qué podemos hacer para definir un nuevo estilo personalizado para nuestro componente? Pues simplemente hacer que el objeto que retorna `makeStyles` contenga un nuevo atributo que contenga las propiedades CSS que definen al estilo. Así si queremos definir un nuevo atributo para el texto al que denominaremos `textStyle` escribiríamos algo como lo siguiente:

```javascript
const useStyles = makeStyles({
  buttonStyle: {
    color: 'red',
    border: 'none'
  },
  textStyle: {
    color: 'green'
  }
})
```

Si ahora queremos aplicar este nuevo estilo dentro de nuestro componente aplicado, por ejemplo, a un nodo html `<h1>` escribiríamos algo como los siguiente:

```javascript
export default function App() {
  const classes = useStyles()
  console.log(classes)
  return (
    <>
      <Button className={ classes.buttonStyle }>
        Small Button
      </Button>
      <h1 className={ classes.textStyle }>
        This is text
      </h1>
    </h1>
  )
}
```

Con esto lograremos que el texto que está contenido dentro de la etiqueta `h1` se muestre en color verde porque así lo hemos establecido en las propiedades CSS. Pero si nos vamos a la consola de JavaScript dentro de las herramientas para desarrolladores podemos obtener más información acerca del objeto que nos ha retornado `makeStyles`:

```javascript
{ buttonStyle: 'makeStyles-buttonStyle-5', textStyle: 'makeStyles-textStyle-6' }
```

Vemos que el patrón de construcción del nombre de la clase CSS que está asociado a cada una de las propiedades del objeto que nos retorna `makeStyles` sigue siendo el mismo que hemos explicado anteriormente pero además, y esto es realmente importante, que como se ha vuelvo a recargar la página el nombre de la clase CSS que estaba anteriormente asociado a `buttonStyle` tiene una probabilidad muy alta de haber cambiado debido a que **Material UI** generará una nueva clase CSS por nosotros.

## Estilos condicionales

Una vez que hemos visto cómo funciona `makeStyles` vamos a ir un paso más allá y ver cómo podemos hacer en todas aquellas situaciones en las que necesitemos aplicar un estilo CSS de forma condicional a uno o más elementos de nuestro componente. Para ello vamos a construir una pequeña aplicación para entenderlo. Partiremos de un componente que funciona como la raíz de la jerarquía de componentes la cual lo único que hará será renderizar otro componente (que vamos a denominar `CoolButton`) pasándole además una prop que nos va a servir para determinar el estilo CSS que dentro del mismo deberemos aplicar:

```javascript
import React from 'react'
import CoolButton from './CoolButton'

export default function App() {
  const cool = false
  return <CoolButton cool={ cool } />
}
```

Veamos ahora el código de `CoolButton` el cual inicialmente tiene el siguiente aspecto (no estamos teniendo en cuenta el valor asignado a la prop `cool` para determinar qué estilo CSS se tiene que aplicar):

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import Button from '@material-ui/core/Button'

const useStyles = makeStyles({
  buttonStyle: {}
})

export default function App() {
  const classes = useStyles()
  return (
    <Button className={ classes.buttonStyle }>
      The Button
    </Button>
  )
}
```

Lo que pretendemos lograr en este ejemplo es que en el caso de que el valor de la prop `cool` sea `true` se ha de mostrar el texto del botón con un color azul mientras que si es `false` lo que vamos a hacer es que se muestre en color rojo (es decir, que queremos utilizar la propiedad CSS `color`). Además para lograrlo queremos utilizar el hook `makeStyles` y más concretamente el objeto que estará asociado al atributo `buttonStyle` que estamos definiendo para el mismo. ¿Cómo podemos lograrlo? La respuesta es utilizando una función que dependa de las props que recibe el componente de la siguiente manera:

```javascript
const useStyles = makeStyles({
  buttonStyle: {
    color: props => props.cool ? 'blue' : 'red'
  }
})
```

Ahora bien, si escribirmos este código directamente en nuestro componente no va a funcionar porque no sabrá qué es `props`. Como sucede con cualquier otro *Functional Component* de React si dentro de un componente queremos tener acceso a las props tendremos que pasárselas como parámetro a la función que define el componente. Esto se traduce en que deberemos escribir lo siguiente:

```javascript
export default function App(props) {
```

El siguiente paso que tendremos que dar será pasar estar props a nuestra función `useStyles` como un parámetro para que estén dispibles dentro de la misma. Esto quiere decir que escribiremos:

```javascript
export default function App(props) {
  const classes = useStyles(props)
```

Y ya estaría porque con esto lo que estamos haciendo es que las props estén accesibles dentro del hook `makeStyles` y así poderlas utilizar.

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import Button from '@material-ui/core/Button'

const useStyles = makeStyles({
   buttonStyle: {
    color: props => props.cool ? 'blue' : 'red'
  }
})

export default function App(props) {
  const classes = useStyles(props)
  return (
    <Button className={ classes.buttonStyle }>
      The Button
    </Button>
  )
}
```

De hecho podemos ir un paso más allá haciendo que un determinado atributo del objeto que retorna `makeStyles` dependa en su totalidad de la ejecución de una función y que el resultado de esa función dependa de las props del componente. Esto, siguiendo con nuestro ejemplo, sería algo como lo siguiente:

```javascript
const useStyles = makeStyles({
  buttonStyle: props => {
    return {
      color: props.cool ? 'blue' : 'red'
    };
  }
})
```

Es decir que no estamos definiendo cada uno de los atributos CSS de forma individual sino que hacemos que la función invocada retorne un objeto con dichos atributos CSS y los valores que tendrá asociados han de ser los valores que se asignarán a cada esas propiedades (de hecho el código que acabamos de mostrar y el código anteriormente utilizado para el determinar el color del texto de nuestro botón producirán el mismo resultado). La ventaja de esta segunda notación la podemos ver rápidamente si queremos definir un segundo atributo CSS en función de la prop `cool`:

```javascript
const useStyles = makeStyles({
  buttonStyle: props => {
    return {
      color: props.cool ? 'blue' : 'red',
      backgroundColor: props.cool ? 'orange' : 'yellow'
    };
  }
})
```

## Utilizar un tema (theme)

Ahora que sabemos cómo podemos utilizar las props de un componente para determinar qué propiedades CSS y sus valores asociados se han de aplicar para renderizar su contenido ¿y si queremos utilizar un tema (theme) para mostrarlo garantizándonos de esta manera una consistencia entre todos los elementos de nuestra aplicación? ¿Qué podemos hacer al respecto?

Continuando con nuestro ejemplo vamos a hacer que el botón que renderiza `CoolButton` posea unas dimensiones u otras en función de cuál sea el tamaño del dispositivo en el que se está mostrando. Para ello lo primero que deberemos conocer es que el componente `Button` de **Material UI** admite una prop denominada `fullWidth` que en el caso de estar presente lo que hace es que el botón se renderice ocupando todo el ancho que tiene a su disposición dentro del elemento html en el que se encuentre situado. Así, si queremos inicialmente el botón ocupe todo el espacio disponibles escribiríamos:

```javascript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import Button from '@material-ui/core/Button'

const useStyles = makeStyles({
   buttonStyle: {
    color: props => props.cool ? 'blue' : 'red'
  }
})

export default function App(props) {
  const classes = useStyles(props)
  return (
    <Button 
      fullWidth
      className={ classes.buttonStyle }
    >
      The Button
    </Button>
  )
}
```

¿Cómo hacemos ahora para pasar un tema (theme) y que este pueda ser utilizado dentro del hook `makeStyles`? La respuesta es que tenemos una versión sobrecargada de este hook que admite como parámetro una función la cual tiene como parámetro el tema que se esté aplicando. Así, si queremos sobreescribir nuestra implementación anterior para `makeStyles` de tal manera que admita una función como parámetro y que nos retorne el mismo resultado que hasta ahora escribiríamos lo siguiente:

```javascript
const useStyles = makeStyles(theme => ({
   buttonStyle: {
    color: props => props.cool ? 'blue' : 'red'
  }
}))
```

Para entender cómo funciona esta implementación lo primero que tenemos que saber es que con **Material UI** siempre tenemos un tema por defecto (podemos ver sus valores [en la documentación oficial](https://material-ui.com/customization/default-theme/#default-theme)) en el que se establecen colores, tamaños, espaciados, etc. Más adelante en este tutorial veremos cómo crear nuestros propios temas para ser utilizados con **Material UI** por lo que ahora no vamos a preocuparnos en absoluto por entender la información que está recogida en la documentación oficial sino que vamos a centrarnos en el caso que estamos intentando resolver. Vamos a partir definiendo que el color por defecto para nuestro botón va a ser el rojo (ya no tendrá que ver con las props):

```javascript
const useStyles = makeStyles(theme => ({
   buttonStyle: {
    color: 'red'
  }
}))
```

Pero vamos a definir el color que se tiene que aplicar en el caso de que el dispositivo en el que se esté mostrando nuestro botón tenga un tamaño superior a 600 píxeles sea diferente al rojo. Pero ¿cómo podemos indicar a **Material UI** que un estilo CSS se ha de aplicar cuando el tamaño sea mayor de una determinada cantidad de píxeles? Pues aquí es donde entra en juego el objeto `theme` que se le pasado como parámetro al hook `makeStyles`. Este objeto posee a su vez un atributo denominado `breakpoint` el cual tiene también asignado un objeto cuya utilidad es establecer puntos de ruptura (breakpoints) para definir nuevas reglas CSS (suena complejo pero con el ejemplo quedará mucho más claro).

¿Y cómo podemos usar el objeto `breakpoint`? Pues sabiendo que nos ofrece el método `up` donde le pasamos como parámetro un tamaño a partir del cual queremos que se aplique la regla en cuestion. Ahora bien, ¿cómo expresamos que sea superior a 600 píxeles? Pues la respuesta pasa por consultar la [documentación oficial asociada a `theme`](https://material-ui.com/customization/default-theme/#default-theme)] y más concretamente a los valores que están asociados a `breakpoints.values` donde veremos que el atributo `sm` se corresponde precisamente con ese valor:

```javascript
breakpoint.values: { 
    xs: 0,
    sm: 600,
    md: 960,
    lg: 1280,
    xl: 1920
}
```

Con todo esto en mente vamos a ver cómo lo aplicamos para definir el color de nuestro botón. Lo primero que tendremos que hacer será establecer la llamada al método `up` entre corchetes, lo cual hará que se ejecute ese código cuando se cumpla con esa condición y asignarle como resultado el objeto que contendrá todas aquellas propiedades CSS que queremos que se sobreescriban cuando se produzca la condición. Esto que es complejo de explicar es sencillo de ver en el código:

```javascript
const useStyles = makeStyles(theme => ({
   buttonStyle: {
    color: 'red'
    [theme.breakpoint.up('sm')]: {
      color: 'blue'
    }
  }
}))
```

Es más, no hay nada que nos impida que trabajemos con las prop y el objeto `theme` cuando estemos definiendo el objeto con los estilos CSS que aplicaremos a nuestro componente. Esto quiere decir que algo como lo siguiente es correcto:

```javascript
const useStyles = makeStyles(theme => ({
   buttonStyle: {
    color: props.cool ? 'blue' : 'red',
    [theme.breakpoint.up('sm')]: {
      color: 'cyan'
    }
  }
}))
```

Con el código anterior lo que estaremos definiendo es que siempre que la prop `cool` sea `true` el texto que acompañará al botón se mostrará en color azul mientras que si la prop adopta el valor `false` el color del texto pasará a ser rojo. Ahora bien, al definir el punto de interrupción con el objeto `theme` en el momento en el que la interfaz de usuario disponga de más de 600 píxeles horizontales el color del texto siempre se mostrará en color cyan.

## Aplicando más de un estilo a un componente.

Vamos a centrarnos ahora en ver cómo podemos aplicar más de uno de los estilos que definimos con `makeStyles` a uno de los elementos html que renderiza nuestro componente. Para ello vamos a modificar ligeramente el objeto que recibe como parámetro con el fin de tener dos atributos asociados al mismo:

```javascript
const useStyles = makeStyles(theme => ({
   buttonText: {
    color: props.cool ? 'blue' : 'red',
    [theme.breakpoint.up('sm')]: {
      color: 'cyan'
    }
  },
  buttonBackground: {
    backgroundColor: 'red'
  }
}))
```
Lo que hemos hecho ha sido definir los atributos `buttonText` y `buttonBackground` por separado siendo el primero el que se encargará de definir el color con el que se mostrará el texto de un botón y el segundo el que se encargará de definir el color de fondo. Está claro que ambos propiedades se podrían haber definido bajo un mismo atributo pero este ejemplo tan simple nos servirá para ver cómo podemos utilizar dos o más de ellos en un mismo elemento. 

Tal y como tenemos hasta ahora definido al botón que renderiza nuestro componente únicamente se le estará aplicando el estilo `buttonText`:

```javascript
<Button 
  fullWidth 
  className={ classes.buttonText }
>
  The Button
</Button>
```

Si queremos aplicar el estilo `buttonBackground` podemos escribir algo como lo siguiente:

```javascript
<Button 
  fullWidth 
  className={ (classes.buttonText, classes.buttonBackground) }
>
  The Button
</Button>
```

El problema que se nos presentará al utilizar los paréntesis para que una o más clases CSS se apliquen a nuestro elemento CSS es que únicamente se acabará aplicando la última de ellas es decir, que el botón se mostrará con el texto en el color por defecto que el navegador tenga definido para los botones mientras que sí que se aplicará el color de fondo. Entonces ¿qué podemos hacer para solucionarlo?

La respuesta pasa por utilizar la librería **classnames** para lo cual lo primero que tendremos que hacer es importarla en nuestro proyecto como si se tratase de cualquier otra librería para posteriormente utilizarla dentro de nuestro componente. En concreto utilizaremos la función `classNames` que nos ofrece la librería por lo que realizamos la siguiente importación:

```javacript
import React from 'react'
import { makeStyles } from '@material-ui/core/styles'
import Button from '@material-ui/core/Button'
import classNames from 'classnames'
```

Ahora simplemente en a la hora de definir qué clases queremos aplicar a nuestro elemento html lo que hacemos es pasarlas como parámetros a la función `classNames` la cual se encargará de combinarlas de tal manera que se apliquen ambas:

```javascript
<Button 
  fullWidth 
  className={ classNames(classes.buttonText, classes.buttonBackground) }
>
  The Button
</Button>
```

Con esto nos garantizamos que tanto la clase CSS que será contruida como consecuencia de la definición del estilo `buttonText` como la clase CSS que se generará como consecuencia de `buttonBackground` ambas serán aplicadas a nuestro botón. Pero ahí no acaba la cosa ya que `classNames` nos va a permitir añadir clases CSS de forma condicional en función de lo que nuestra aplicación demante, permitirá añadir tantas clases como necesitemos, etc.

---
**Nota:** para obtener más información de cómo funciona la librería **classnames** se recomienda [consultar la página en GitHub](https://github.com/JedWatson/classnames) donde se manteniene el código de la misma.

---

## `styled` 

**Material UI** nos ofrece otro mecanismo para poder definir los estilos que queremos que se apliquen en los componentes que formarán nuestra aplicación que es el uso de la función `styled`. No vamos a detenernos en su explicación ya que no suele tratarse de una opción muy utilizada pero sí que vamos a mostrar un ejemplo de cómo se debería invocar:

```javascript
import React from 'react'
import { styled } from '@material-ui/core/styled'
import Button from '@material-ui/core/Button'

const MyButton = styled(Button)({
  background: 'red',
  border: 0
})
```

Básicamente `styled` es una función que espera decibir como parámetro un componente retornando a su vez una función que puede ser invocada recibiendo como parámetro un objeto donde se definen los atributo CSS que queremos que se apliquen sobre le componente que se ha definido como parámetro de `styled`. El resultado de esta invocación será un nuevo componente similar al original (en nuestro caso igual que el componente `Button`) pero sobre el que se aplicarán los estilos que hayamos definido.

## `withStyles`

El uso de `withStyles` se basa en la aplicación de un Higher Order Component que lo que hará será aplicar los estilos CSS que definamos para nuestros componentes, siendo este una de las formas más comunes para afrontar la solución al problema de cómo aplicar los estilos CSS a nuestros componentes. Es más, este mecanismo es tan sumamente popular que lo más probable es que en cualquier proyecto que utilicen **Material UI** antes de la aparición de `makeStyles` sea la solución que esté implementada.

¿En qué se basa esta aproximación? La idea parte de la definición de un objeto que ha de contener como atributos las clases CSS con las reglas que han de ser aplicadas para definir el estilo de los componentes de nuestra aplicación. Así, por ejemplo, podríamos definir el siguiente objeto:

```javascript
const styles = {
  root: {
    border: 0,
    color: 'blue'
  }
}
```

Ahora lo que tendremos que hacer será, en primer lugar, importar el función `withStyles` como si se tratase de cualquier otra función dentro del componente:

```javascript
import { withStyles } from '@material-ui/core/styles'
```

Siendo el siguiente paso que tenemos que dar realizar la invocación de la misma en el momento en el que vamos a hacer la exportación del componente para que actúe como un Higher Orden Component de tal manera que el componente que se estará realmente exportando será el que se obtiene como resultado la invocación de la función `withStyles` (a la que se invocará recibiendo como parámetro el objeto con las diferentes clases CSS que queremos que se apliquen), componente que tendrá todos los estilos CSS que hayamos definido. Por lo tanto escribiremos:

```javascript
export default withStyles(styles)(MyComponent)
```

¿Cómo podemos ahora utilizar los estilos recogidos en el objeto que hemos pasado como parámetro a `withStyles` dentro de nuestro componente? Pues como hemos visto anteriormente teniendo en cuenta que cada atributo de dicho objeto lo tendremos a nuestra disposición como un atributo de la prop `classes` que `withStyles` inyecta a nuestro componete. Así, en nuestro ejemplo, podríamos utilizar:

```javascript
function MyComponent(props) {
  const { classes } = props
  return <Button classNames={ classes.root }>Higher Order Component</Button>
}
```

----
**Tip:** Es bastante frecuente que en aquellos proyectos que utilizan `withStyles` a todos las propiedades CSS que queremos que se apliquen en todo el componente con el atributo `root` del objeto que define las clases CSS que queremos que se aplique.

---
**Nota:** para obtener más información de cómo funcionan los Higher Order Components se recomienda [consultar la documentación de React](https://en.reactjs.org/docs/higher-order-components.html) para obtener una explicación completa.

---

## Enlaces de interés

Para poder profundizar en todos los aspectos que han sido explicados a lo largo de este punto se recomienda consultar las siguientes direcciones:

* [Estilos en Material UI - Aspectos básicos](https://material-ui.com/es/styles/basics/).
* [Estilos en Material UI - Aspectos avanzados](https://material-ui.com/es/styles/advanced/).
* [Estilos en Material UI - API](https://material-ui.com/es/styles/api/)

