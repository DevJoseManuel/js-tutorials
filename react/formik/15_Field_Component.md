# El componente `Field`

En este punto vamos a ver cómo podemos utilizar el componente `Field` que nos proporciona **Formik** cuyo objetivo no es otro que simplificar el código que tenemos que escribir cuando estamos escribiendo el marcado para mostrar un campo de texto.

En el ejemplo que estamos desarrollando hasta ahora hemos visto que gracias al método `getFieldProps` que nos proporciona el objeto **Formik** pasándole como parámetro el nombre del campo que vamos a renderizar hemos conseguido simplificar el código de nuestro componente. A modo de ejemplo recordemos el marcado para el campo `name` de nuestro formulario:

```javascript
<input
  type='text'
  id='name'
  name='name'
  { ...formik.getFieldProps('name') }
/>
```

Los desarrolladores de **Formik** descubrieron que este patrón es muy común a la hora de crear nuestros formularios por lo que implementaron el componente `Formik` con el fin de simplificar nuestro trabajo. Como hemos hecho hasta ahora, vamos a ver paso a paso la secuencia de acciones que tenemos que llevar a cabo para poderlo utilizar en nuestro formulario.

Como en otras ocasiones el primero paso consistirá en importar el componente para que esté disponible en nuestro código. Así pues escribiremos:

```javascript
import React from 'react'
import { Formik, Form, Field } from 'formik'
import * as Yup from 'yup
```

El segundo lugar lo que tenemos que hacer es sustituir todos los elementos `<input>` de nuestro formulario por el componente `Field` que acabamos de importar. Con esto tendremos:

```javascript
<Field
  type='text'
  id='name'
  name='name'
  { ...formik.getFieldProps('name') }
/>
```

En tercer lugar simplemente quitaremos la invocación al método `getFieldProps` del objeto **Formik** porque ya no es necesario dejándonos cada uno de los campos de texto con un aspecto como el siguiente:

```javascript
<Field
  type='text'
  id='name'
  name='name'
/>
```

Con esto conseguimos tener la misma funcionalidad que anteriormente ya que internamente el componente `Field` de **Formik** realiza tres cosas por nosotros:

1. Realiza la vinculación interna entre cada uno de los componentes `Field` que vayamos escribiendo y el componente `Formik` que [hemos definido anteriormente](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/formik/13_Formik_Components.md).
2. Utiliza la prop `name` que asignemos para realizar la vinculación entre el campo que estamos representando y el estado del formulario.
3. Por defecto el componente `Field` renderizará un campo `<input>` que es lo que necesitamso en el formulario de ejemplo aunque, como veremos más adelante, podremos establecer que se renderice cualquier otro tipo de campo de un formulario (un checkbox, textarea, etc.).

Ya para finalizar con el refactor de nuestro componente en el siguiente punto vamos a ver cómo podemos utilizar el componente `ErrorMessage` que también nos proporciona **Formik**.

## Componente final

El código completo de nuestro componente tras la finalización de este punto es el que se muestra a continuación:

```javascript
import React from 'react'
import { Formik, Form, Field } from 'formik'
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
  return (
    <Formik
      initialValues={ initialValues }
      validationSchema={ validationSchema }
      onSubmit={ onSubmit }
    >
      <Form>
        <div className='form-control'>
          <label htmlFor='name'>Name</label>
          <Field
            type='text'
            id='name'
            name='name'
          />
          {
            formik.errors.name &&
            formik.touched.name &&
            <div className='error'>{ formik.errors.name }</div>
          }
        </div>

        <div className='form-control'>
          <label htmlFor='email'>Email</label>
          <Field
            type='text'
            id='email'
            name='email'
          />
          {
            formik.errors.email &&
            formil.touched.email &&
            <div className='error'>{ formik.errors.email }</div>
          }
        </div>

        <div className='form-control'>
          <label htmlFor='channel'>Channel</label>
          <Field
            type='text'
            id='channel'
            name='channel'
          />
          {
            formik.errors.channel &&
            formik.touched.channel &&
            <div className='error'>{ formik.errors.channel }</div>
          }
        </div>

        <button type='submit'>Submit</button>
      </Form>
    </Formik>
  )
}

export default YoutubeForm
````
