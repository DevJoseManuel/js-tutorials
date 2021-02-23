# Mensajes de error

En el punto anterior hemos visto cómo crear la **validate function** la cual se encargará de realizar las validaciones sobre los campos del formulario asignando un atributo en al objeto `errors` que retorna donde el nombre de cualquiera de los atributos que lo forman se corresponderá con el nombre del campo del formulario que tiene un error y su valor será el mensaje de error que ha de ser mostrado como error de validación. En este punto veremos qué pasos tenemos que seguir para poder mostrar estos mensajes de error.

Hasta ahora hemos visto que el objeto **Formik** que nos retorna la invocación del hook `useFormik` nos proporcionará una serie de atributos y métodos que nos van a ser de utilidad a la hora de trabajar con nuestros formularios.

```javascript
const formik = useFormik({
  initialValues, 
  onSubmit,
  validate
})
```

Como podríamos esperar es gracias a este objeto que **Formik** va a poder interactuar con los mensajes de error que se han producido derivados de la validación del formulario. En este momento tenemos que recordar que el objeto **Formik** posee un atributo `values` el cual es a su vez un objeto que tendrá como atributos los nombres de cada uno de los campos del formulario los cuáles tendrán a su vez asignados como valores los valores de dichos campos.

De forma análoga a cómo está construido el atributo `values` el objeto **Formik** posee otro atributo más denominado `errors` el cual es su vez un objeto donde cada uno de los atributos será cada uno de los campos del formulario y el valor que tendrán asociado será el mensaje de error que deberá ser mostrado como consecuencia de no haber pasado alguna de las validaciones llevadas a cabo en la **validation function**.

Lo que tenemos que entender es que inicialmente, cuando se muestra por primera vez el formulario al usuario, este objeto `errors` estará vacío pero es en el instante en el que se introduce el primero de los caracteres en cualquiera de los campos del formulario que invocará a la función `validate` y por lo tanto el objeto `errors` se irá poblando con los diferentes mensajes de error que se puedan producir.

¿Qué es lo que tendríamos que hacer ahora para poder mostrar estos mensajes de error al usuario? Pues simplemente acceder al objeto `errors` como si se tratase de cualquier otro objeto JavaScript con el que pudiésemos estar trabajando. Así, en nuestro ejemplo, lo que vamos a hacer es crear un nuevo elemento `<div>` a continuación de cada uno de los campos de texto dentro del cual mostraremos el mensaje de error:

```javascript
<label htmlFor='name'>Name</label>
<input
  type='text'
  id='name'
  name='name'
  onChange={ formik.handleChange }
  value={ formik.values.name }
/>
```
Vamos a utilizar lo que se conoce como **conditional rendering** ya que únicamente se mostrará el mensaje de error en el caso de que el campo asociado al error que queremos mostrar esté presente en el objeto `errors` que contiene la descripción de todos los errores de validación que se han ido produciendo.

```javascript
<label htmlFor='name'>Name</label>
<input
  type='text'
  id='name'
  name='name'
  onChange={ formik.handleChange }
  value={ formik.values.name }
/>
{ formik.errors.name ? <div>{ formik.errors.name }</div> : null }
```

De esta manera lo que estamos diciendo es que si existe (y no está vacío, es decir, que no tiene asignada la cadena vacía) el atributo `name` dentro del objeto `errors` lo que haremos será mostrar el `<div>` con el mensaje de error mienstras que si no existe (o está vacío) no mostraremos nada (`null`).

---
**Tip:** una forma de acotar el operador ternario que hemos utilizado anteriormente es utilizando directamente el operador `&&` de JavaScript ya que podemos hacer que este evalúa el primero de los operando y únicamente en el caso de que sea `true` pasará a evaluar el segundo de ellos. Si hacemos que el segundo de ellos retorne el código JSX que queremos mostrar lograríamos el mismo objetivo que cuando estábamos utilizando el operador ternario.

Esto quiere decir que convertiremos la instrucción:

```javascript
{ formik.errors.name ? <div>{ formik.errors.name }</div> : null }
```

en lo siguiente:

```javascript
{ formik.errors.name && <div>{ formik.errors.name }</div> }
```

---
**Nota:** antes de seguir con la explicación vamos a tener que llevar a cabo un pequeño refactor en el código JSX de nuestro formulario ya que, [si recordamos](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/formik/02_Simple_Form.md) cuando hemos definido el código CSS para mostrar nuestro formulario hemos especificado que cada uno de los elementos `<input>` que podamos tener en nuestro formulario dejen un margen de 20 píxeles en su parte inferior lo que haría que, en el caso de que se produjese un error, el `<div>` que contendría el mensaje de error se muestre separado del `<input>` al que hace referencia.

Así pues lo que vamos a hacer es englobar cada uno de los elemento `<label>`, `<input>` y el `<div>` que puede contener el mensaje de error asociado al campo del formulario, dentro de un nuevo `<div>` al que vamos a asignarle la clase CSS `form-control`:

```javascript
<div className='form-control'>
  <label htmlFor='name'>Name</label>
  <input
    type='text'
    id='name'
    name='name'
    onChange={ formik.handleChange }
    value={ formik.values.name }
  />
  { formik.errors.name && <div>{ formik.errors.name }</div> }
</div>
```

