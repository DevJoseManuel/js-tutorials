# El componente `Form`

En este punto vamos a explicar cómo utilizar el componente `Form` de **Formik** dentro de nuestros formularios pese a que este componente no va simplificar el código de nuestro componente pero lo que sí sucede es que será necesario para el correcto funcionamiento del resto de componentes que explicaremos en los siguientes puntos.

Lo primero que haremos, como en otras ocasiones, será importar el componente `Form` para poderlo utilizar. Así escribimos lo siguiente:

```javascript
import React from 'react'
import { Formik, Form } from 'formik'
import * as Yup from 'yup'
```

El siguiente paso consistirá en reemplazar el marcado JSX que estábamos utilizando para renderizar el elemento `<form>` dentro de nuestro componente:

```javascript
<Formik
  initialValues={ initialValues }
  validationSchema={ validationSchema }
  onSubmit={ onSubmit }
>
  <form onSubmit={ formik.handleSubmit }>
  { /* ... resto del código ... */ }
  </form>
</Formik>
```

por el componente `Formik`:

```javascript
<Formik
  initialValues={ initialValues }
  validationSchema={ validationSchema }
  onSubmit={ onSubmit }
>
  <Form>
  { /* ... resto del código ... */ }
  </Form>
</Formik>
```

Lo que tenemos que tener presente es que el componente `Form` no define el atributo `onSubmit` que estaba recogido y definido en el elemento `<form>`. Esto es así porque el componente `Form` es un envoltorio que se encargará de escribir el elemento `<form>` y asignará el método `handleSubmit` del objeto **Formik** al atirbuto `onSubmit` del elemento html que recogerá el formulario.

En otras palabras, el component e`Form` de **Formik** nos ayudarán a vincular directamente por nosotros el método `handleSubmit` del objeto **Formik** al atributo `onSubmit` al elemento html `<form>` que será renderizado.

En siguiente punto vamos a seguir con el estudio de los componetes que tenemos a nuestra disposición estudiando el componte `Field`.

## Componente final

El código completo de nuestro componente tras la finalización de este punto es el que se muestra a continuación:

```javascript
import React from 'react'
import { Formik, Form } from 'formik'
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
      </Form>
    </Formik>
  )
}

export default YoutubeForm
````
