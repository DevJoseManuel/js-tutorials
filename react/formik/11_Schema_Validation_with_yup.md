# Validación utilizando `yup`

En este punto vamos a ver la alternativa que tenemos para poder realizar las validaciones de los campos de nuestros formularios utilizando **Formik** y es utiliar la librería **yup**. 

Como siempre que trabajamos con una nueva librería el primer paso que tendremos que hacer será instalarla en nuestro proyecto por lo que desde la terminal de comandos de nuestro equipo escribiremos:

```console
$ npm install yup
```

Una vez finaliza la instalación para poderla utilizar dentro del componente que estamos desarrollando a lo largo del manual, lo primero que tendremos que hacer será importarla dentro del mismo:

```javascript
import React from 'react'
import { useFormik } from 'formik'
import * as Yup from 'yup'
```

---
**Nota:** con la instrucción de importación que hemos realizado lo que estamos consiguiendo es importar todas las funciones y constantes que son exportadas dentro de la librería **yup** para que estén disponibles a través de un objeto al que hemos denominado `Yup`. Así si, por ejemplo, existe una función que es exportada con el nombre del `foo` podremos acceder a la misma como `Yup.foo()`.

---

En este tutorial no vamos a explicar cómo internamente funciona **yup** si no que lo que perseguiremos será ver cómo podemos utilizar esta librería para realizar las validaciones de los formularios de **Formik** dejando al lector que esté interesado en ello que visite la [página del proyecto en GitHub](https://github.com/jquense/yup).


[INCOMPLETO]


## Componente final

El código completo de nuestro componente tras la finalización de este punto es el que se muestra a continuación:

```javascript
import React from 'react'
import { useFormik } from 'formik'
import * as Yup from 'yup'

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
            onBlur={ formik.handleBlur }
            onChange={ formik.handleChange }
            value={ formik.values.name }
          />
          {
            formik.errors.name &&
            formik.touched.name &&
            <div className='error'>{ formik.errors.name }</div>
          }
        </div>

        <div className='form-control'>
          <label htmlFor='email'>Email</label>
          <input
            type='text'
            id='email'
            name='email'
            onBlur={ formik.handleBlur }
            onChange={ formik.handleChange }
            value={ formik.value.email }
          />
          {
            formik.errors.email &&
            formil.touched.email &&
            <div className='error'>{ formik.errors.email }</div>
          }
        </div>

        <div className='form-control'>
          <label htmlFor='channel'>Channel</label>
          <input
            type='text'
            id='channel'
            name='channel'
            onBlur={ formik.handleBlur }
            onChange={ formik.handleChange }
            value={ formik.value.channel }
          />
          {
            formik.errors.channel &&
            formik.touched.channel &&
            <div className='error'>{ formik.errors.channel }</div>
          }
        </div>

        <button type='submit'>Submit</button>
      </form>
    </div>
  )
}

export default YoutubeForm
```