Adicionalmente a cada uno de los `<div>` que se encargarán de mostrar los mensajes de error vamos a asignarles una clase CSS que denominaremos `error`:

```javascript
  { formik.errors.name && <div className='error'>{ formik.errors.name }</div> }
```

Hecho esto tenemos que abrir el fichero donde se están definiendo las reglas CSS para nuestra aplicación (el fichero `App.css` dentro del directorio `src`) donde lo primero que vamos a hacer es quitar el margen inferior para los elementos `<input>` de nuestra página dejándolo de la siguiente manera:

```css
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
}
```

Lo siguiente que haremos será añadir el margen que hemos eliminado de los elemento `<input>` como el margen inferior que se ha de aplicar al nuevo `<div>` que engloba tanto al `<label>`, como al `<input>` como al mensaje de error asociado a cada uno de los elementos del formulario, es decir, que asignamos la propiedad `margin-bottom` a la nueva clase CSS `form-control`:

```css
.form-control: {
  margin-bottom: 10px;
}
```

Nos quedará por especificar los estilos CSS que queremos que se apliquen al `<div>` que contiene los mensajes de error donde simplemente lo que vamos a hacer es que el texto que se muestre lo haga en color rojo:

```css
.error {
  color: red;
}
```

---

Repasemos ahora cuáles son los pasos que hemos seguido para realizar la validación de los campos del formulario:

1. En primero lugar hemos creado la **validation function** (que en nuestro ejemplo se llama `validate`) y esta función la hemos pasado como el valor del atributo `validate` del objeto que se pasa como parámetro en la invocación del hook `useFormik`.
2. La **validaton function** recibe como parámetro el objeto que contiene la relación de los campos que formnan el formulario junto con el valor que tienen asociado cada uno de ellos en el momento en el que se invoca siendo su cometido realizar las validaciones pertienentes sobre los datos y retornar un objeto que ha de tener un atributo por cada uno de los campos que tienen al menos un error (además de que el atributo ha de llamarse igual que el nombre del campo) siendo su valor un string con el mensaje de error que deberá ser mostrado.
3. Este objeto con los errores de validación estará disponible en el atributo `errors` que pone a nuestra disposición el objeto **Formik** que obtenemos cuando llamamos al hook `useFormik`.
4. Gracias al objeto `formik.errors` y al uso de **conditional rendering** que podremos mostrar cada uno de los posibles mensaje de error asociados a los campos de nuestros formularios.

Vemos, por lo tanto, que **Formik** realiza una abstracción más natural de la forma en la que se han de realizar las validaciones sobre los campos de nuestros formularios haciendo además que el código que tengamos sea mucho más limpio.

El problema con la aproximación que hemos realizado hasta ahora es que, como ya hemos dicho, la **validation function** se ejecutará siempre que se produzca un cambio en cualquiera de los campos que forman parte de nuestro formulario. Ahora bien, si por ejemplo estamos interactuando con el campo `name` de nuestro formulario nada más que escribamos el primero de los caracteres se producirá un nuevo renderizado del formulario y como los campos `email` y `channel` no tienen ningún valor asociado, no pasarán la función de validación lo que provocará que se muestre un mensaje de error asociado a los mismos cuando ni siquiera el usuario ha interactuado con dichos campos. Todo esto se traduce en una mala experiencia de uso para el usuario.

Afortunadamente **Formik** nos ofrece mecanismos para evitar este tipo de situaciones y eso es precisamente lo que vamos a explicar en el siguiente punto del tutorial.

## Componente final

El código completo de nuestro componente tras la finalización de este punto es el que se muestra a continuación:

```javascript
import React from 'react'
import { useFormik } from 'formik'

const initialValues = {
  name: '',
  email: '',
  channel: ''
}

const onSubmit = values => {
   console.log('Form data ', values)
}

const validate = values => {
  let err = {}

  if (!values.name) {
    err.name = 'Required'
  }

  if (!values.email) {
    errors.email = 'Required'
  } else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email))
    errors.email = 'Invalid email format'
  }

  if (!values.channel) {
    errors.channel = 'Required'
  }

  return err
}

const YoutubeForm = () => {
  const formik = useFormik({
    initialValues, 
    onSubmit,
    validate
  })

  return (
    <div>
      <form onSubmit={ formik.handleSubmit }>
        <div className='form-control'>
          <label htmlFor='name'>Name</label>
          <input
            type='text'
            id='name'
            name='name'
            onChange={ formik.handleChange }
            value={ formik.values.name }
          />
          { formik.errors.name && <div className='error'>{ formik.errors.name }</div> }
        </div>

        <div className='form-control'>
          <label htmlFor='email'>Email</label>
          <input
            type='text'
            id='email'
            name='email'
            onChange={ formik.handleChange }
            value={ formik.value.email }
          />
          { formik.errors.email && <div className='error'>{ formik.errors.email }</div> }
        </div>

        <div className='form-control'>
          <label htmlFor='channel'>Channel</label>
          <input
            type='text'
            id='channel'
            name='channel'
            onChange={ formik.handleChange }
            value={ formik.value.channel }
          />
          { formik.errors.channel && <div className='error'>{ formik.errors.channel }</div> }
        </div>

        <button type='submit'>Submit</button>
      </form>
    </div>
  )
}

export default YoutubeForm
```
