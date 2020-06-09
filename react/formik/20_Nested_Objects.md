# Objetos anidados (Nested Objects)

En el formulario de ejemplo que estamos utilizando hasta ahora estamos definiendo un formulario que utiliza un total de cinco campos para recoger un voto de un usuario de nuestra aplicación para un canal de Youtube siendo una particularidad del mismo que cada uno de los campos tendrá un valor asociado y que todos los campos están recogidos al mismo nivel dentro de la jerarquía de objetos que se recogen en el estado de formulario.

Esto que puede sonar algo confuso se ve bien si cargamos el formulario dentro de un navegador web, rellenamos los campos del formulario y pulsamos sobre el botón **Submit** para que en al consola de JavaScript dentro de las herramientas para desarroladores veamos los pares campo-valor que se habrán enviado:

```console
{
  name: 'user name',
  email: 'user@example.com`,
  channel: `youtube channel`,
  comments: `user comments`,
  address: `user address`
}
```

Este tipo de estructura de datos nos puede valer en la mayoría de las ocasiones con las que estemos trabajando pero también es posible que en alguna ocasión necesitemos agrupar varios campos en una estructura de objetos anidada.

---
**Nota:** las razones para ello pueden ser varias pero pueden ir desde que la API que se va a acceder con los datos del formulario precise de una estructura de datos determinada o bien que la información que se vaya a guardar en la base de datos que utiliza la aplicación precise que los datos estén definidos de una determinada manera.

---

Al final lo que queremos hacer es poder agrupar varios de los campos de nuestros formularios de una manera determinada para poderlos utilizar como precisemos. Sea cual sea la razón vamos a tener que utilizar lo que se conoce como los objetos anidados o **nested objects**.



## Componente final

El código completo de nuestro componente tras la finalización de este punto es el que se muestra a continuación:

```javascript
import React from 'react'
import { Formik, Form, Field } from 'formik'
import * as Yup from 'yup'
import TextError from './TextError'

const initialValues = {
  name: '',
  email: '',
  channel: '',
  comments: '',
  address: ''
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
          <ErrorMessage
            name='name'
            component={ TextError }
          />
        </div>

        <div className='form-control'>
          <label htmlFor='email'>Email</label>
          <Field
            type='text'
            id='email'
            name='email'
          />
          <ErrorMessage name='email'>
          { errorMsg => <div className='error'>{ errorMsg }</div> }
        </ErrorMessage>
        </div>

        <div className='form-control'>
          <label htmlFor='channel'>Channel</label>
          <Field
            type='text'
            id='channel'
            name='channel'
            placeholder='Youtube channel name'
          />
          <ErrorMessage
            name='channel'
            component={ TextError }
          />
        </div>

        <div class='form-control'>
          <label htmlFor='address'>Address</label>
          <Field name='address'>
            props => {
              const { field, meta } = props
              return (
                <div>
                  <input type='text' id='address' { ...field }/>
                  {
                    meta.touched && meta.error
                        ? <div>{ meta.error }</div>
                        : null
                  }
                </div>
              )
            }
          </Field>
        </div>

        <button type='submit'>Submit</button>
      </Form>
    </Formik>
  )
}

export default YoutubeForm
````
