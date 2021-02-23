# El componente `ErrorMessage`

En este punto vamos a estudiar el cuarto de los componetes que **Formik** nos proporciona para simplificar el código nuestros componentes: `ErrorMessage`. 

Lo primero que tenemos que hacer es recordar cómo estamos haciendo hasta ahora para determinar si tenemos que mostrar un mensaje de error o no. Cogiendo como ejemplo el campo `name` dentro de nuestro formulario de ejemplo vemos que tenemos lo siguiente:

```javacript
{
  formik.errors.name &&
  formik.touched.name &&
  <div className='error'>{ formik.errors.name }</div>
}
````

Es decir, que lo que estamos haciendo es comprobar que en el objeto que está asociado al atributo `errors` del objeto **Formik** existe el atributo `name` y además que no está vacío (recordemos que el campo `name` es obligatorio) y, a continuación comprobamos que dicho campo ha sido visitado gracias a comprobar que el objeto asociado al atributo `touched` del objeto **Formik** posee el atributo `name` y además que ese valor es `true`. Solamente en el caso de que se cumplan las condiciones anteriores será cuando se mostrará el `<div>` en el que se rederiza el mensaje de error.

Este proceso de verificación que acabamos de describir se repite para los tres campos de nuestro formulario y, en general, de muchos de los formularios que podamos construir en nuestras aplicaciones. Así pues, los desarrolladores de **Formik** han decidido facilitarnos las cosas proporcionándonos el componente `ErrorMessage`.

Como en otras ocasiones vamos a ver cómo lo podemos incoporar en nuestro ejemplo paso a paso. En primer lugar lo que tenemos que hacer es importar el componente para poderlo utilizar:

```javascript
import React from 'react'
import { Formik, Form, Field, ErrorMessage } from 'formik'
import * as Yup from 'yup'
```

En segundo lugar lo que tenemos que hacer es reemplazar el código que hasta ahora teníamos para renderizar cada uno de los errores de validación asociados a los campos de nuestro formulario por el componente `ErrorMessage`. En caso del campo `name` partimos del siguiente código:

```javacript
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
````

el cual deberemos sustituir simplemente por el componente `ErrorMessage`:

```javascript
<Field
  type='text'
  id='name'
  name='name'
/>
<ErrorMessage />
````

En tercer lugar deberemos establecer la prop `name` de este componente la cual ha de tener el mismo valor que la prop `name` del componente `Field` que renderiza el campo de al que está asociado. Esto se traduce en nuestro ejemplo en el siguiente código:

```javascript
<Field
  type='text'
  id='name'
  name='name'
/>
<ErrorMessage name='name' />
````

Como es evidente estos mismos pasos los tenemos que seguir con el resto de los campos de nuestro formulario: `email` y `channel`.

---
**Nota:** esta sustitución que hemos realizado es posible porque internamente el componente `ErrorMessage` comprueba si existe un error para un determinado campo del formulario que haya sido visitado (en concreto el campo cuyo nombre especifiquemso en la prop `name`)

---
**Nota:** hay un pequeño detalle que ha cambiado tras realizar el refactor de la forma en la que se mostrarán los mensajes de error y es que anteriormente el mensaje aparecía en rojo (gracias a estilo CSS que aplicábamos al elemento `<div>` que utilizábamos para mostrar el mensaje) y ahora el mensaje no aparecerá en ningún color diferente al resto del texto de la página lo que puede llevar a confusión. De cualquier manera esto es algo que veremos en próxima punto de este tutorial cuando estudiemos con más detalle el componente `ErrorMessage`.

---

Con esto damos por concluido el refactor sobre el código de nuestro componente de ejemplo `YoutubeForm` logrando obtener la misma funcionalidad además de reducir considerablemente el número de líneas de código que son necesarias para definirlo.

Pero no solamente hemos logrado reducir el número de líneas sino que podemos asegurar que ahora nuestro código es mucho más sencillo de leer, más limpio y al final esto se traduce en que será mucho más fácil de mantenter.

Antes de pasar al siguiente punto del tutorial vamos a hacer un repaso rápido a todos los pasos que hemos dado para realizar el refactor que nos ha llevado a este punto:

1. En primero lugar hemos importado el componente `Formik` el cual hemos utilizado para envolver todo el marcado JSX que definirá nuestro formulario. Este componente acepta como props los valores iniciales para los campos del formulario (en la prop `initialValues`), el esquema de validación de **yup** que queremos utilizar (en la prop `validationSchema`) y la función que queremos que se ejecute en cuando se envíe el formulario (en la prop `onSubmit`).
2. Hemos reemplazado la etiqueta html `<form>` por el componente `Form` de **Formik**. Con esto se vinculará de forma automática el evento `onSubmit` de html con el método `handleSubmit` del objeto **Formik** sin necesidad de que nosotros lo tengamos que especificar de ninguna manera.
3. Hemos reemplazado cada uno de los campos de texto que forman nuestro formulario por componente `Field` de **Formik**. Cada uno de estos componentes quedará vinculado con **Formik** a través del valor que le asignaremos a su prop `name`. Además con el uso del componente `Field` tendremos la garantía de que quedará vinculado el valor del campo del formulario y el valor que dicho campo tiene en el estado del formulaio además de vincularse los eventos `onChange` y `onBlur` de html con los métodos `handleChange` y `handleBur` del objeto **Formik** respectivamente.
4. Por último para mostrar los posibles errores de validación en nuestro formulario hemos utilizado el componente `ErrorMessage` que nos permitirá mostrar de forma condicional (es decir, en el caso de que exista un error en un determinado campo de nuestro formulario y además que dicho campo haya sido visitado) el mensaje de error asociado al campo.

En este punto estamos en condiciones de asegurar que ya conocemos todos los aspectos básicos en los que se apoya **Formik** para funcionar.

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
          />
          <ErrorMessage name='channel' />
        </div>

        <button type='submit'>Submit</button>
      </Form>
    </Formik>
  )
}

export default YoutubeForm
````
