# Temas (Theming)

En en este punto vamos a centrarnos en aprender qué son y cómo funcionan los temas (a partir de ahora *theme*) con los que trabaja **Material UI** además de ver qué pasos tenemos que seguir para poder crear nuestros propios temas. Este es uno de los temas más importantes que como desarrolladores que utilizan la librería deberíamos tener en cuenta ya que las opciones de personalización que están a nuestra disposición son muchas pero también es cierto que desconocidas.

## Default Theme

En la [documentación oficial](https://material-ui.com/customization/default-theme/) se habla del *default theme* que no es más que el conjunto de todas las opciones de presentación que se serán aplicadas a cada uno de los diferentes componentes de **Material UI** siempre que no se especifique lo contrario a la hora de declarar el componente. Pero ¿esto qué quiere decir? Pues que si utilizamos un componente como `Button` este tendrá unas opciones de visualización (propiedades CSS) que serán proporcionadas por el *default theme*.

Si ahora nos vamos a la documentación vemos que el *default theme* es un objeto de JavaScript el cual posee una serie de atributos cada uno de ellos representará un aspecto de la visualización sobre el que podemos aplicar nuestras propias opciones personalizadas. A lo largo de este tutorial iremos viendo todos ellos pero en este punto en concreto vamos a centrarnos en tres: `breakpoints`, `palette` y `typography`.



### breakpoints

El primero de los atributos que vamos a estudiar del objeto *default theme*. Si volvemos a la [documentación oficial](https://material-ui.com/customization/default-theme/) veremos que este a su vez tiene asignado un objeto que sirve para recoger las diferentes opciones relacionadas con los *breakpoints* a la hora de construir nuestra interfaz de usuario.

|atributo|descripción|
|---|---|
|keys|se trata de un array formado por cinco posiciones que recoge las distintos identificadores (en modo texto) que estarán asociados a cada *breakpoint*. Así los valores que tenemos son: `xs` (extra small), `sm` (small), `md` (medium), `lg` (large) y `xl` (extra large).
|values|en este caso tenemos asignado un objeto con un atributo por cada uno de los valores que están asignados al array `keys` y cuyos valor es un número que identifica el ancho mínimo de la pantalla (en píxeles) a partir del cual se considerá el breakpoint. Los valores serán `xs:0`, `sm:600`, `md:960`, `lg:1280` y `xl:1920`.<br/><br/>Pero ¿cómo se determina qué breakpoint se aplica? Pues simplemente teniendo en cuenta aquel cuya posición en el array `keys` es la más baja y que el valor que tiene asignado es menor o igual que el ancho que se está considerando? Un ejemplo, ¿qué breakpoint se aplicaría en el caso de un ancho de pantalla de 1000 píxeles? La respuesta es `md` porque el ancho mínimo que soporta es `960` (`lg` no puede ser porque su valor de `1280` es mayor que el ancho de la pantalla que se está considerando y `sm` tampoco puede ser porque aunque su valor sí que es menor que el ancho considerado existe un elemento posterior en el array `keys` que también tiene un valor inferior y por lo tanto será elegido).

El objeto que `breakpoints` también pone a nuestra dispoción los métodos `up`, `down`, `between`, `only` y `width` que podemos utilizar cuando lo consideremos oportuno durante la creación de nuestros propios temas. Ahora mismo no nos vamos a detener en su explicación porque más adelante podemos ver cómo utilizarlos.

Tenemos que pensar en los `breakpoints` como un mecanismo que tenemos a nuestra disposición para poder definir determinados estilos para los elementos de nuestra interfaz que se han de aplicar únicamente en determinados tamaños de la pántalla lo cual viene a ser muy muy útil como iremos viendo. Por otra parte, los valores que están asignados al objeto `values` no suelen ser cambiados a no ser que, por alguna razón en particular (como por ejemplo, que la compañía para la que estemos trabajando tenga su propio sistema de grid) no serán cambiados.

### palette

Este atributo contendrá un objeto cuyos atributos serán utilizados para definir todos y cada uno de los colores que tenemos a nuestra disposición dentro de la interfaz de usuario de nuestra aplicación. Esto quiere decir que cuando definimos un componente como el siguiente:

```javascript
<Button color='primary'>This is a button</Button>
```

lo que está sucediendo internamente es que **Material UI** sabe que en la prop `color` se ha de definir un string que se corresponderá con el nombre de uno de los atributos del objeto que está asignado al atributo `palette` del *theme* que se está aplicando. Y efectivamente así es porque si nuevamente vamos a la [documentación oficial](https://material-ui.com/customization/default-theme/) y desplegamos la información del objeto `palette` vemos que sí que tiene un atributo `primary` que a su vez es un objeto con los valores:

```javascript
primary: {
  light: #7986cb,
  main: #3f51b5,
  dark: #303f9f,
  contrastText: #fff
}
```

Pero ¿cuál de todos esos valores es el que estará aplicando cuando hemos definido la prop `color` como `primary`? La respuesta es que se aplicará el color que está asignado al atributo `main` lo que viene a indicar que en nuestro ejemplo se estará aplicando el color `#3f51b5`. Entonces ¿qué sentido tienen los atributo `light` y `dark`? La respuesta es que podemos estar creando una aplicación en la que definamos dos modos (*themes*) siguiendo las reglas de **Material UI**: el *light mode* y el *dark mode* en cuyo caso cuando uno de estos dos modos se esté aplicando el color que se mostrará a la que hora de renderizar el componente será el que está asociado al atributo `light` o al atributo `dark` en función del modo. Por último, el atributo `contrastText` sirve para definir en qué color se mostrará el texto contenido en el componente (en nuestro ejemplo, el texto del botón).

### typography

El atributo `typography` tiene asociado también un objeto con el objetivo de definir todos los aspectos relacionados con las fuentes de letras, tamaños, etc. que se han de aplicar en los componentes de la librería. Como en el caso de los dos atributos anteriores cada uno de los aspectos que podemos configurar se definirá como un atributo del objeto siendo, por ejemplo, el atributo `fontFamily` el que nos viene a indicar qué fuente de letra se estará utilizando en la aplicación (que por defecto será *Roboto*).

En este caso cabe destacar que tenemos una serie de atributo que están asociados a cada uno de los valores que podemos asignar a la prop `variant` de un componente `Typography` (más información de este componente [aquí](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/material-ui/04_Button.md)) por lo que si escribimos algo como lo siguiente:

```javascript
<Typography variant='h1'>This is a text</Typography>
```

Lo que se aplicarán serán los valores que están asociados al atributo `h1` y que sobreescriban los valores por defecto. Así, en este ejemplo, en los valores que se aplicarán serán:

```javascript
h1: {
  fontFamily: ""Roboto", "Helvetica", "Arial", sans-serif"
  fontWeight: 300
  fontSize: "6rem"
  lineHeight: 1.167
  letterSpacing: "-0.01562em"
}
```

en vez los valores por defecto que estuviesen definidos para todos los componentes `Typography` en sus atributos `fontFamily`, etc. Además, hay atributo como `lineHeight` que no están recogidos en los atributos por defecto pero que sí que se aplicarán cuando se esté aplicando un `variant` concreto.

¿Qué es lo importante que tenemos que saber de este objeto? Pues que si queremos aplicar nuestros propios estilos a los `variant` que nos proporciona **Material UI** simplemente tenemos que sobreescribir estas propiedades en nuestros propios *themes* dándonos una gran versatilidad.

## Creación un *theme*

Vamos ahora a ver cómo podemos crear nuestros propios *themes* en **Material UI** siendo este uno de los aspectos a los que la [documentación oficial](https://material-ui.com/customization/theming/) dedica toda una sección.

Lo primero que tenemos que entender es que siempre que queramos a aplicar un *theme* a uno o más componentes de nuestra aplicación tendremos que englobarlo con un componente especial de **Material UI** denominado `ThemeProvider` el cual aceptará una prop denominada `theme` que tendrá como valor el objeto que contiene la definición del *theme* que queremos que se aplique. Ahora bien, como el *theme* es algo que suele ser general a toda aplicación, lo más común es que nos encontremos con este objeto envolviendo al resto de los componentes que la forman. Así, en una típica aplicación de React nos encontraríamos con algo como lo siguiente:

```javascript
ReactDOM.render(
  <ThemeProvider theme={ theme }>
    <App />
  </ThemeProvider>
  , document.getElementById('root')
)
```

donde el componente `App` sería el componente raíz de la jerarquía de componentes en nuestra aplicación.

Pero ¿qué aspecto tiene ese objeto `theme` que estamos pasando en la prop? Y, lo que es más importante ¿para qué sirve? Lo primero que tenemos que entender es que es en este objeto donde sobreescribiremos todos aquellas propiedades del *default theme* que queremos personalizar en nuestra aplicación. Pero, ¿cómo podemos crearlo? Aquí es donde entra en juego la función `createMuiTheme` que nos proporciona **Material UI**. Vamos a verlo con un ejemplo:

```javascript
import { createMuiTheme } from '@material-ui/core/styles'
import purple from '@mateial-ui/core/colors/purple'
import green from '@mateial-ui/core/colors/green'

const theme = createMuiTheme({
  palette: {
    primary: purple,
    secondary: green
  }
})
```

La función `createMuiTheme` lo que espera recibir es un objeto que ha de contener tanto los atributos como los valores que queramos sobreescribir del *default theme*. En este caso, lo que estamos haciendo es sobreescribir los valores de los atributo `primary` y `secondary` del objeto `palette`. Además en el ejemplo hemos hecho la asignación de cada uno de estos atributos a uno de los objeto que representan a los colores que tenemos a nuestra disposición y que nos proporciona **Material UI** porque tenemos que recordar por lo visto anteriormente, que un atributo como `primary` espera recibir un objeto con cuatro atributos donde se recoge la información de los colores que se han de aplicar y estos objeto que representan a los colores de la librería contienen la definición de todos los atributos.

Ahora bien, en este ejemplo concreto, ¿y si no queremos utilizar uno de los colores que nos ofrece la librería? ¿qué tendremos que hacer? Pues simplemente expresar el único atributo en el define al color y que es obligatorio (los otros podremos proporcionarlos o no) que es el atributo `main` en formato hexadecimal. Un ejemplo sería:

```javascript
const theme = createMuiTheme({
  palette: {
    primary: {
      main: '#ccc';
    },
    secondary: green
  }
})
```
---
**Nota:** si estamos desarrollando una aplicación en la que puede haber en juego varios *themes* entre los que puede, por ejemplo, seleccionar un usuario y asociarlos a su cuenta, una buena práctica de programación es definir cada uno de esos *theme* en un objeto separado dentro de un fichero y en función de la opción de personalización que hay elegido el usuario, utilizar el adecuado a la hora de invocar a la función `createMuiTheme`.

---

## *theme* y la función `makeStyles`

Anteriormente [hemos visto](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/material-ui/03_makeStyles.md) cómo utilizar la función `makeStyles` para crear los diferentes estilos que queremos que se apliquen en los componentes de nuestra aplicación y qué posibilidades nos ofrece a la hora personalizarlos. 

Ahora bien, pensemos en un caso de uso algo más complejo a la hora de personalizar nuestros componentes. Vamos a suponer que tenemos un botón que queremos que se muestre con un determinado color cuando estamos en una resolución de pantalla de un determinado tamaño o superior y en otro distinto cuando estamos en una resolución de pantalla inferior. 


https://youtu.be/Q4o0GmfNpJc?t=744


