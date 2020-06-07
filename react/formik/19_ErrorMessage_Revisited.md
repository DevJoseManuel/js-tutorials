# Revisando el componente `ErrorMessage`




## Componente final

El código completo de nuestro componente tras la finalización de este punto es el que se muestra a continuación:

```javascript
import React from 'react'
import { Formik, Form, Field } from 'formik'
import * as Yup from 'yup'

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
          <ErrorMessage name='name' />
        </div>

        <div className='form-control'>
          <label htmlFor='email'>Email</label>
          <Field
            type='text'
            id='email'
            name='email'
          />
          <ErrorMessage name='email' />
        </div>

        <div className='form-control'>
          <label htmlFor='channel'>Channel</label>
          <Field
            type='text'
            id='channel'
            name='channel'
            placeholder='Youtube channel name'
          />
          <ErrorMessage name='channel' />
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
