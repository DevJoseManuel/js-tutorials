# Formulario de ejemplo

Durante el desarrollo de este tutorial vamos a trabajar con un formulario de ejemplo que vamos a suponer que es necesario para recoger información de una votación para acerca de cuál es el mejor canal de Youtube. Para modelalo vamos a crear un nuevo componente de React que denominaremos `YoutubeForm`.

Nuestro funcionario será muy simple ya que estará formado únicamente por tres campos de texto: un campo que servirá para recoger el nombre de la persona que realiza la votación (campo `name`), un campo que servirá para recoger la dirección de correo del usuario que vota (compo `email`) y por último un tercer campo donde se ha de indicar el nombre del canal por el que se realizará la votación (campo `channel`).

Adicionalmente el último elemento que formará parte de la interfaz de usuario será un botón **Submit** que va a permitir que el usuario pueda enviar su voto para que sea recogido por la aplicación.

¿De qué aspectos nos tenemos que preocupar cuando estemos trabajando con los formularios dentro de nuestras aplicaciones? O dicho de otra manera ¿qué es lo que nos debería proporcionar una librería para simplificar nuestro trabajo cuando estemos trabajando con formularios en React? La respuesta son los tres aspectos siguientes:

1. Gestionar el estado del formulario entendiendo como tal conocer cuáles son los campso que lo forman y los valores que tienen asignados.
2. Permitirnos realizar de forma simple el envío del formulario.
3. Realizar las validaciones sobre los datos que se recogen en cada uno e los campos del formulario y permitirnos mostrar los mensajes de error asociados a los posibles errores de validación.

A medida que vayamos avanzando en el tutorial veremos como **Formik** implementa la solución para cada uno de estos puntos y cómo podemos llevarlo a cabo en nuestro formulario de ejemplo.

Con esto en mente vamos a mostrar el componente de React del que partiremos para seguir avanzando en la explicación. Lo primero que tenemos que entender es que hemos creado una nueva aplicación de React gracias al uso `create-react-app`. Si queremos denominar a la aplicación como `react-formik-demo` deberemos escribir en la terminal de nuestro sistema lo siguiente:

```console
$ create-react-app react-formik-demo
```

Una vez finaliza la instalación nos dirigiremos al archivo `src/App.js` donde modificaremos su código para que quede de la siguiente manera:

```javascript
import React from 'react'
import './App.css'

function App() {
  return (
    <div className='App'>
    </div>
  )
}

export default App
```

Hecho esto nos dirigemos al direction `src` y dentro del mismo creamos el directorio `components`. Nuesto objetivo será que dentro de la aplicación el código de todos los componentes de React que vayamos desarrollando queden contenidos dentro de este directorio. En este momento el único componente que vamos a necesitar es el que va a permitir mostrarle al usuario el formulario para recoger su votación por lo que dentro de este directorio creamos el fichero `YoutubeForm.js` donde comenzaremos definiendo el esqueleto para un **functional component** de React de la siguiente manera:

```javascript
import React from 'react'

const YoutubeForm = () => (
  <div>
  </div>
)

export default YoutubeForm
```

El siguiente paso que daremso será definir el código JSX que servirá para mostrar en la pantalla del usuario el formulario que va a permitir recoger la información de su votación:

```javascript
import React from 'react'

const YoutubeForm = () => (
  <div>
    <form>
      <label htmlFor='name'>Name</label>
      <input type='text' id='name' name='name' />

      <label htmlFor='email'>Email</label>
      <input type='text' id='email' name='email' />

      <label htmlFor='channel'>Channel</label>
      <input type='text' id='channel' name='channel' />

      <button>Submit</button>
    </form>
  </div>
)

export default YoutubeForm
```

Como podemos observar el código JSX que define nuestro formulario es muy sencillo ya que por cada uno de los campos del mismo se define la etiqueta `<label>` que va a permitir situarse en el campo de texto (etiqueta `<input>`) que tienen asignado como valor de su atributo `id` el mismo valor que tiene asignado la etiqueta `<label>` en su atributo `htmlFor`.

Para terminar con la definición de nuestro componente de React vamos a centrarnos en el código CSS que ayudará a mejorar la apariencia de los elementos cuando son mostrados al usuario. Teniendo en cuenta que estos estilos son importados del archivo `App.css` (esto lo sabemos por las importaciones que se están haciendo al principio del código del fichero `App.js` que sirve de punto de entrada a la aplicación). El código CSS que vamos a utilizar es el siguiente:

```css
/* Centramos el contenido de la página */
.App {
  display: flex;
  justify-content: center;
}

/* Estilos para las etiquetas <label> */
label {
  font-weight: bold;
  display: flex;
  margin-bottom: 5px;
}

/** Estilos para las etiquetas <input> */
input[type='text'] {
  display: 'block';
  width: 400px;
  padding: 6px 12px;
  font-size: 14px;
  line-height: 1.5;
  color: #555;
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-bottom: 20px;
}
```

Por último lo que tenemos que hacer es incluir nuestro nuevo componente `YoutubeForm` dento del componente `App` para que sea renderizado cuando se accede a la aplicación. Así pues modificamos el código del archivo `App.js` para que quede de la siguiente manera:

```javascript
import React from 'react'
import './App.css'
import YoutubeForm from './components/YoutubeForm'

function App() {
  return (
    <div className='App'>
      <YoutubeForm />
    </div>
  )
}

export default App
```

En este momento si ahora cargamos la aplicación en un navegador web podremos ver que se puede escribir información en cada uno de los campos del formulario e incluso pulsar sobre el botón **Submit** sin que nada suceda. Esto es debido a que todavía no hemos introducido en el código ninguna interacción con React ni con **Formik** cosa que vamos a comenzar a realizar a partir del siguiente punto.

## Componente final

El código completo de nuestro componente tras la finalización de este punto es el que se muestra a continuación:

```javascript
import React from 'react'


const YoutubeForm = () => (
  <div>
    <form>
      <label htmlFor='name'>Name</label>
      <input type='text' id='name' name='name' />

      <label htmlFor='email'>Email</label>
      <input type='text' id='email' name='email' />

      <label htmlFor='channel'>Channel</label>
      <input type='text' id='channel' name='channel' />

      <button>Submit</button>
    </form>
  </div>
)

export default YoutubeForm
```