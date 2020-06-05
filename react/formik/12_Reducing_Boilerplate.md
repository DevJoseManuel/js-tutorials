# Refactorización

Una vez hemso adquirido los conocimientos básicos de **Formik** vamos a centrarnos ahora en ver qué es lo que podemos hacer para reducir el código repetido dentro de nuestros componentes dejándolo más limpio. Comencemos viendo el aspecto del código del componente con el que hemos estado trabajando hasta ahora:

```javascript
import React from 'react'
import { useFormik } from 'formik'
import * as Yup from 'yup'

const initialValues = {
  name: '',
  email: '',
  channel: ''
}

const validationSchema = Yup.object({
  name: Yup.string().required('Required'),
  email: Yup.string().required('Required').email('Invalid email format'),
  channel: Yup.string().required('Required')
})

const onSubmit = values => {
   console.log('Form data ', values)
}

const YoutubeForm = () => {
  const formik = useFormik({
    initialValues,
    onSubmit,
    validationSchema
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

Para realizar el primero de los refactor de nuestro componente vamos a fijarnos en el código que se encargar de renderizar los tres campos de nuestros formulario es prácticamente el mismo. De hecho si nos fijamos un poquito más en cada uno de los campos del formulario tenemos que definir los atributo `onChange`, `onBlur` y `value` siendo el valor que tiene asignado el mismo en el caso de los primeros (los método `handleChange` y `handleBlur` del objeto **Formik** respectivamente) y en el caso del tercero accediendo siempre al objeto asignado al atributo `value` del objeto **Formik** que contiene las información del campo del formulario (`name` para el primer campo de texto, `email` para el segundo y `channel` para el tercero). Todo esto nos hace ver que el código es bastante repetetivo.

Por lo tanto por cada uno de los campos del formulario se han de definir estos tres atributos por lo que sería muy conveniente tener a nuestra disposición una función que nos ayude a establecer el valor de estos tres campos. Tanto es así, que los desarrolladores de **Formik** nos proporcionan, dentro del objeto **Formik** el método `getFieldProps` con este propósito.

El método `getFieldProps` espera recibir un parámetro que será el nombre del campop para el que queremos establecer los tres atributos vistos anteriormente por lo que podremos sustituir el códígo:

```javascript
<input
  type='text'
  id='email'
  name='email'
  onBlur={ formik.handleBlur }
  onChange={ formik.handleChange }
  value={ formik.value.email }
/>
```

Por este código:

```javascript
<input
  type='text'
  id='email'
  name='email'
  { ...formik.getFieldProps('name') }
/>
```
Como podemos deducir por el uso del **spread operator** de ES6 lo que el método `getFieldProps` retorna es un objeto que contiene los atributo `onChange`, `onBlur` y `value` asignados a los métodos y valores que esperábamos.

Con este primer refactor lo que hemos conseguido es recudir tres líneas de código de cada uno de los campos de nuestro formulario en una única línea lo que en formularios con un mayor número de campos supondrá un ahorro de líneas de código considerable.

Pero este no es el único refactor que podremos llevar a cabo en nuestro código y que **Formik** nos va a ofrecer más posibilidades para que nuestro código sea mucho más límpio como veremos en los siguientes punto del tutorial.

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

const validationSchema = Yup.object({
  name: Yup.string().required('Required'),
  email: Yup.string().required('Required').email('Invalid email format'),
  channel: Yup.string().required('Required')
})

const onSubmit = values => {
   console.log('Form data ', values)
}

const YoutubeForm = () => {
  const formik = useFormik({
    initialValues,
    onSubmit,
    validationSchema
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
            { ...formik.getFieldProps('name') }
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
            { ...formik.getFieldProps('email') }
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
            { ...formik.getFieldProps('channel') }
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
