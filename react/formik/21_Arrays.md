# Arrays

En este punto del tutorial vamos a ver cómo podemos gestionar el estado de alguno de los campos de nuestros formularios como un array. Como hemos hecho anteriormente para lograrlo vamos a continuar con el ejemplo que estamos siguiendo a lo largo del tutorial suponiendo esta vez que vamos a crear un nuevo campo dentro de nuestro formulario para recoger el número de teléfono de los usuarios, es más, vamos a suponer que queremos permitir que el usuario nos indique cuál es su número de teléfono principal y un número de teléfono adicional (secundario) en el que podríamos localizarlo.

Para que esta modificación que vamos a realizar tenga sentido guardarla en un array vamos a suponer que a nivel de nuestra aplicación nos dará igual si el número de teléfono que se nos proporciona es el principal, el secundario o ambos (en otras palabras, para nosotros tienen la misma importancia cualquier número de teléfono). 

Como sucede con cualquier otro campo que queramos añadir a nuestro formulario el primer paso que tendremos que dar consistirá en añadir un nuevo atributo al objeto `initialValues` que utilizamos para construir el objeto **Formik**. Vamos a denominar a este atributo como `phoneNumbers` (para indicar que se encargará de recoger los valores de los números de teléfono) y como son dos los elementos que va a poder contener (uno por cada número de teléfono) lo que vamos a hacer es inicializarlo como un array con dos posiciones siendo cada una de ellas una cadena vacía.

```javascript
const initialValues = {
  name: '',
  email: '',
  channel: '',
  comments: '',
  address: '',
  social: {
    facebook: '',
    twitter: ''
  },
  phoneNumbers: ['', '']
}
```

En segundo lugar lo que tenemos que hacer es crear el código JSX que nos va a permitir mostrar los campos de nuestro formulario. En este caso el código que escribiremos será el siguiente:

```javascript
<div className='form-control'>
  <label htmlFor='primaryPh'>Primary phone number</label>
  <Field type='text' id='primaryPh' name='phoneNumbers[0]' />
</div>
<div className='form-control'>
  <label htmlFor='secondaryPh'>Secondary phone number</label>
  <Field type='text' id='secondaryPh' name='phoneNumbers[1]' />
</div>
```

En principio el código JSX no es muy diferente del que hemos escrito para mostrar cualquiera de lost otros campos de nuestro formulario excepto por el valor que le asignamos a la prop `name` dentro del componente `Field` de **Formik** ya que ahora estaremos indicando que el valor del campo cuyo `id` es `primaryPh` será guardado en la posición 0 del array asignado al atributo `phoneNumbers` dentro del objeto **Formik** que gestiona el estado del formulario (el cual representará al número de teléfono principal) y el valor del campo cuyo `ìd` es `secondaryPh` será guardado en la posición 1 del array asignado al atributo `phoneNumbers`.



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
  address: '',
  social: {
    facebook: '',
    twitter: ''
  },
  phoneNumbers: ['', '']
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

        <div className='form-control'>
          <label htmlFor='facebook'>Facebook profile</label>
          <Field
            type='text'
            id='facebook'
            name='social.facebook'
          />
        </div>
        <div className='form-control'>
          <label htmlFor='twitter'>Twitter profile</label>
          <Field
            type='text'
            id='twitter'
            name='social.twitter'
          />
        </div>

        <div className='form-control'>
          <label htmlFor='primaryPh'>Primary phone number</label>
          <Field 
            type='text' 
            id='primaryPh' 
            name='phoneNumbers[0]' 
          />
        </div>
        <div className='form-control'>
          <label htmlFor='secondaryPh'>Secondary phone number</label>
          <Field 
            type='text' 
            id='secondaryPh' 
            name='phoneNumbers[1]' 
          />
        </div>

        <button type='submit'>Submit</button>
      </Form>
    </Formik>
  )
}

export default YoutubeForm
````



