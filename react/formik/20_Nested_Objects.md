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

Como hemos hecho en los puntos anteriores del tutorial vamos a ver cómo trabajar con los **nested objects** basándonos en un ejemplo. Para ello vamos a suponer que en nuestro formulario para recoger el voto de un usuario acerca de su canal favorito de Youtube queremos recolectar información acerca de cuáles son sus cuentas en redes sociales por lo que vamos a utilizar dos nuevos campos para que el usuario nos proporcione cuál es su cuenta de Facebook y su cuenta de Twitter, siempre que nos lo quiera facilitar.

Aunque en principio estos dos datos pueden ser considerados como separados nosotros vamos a tratarlos como un **nested object** para lo cual simplemente vamos a tener que seguir dos pasos muy sencillos. El primero de ellos, y como sucede con cualquier otro campo de nuestros formulario, tendremos que definir el atributo dentro del objeto `initialValues` que contendrá los valores de estos dos campos. En nuestro caso, como queremos crear un **nested object** lo que haremos será definir un único atributo el cual será igual a un objeto. Inicialmente llamamos a este atributo `social`:

```javascript
const initialValues = {
  name: '',
  email: '',
  channel: '',
  comments: '',
  address: '',
  social: {}
}
```

Pero ¿qué atributos tenemos que definir en este nuevo objeto? Pues sencillamente un atributo por cada uno de los campos que queremos queremos agrupar que en el caso de nuestro ejemplo será un campo (atributo) para recoger la información de la cuenta de Facebook del usuario y otro campo para recoger la información de la cuenta de Twitter (haciendo además que ambos campos incialmente estén inicializados a un string vacío):

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
  }
}
```

En segundo lugar tendremos que escribir el código JSX que se encargue de mostrar el marcado necesario para recoger la información de los dos nuevos campos de nuestro formulario. Como este paso es algo que ya hemos realizado varias veces a lo largo de este tutorial simplemente mostramos el código JSX del que partiremos para mostrar uno de los dos nuevos campos:

```javascript
<div className='form-control'>
  <label htmlFor='facebook'>Facebook profile</label>
  <Field type='text' id='facebook' name='social.facebook' />
</div>
```

Aquí en lo que tenemos que fijarnos es en el valor de la prop `name` al que le hemos establecido el valor `social.facebook` lo que nos viene a indicar (por el hecho de tener un punto `.` en el medio) que estamos haciendo referencia a un objeto (el que está asociado al atributo `social` dentro del objeto de **Formik** que recoge el estado del formulario) y dentro de ese objeto a un atributo concreto del mismo (en este caso al atributo `facebook` del objeto `social`).

Repitiendo este proceso para el campo que recoge la información de la cuenta de Twitter del usuarios tendríamos lo siguiente:

```javascript
<div className='form-control'>
  <label htmlFor='facebook'>Facebook profile</label>
  <Field type='text' id='facebook' name='social.facebook' />
</div>
<div className='form-control'>
  <label htmlFor='twitter'>Twitter profile</label>
  <Field type='text' id='twitter' name='social.twitter' />
</div>
```

Si ahora volvemos a recargar la página, rellenamos la información de todos los campos de nuestro formulario y pulsamos sobre el botón **Submit** el aspecto del objeto de **Formik** que contiene los campos del formulario junto con sus valores tendrá el siguiente aspecto:

```console
{
  name: 'user name',
  email: 'user@example.com`,
  channel: `youtube channel`,
  comments: `user comments`,
  address: `user address`
  social: {
    facebook: 'Facebook account',
    twitter: '@twitterAccount'
  }
}
```

Por lo tanto siempre que queramos agrupar campos dentro de nuestros formularios tendremos que hacer uso de los denominados **nested objects**. Para conseguirlo simplemente tendremosq ue seguir dos sencillos pasos:

1. Tendremos que definir el atributo dentro del objeto `initialVales` que recogerá la información de los campos de nuestro formulario que queremos agrupar como un objeto. Dentro de este objeto tendremos que definir un atributo por cada uno de los campos que queremos agrupar junto con el valor inicial para los mismos.
2. En segundo lugar tenemos que estar seguros de que cuando definimos el campo del formulario que contendrá el **nested object** en la prop `name` tendremos que asegurarnos de que estamos utilizando la notación con un punto `.` (**dot notation**) que nos permita navegar hasta el **nested object** y que dentro de del mismo naveguemos hasta el atributo.

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
  }
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

        <button type='submit'>Submit</button>
      </Form>
    </Formik>
  )
}

export default YoutubeForm
````
