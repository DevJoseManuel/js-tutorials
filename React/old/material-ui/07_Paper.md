# `Paper`

Es muy probable que el lector que haya estado mirando tutoriales o leyendo tutoriales de **Material UI** se haya encontrado varias veces con este componente sin llegar a entender muy bien para qué sirve. De hecho, en todos los ejemplos que hemos estado desarrollando hasta ahora, nosotros nunca lo hemos utilizado pero ¿por qué es tan popular?

La explicación no es otra que el componente `Paper` lo que nos va a permitir es mostrar un background dentro de los componentes de nuestras páginas (más concretamente alrededor del contenido del componente `Paper`). Veamos su aplicación con un ejemplo:

```javascript
import React from 'react'
import { Paper, Typography } from '@material-ui/core'

export default function App() {
  return (
    <Paper>
      <Typography variant='h6'>This is a typography</Typography>
    </Paper>
  )
}
```


En realidad lo que `Paper` estará haciendo es renderizar el contenido del componente `Typography` aplicándole un color de fondo (background) (que por defecto será el blanco y que, como cualquier otra propiedad CSS asociada al theme que nuestra aplicación está aplicando, vamos a poder cambiar a nuestro gusto) y un box-shadow. 

## Props

Vamos a estudiar las diferentes props que tenemos a nuestra disposición cuando estamos trabajando con el componente `Paper`.

### `children`

Como es evidente aquí tendremos la posibilidad de incluir cualquier marcado JSX que queremos que sea renderizado como contenido del `Paper` lo que indica que se mostrará con un color de fondo y rodeado de un border con una sombra alrededor (box-shadow).

### `classes`

Nuevamente nos aparece esta props teniendo la misma función que en resto de componentes que hemos estudiado hasta ahora [`Button`](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/material-ui/04_Button.md) y [`Typography`](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/material-ui/05_Typography.md) y que básicamente sirve para modificar las propiedades CSS del componente `Paper` en cuestión que queremos modificar de forma individual sin que perjudique a los diferentes `Paper` que podemos tener en nuestra aplicación.

### `component`

Esta prop también la hemos visto anteriormente tanto cuando hemos estudiado al componente [`Button`](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/material-ui/04_Button.md) y al componente [`Typography`](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/material-ui/05_Typography.md) y básicamente se utiliza para especificar con qué elemento html (en el caso de especificar un string) o componente de React (en el caso de especificar un código JSX) queremos que sea la raíz de la parte del DOM de la página que renderiza este componente.

Conviene saber que por defecto el valor de esta prop está establecido a `div` lo que nos viene a indicar que todo el contenido renderizado por el componente estará rodeado de una etiqueta `<div>`.

### `elevation`

Esta es la primera prop que es específica de `Paper` y admite como valor un número comprendido entre 0 y 24 (siendo el valor por defecto 1). Pero ¿qué es lo que está haciendo esta propiedad? Pues indicar cómo queremos que se renderice el box-shadow que rodeará al contenido del componente.

De esta manera si especificamos como valor 0 lo que sucederá es que el contenido no tendrá ningún box-shadow y por lo tanto no muestra un borde, mientras que si lo establecemos a 24 obtendremos el tamaño más grande para el box-shadow. La anología de este valor es considerarlo como la altura a la que está situado el componente con respecto al elementos que lo compone ya que, una valor más grande indicaría que el componente está más elevado y por lo tanto la sombra que proyectaría sobre el elemento que lo contiene será más grande que en el caso de una elevación menor.

### `square`

Se trata de una prop que acepta un valor booleano y que en el caso de que esté establecido a `true` (el valor por defecto es `false`) indicará que las esquinas que se muestran en el elemento que contendrá al marcado del `Paper` no muestren las esquinas redondeadas sino que se muestren como un rectángulo.

### `style`

Esta prop la tenemos a nuestra disposición en **todos los componentes de Material UI** ya que se trata de una prop que es propia de React y como tal la podemos utilizar cuando la necesitemos para definir aquellas propiedades CSS que necesitemos para nuestro `Paper` concreto. Por ejemplo:

```javascript
<Paper style={{ borderRadius: 10px }}>
  <Typography>This is my typography</Typography>
</Paper>
```

## Enlaces de interés.

* [Paper en Material UI](https://material-ui.com/components/paper/#paper).
* [API de `Paper`](https://material-ui.com/api/paper/).