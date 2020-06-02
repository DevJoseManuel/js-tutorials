# Mejoras en la validación (UX)


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
            onBlur={ formik.handleBlur }
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
            onBlur={ formik.handleBlur }
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
            onBlur={ formik.handleBlur }
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
