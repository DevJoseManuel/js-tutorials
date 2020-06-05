# Los componentes de **Formik**

En el punto anterior hemos visto cómo podemos utilizar el método `getFieldProps` para reducir la cantidad de código que tenemos que escribir para construir nuestros nuestros formularios. Sin embargo el uso de este método no nos impide saber que tengamos que escribirlo en todos y cada uno de los campos que formarán la interfaz de nuestro formulario.

Para ahorrarnos este trabajo **Formik** nos ofrece una serie de componentes que podemos utilizar para definir nuestros formularios y que hacen uso del contexto de React para trabajar internamente con la información que les hace falta para ofrecer su funcionalidad haciendo que que nuestro código sea mucho más sencillo, limpio y fácil de mantener.

---
**Nota:** en este tutorial no vamos a profundizar en entender qué es el contexto de React y cómo se utiliza por lo que aquellos lectores que estén interesados en entender mejor cómo funciona se recomienda [leer la documentación oficina](https://reactjs.org/docs/context.html).

---

De entre los mucho componentes que nos ofrece **Formik** vamos a hacer uso de cuatro de ellos para realizar una refactorización del código del componente con nuestro formulario de ejemplo. Estos componentes, que estudiaremos en los siguientes puntos:

* `Formik`
* `Form`
* `Field`
* `ErrorMessage`

En primer lugar a ver cómo podemos utilizar el componente `Formik`. Lo primero que tenemos que entender es que el componente `Formik` viene a reemplazar al hook `useFormik` que hemos estado utilizando hasta ahora:

```javascript
const formik = useFormik({
  initialValues,
  onSubmit,
  validationSchema
})
````

Lo que tenemos que saber es que cada uno de los atributos que posee el objeto que pasamos como parámetro al hook `useFormik` se deberán definir ahora como props del componente `Formik`.

Vamos a ejecutar paso a paso la secuencia de operaciones que se han de seguir para poder utilizar el componente `Formik` en nuestro formulario de ejemplo. Como sucede con cualquier otro componente lo primero que tenemos que hacer es importarlo gracias a una sentencia `import`. Así pues reemplazamos:

```javascript
import React from 'react'
import { useFormik } from 'formik'
import * as Yup from 'yup'
```

donde ya no vamos a utilizar el hook `useFormik` por el componente `Formik`:

```javascript
import React from 'react'
import { Formik } from 'formik'
import * as Yup from 'yup'
````

El siguiente paso será eliminar la invocación al hook `useFormik` dentro del código del componente. Esto se traduce en que el siguiente código desaparecerá del componente:

```javascript
// Eliminamos este código del componente.
/*
const formik = useFormik({
  initialValues,
  onSubmit,
  validationSchema
})
*/
````

A continuación lo que tendremos que hacer es envolver (rodear) el marcado JSX que define nuestro formulario con el componente `Formik`. Por lo tanto se cambiará:

```javascript
const YoutubeForm = () => {
  return (
    <div>
    // ...resto de código...
    </div>
  )
}
```

por el siguiente marcado JSX:

```javascript
const YoutubeForm = () => {
  return (
    <Formik>
    // ...resto de código...
    </Formik>
  )
}
```

Para terminar nos quedará por establecer las props de este componente `Formik` que como ya hemos dicho tendrán el mismo nombre que los atributos del objeto que se le pasa como parámetro al hook `useFormik` y su valor será el mismo que el tienen asignado en dicho objeto. Así por lo tanto definimos:

```javascript
const YoutubeForm = () => {
  return (
    <Formik
      initialValues={ initialValues }
      validationSchema={ validationSchema }
      onSubmit={ onSubmit }
    >
    // ...resto del código...
    </Formik>
  )
}
```

---
**Tip:** internamente el componente `Formik` funciona como un `Provider` del contexto de React para trabajar con **Formik** y que nos va a permitir trabajar con los métodos y atributos del objeto **Formik** en el código de nuestros componentes. 

En concreto este `Provider` lo que va a hacer es proporcionar al resto de componentes de **Formik** el acceso a los métodos y valores del objeto **Formik** que hemos estado utilizando en nuestros ejemplos anteriores.

---

A modo de resumen ¿qué es lo que tenemos que hacer para poder utilizar el componente `Formik` en nuestros formularios?

1. En primer lugar se ha de importar el componente en nuestro código.
2. Envolver el formulario de nuestra aplicación con el componente `Formik`.
3. Establecer las props necesarias de `Formik` que en el caso de nuestro ejemplo serán los atributos `initialValues`, `validationSchema` y `validate`.

En el siguiente punto vamos a ver cómo podemos utilizar el componete `Form` a la hora de definir los formularios de nuestras aplicaciones.

## Componente final

El código completo de nuestro componente tras la finalización de este punto es el que se muestra a continuación:

```javascript
import React from 'react'
import { Formik } from 'formik'
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
    </Formik>
  )
}

export default YoutubeForm
```
