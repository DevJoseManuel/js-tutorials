# Función de validación.

Para poder realizar las validaciones de los campos de nuestro formulario **Formik** nos permitirá definir lo que se conoce como una **validation function** (validation function) y posteriormente asignar dicha función al atributo `validate` del objeto que se pasa como parámetro al hook `useFormik`.

La **validation function** recibirá como parámetro el objeto que contiene el estado del formulario, es decir, el objeto que contiene la información de cada uno de los campos que forman el formulario así como los valores que tienen asignados.

Si retomamos el ejemplo con el que estamos trabajando partimos vamos a definir el atributo `validate` dentro de este objeto de la siguiente manera:

```javascript
const formik = useFormik({
  initialValues: {
    name: '',
    email: '',
    channel: ''
  },
  onSubmit: values => {
    console.log('Form data ', values)
  },
  validate: values => {
    // aquí irá el código de validación.
  }
})
```

Siguiendo con el ejemplo, el objeto que recibe como parámetro la función asignada al atributo `validate`, es decir, el objeto `values` contiene un total de tres atributos (uno por cada uno de los campos del formulario) que en este caso son `name`, `email` y `channel`. Es decir que el objeto `values` tendrá los siguientes atributos:

```javascript
values.name = // valor del campo name.
values.email = // valor del campo email.
values.channel = // valor del campo channel.
```

Pero ¿qué es lo que tendremos que hacer dentro de la función `validation` para que **Formik** sepa que los valores establecidos en los campos de nuestro formulario son los correctos? Lo primero que tenemos que saber es que **Formik** espera que esta función retorne un objeto que contendrá todos los errores que han sido detectados en los datos del formulario. Así pues, vamos a crear un nuevo objeto dentro de nuestra función `validate` (al que vamos a denominar `errors`) el cual estará inicialmente vacío y hacer este objeto sea el que será retornado como resultado de la invocación de la función:

```javascript
validate: values => {
  let errors = {}
  // código de valición de los campos del formulario.
  return errors
}
```

La segunda condición que nos impone **Formik** para la construcción del objeto retornado por la función `validate` es que la definición de los atributos que lo compondrá han de ser los mismos que los atributos que forman parte del objeto que contiene los datos del formulario. En nuestro ejemplo esto se traduce en que el objeto `errors` deberá tener los atributos `name`, `email` y `channel`.

¿Y qué valor han de tener cada uno de estos atributos? La respuesta es que deberemos asignarles un string que contendrá el mensaje de error que estará asociado al campo del formulario que representan siendo esta la tercera condición que **Formik** nos impone sobre el objeto `errors`. 




## Componente final

El código completo de nuestro componente tras la finalización de este punto es el que se muestra a continuación:

```javascript
import React from 'react'
import { useFormik } from 'formik'

const YoutubeForm = () => {
  const formik = useFormik({
    initialValues: {
      name: '',
      email: '',
      channel: ''
    },
    onSubmit: values => {
      console.log('Form data ', values)
    }
  })

  return (
    <div>
      <form onSubmit={ formik.handleSubmit }>
        <label htmlFor='name'>Name</label>
        <input
          type='text'
          id='name'
          name='name'
          onChange={ formik.handleChange }
          value={ formik.values.name }
        />

        <label htmlFor='email'>Email</label>
        <input
          type='text'
          id='email'
          name='email'
          onChange={ formik.handleChange }
          value={ formik.value.email }
        />

        <label htmlFor='channel'>Channel</label>
        <input
          type='text'
          id='channel'
          name='channel'
          onChange={ formik.handleChange }
          value={ formik.value.channel }
        />

        <button type='submit'>Submit</button>
      </form>
    </div>
  )
}

export default YoutubeForm
```
