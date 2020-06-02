# Campos visitados

En el punto anterior hemos visto que la **validation functions** que está asociada a un formulario se ejecutará siempre que se produce un cambio en uno de los campos del formulario lo que se puede traducir en una mala experiencia del usuario ya que puede darse la situación en la que estemos mostrando mensajes de error de validación asociados a campos del formulario con los que el usuario todavía no ha interactuado.

En el ejemplo que estamos siguiendo la técnica que hemos seguido para determinar si un mensaje de error ha de ser mostrado o no es simplemente comprobar si dentro del objeto `errors` que está asociado al objeto **Formik** que nos retorna la invocación del hook `useFormik` contiene un antributo cuyo nombre es el mismo que el nombre del campo del formulario que estamos chequeando y en el caso de que así sea mostramos directamente el mensaje de error. Así, en el caso del campo `name` la comprobación que estamos haciendo es la siguiente:

```javascript
{ formik.errors.name && <div className='error'>{ formik.errors.name }</div> }
```

La idea que estamos persiguiendo es la de no mostrar los mensajes de error asociados a todos aquellos campos del formulario que el usuario todavía no haya visitado. Pero ¿cómo podemos saber qué campos han sido visitados y cuáles no? O, dicho de otra manera ¿de qué manera podemos llevar el registro de los campos del formulario que han sido visitados por un usuario? Para lograrlo nuevamente volvermos a apoyarnos en **Formik**.

Lo primero que tenemos que saber es que en **Formik** para poder saber qué campos han sido visitados es preciso que para cada uno de los elementos `<input>` que forman nuestro formulario tengamos que definir la propiedad `onBlur` asignándole como valor el método `handleBlur` que está asociado al objeto **Formik** que nos ha retornado la invocación del hook `useFormik`:

```javascript
<input
  type='text'
  id='name'
  name='name'
  onBlur={ formik.handleBlur }
  onChange={ formik.handleChange }
  value={ formik.values.name }
/>
```

---
**Nota:** `handleBlur` es otro de los **helper methods** que nos ofrece el objeto **Formik** (al igual que, por ejemplo, `handleChange`).

---

Gracias al método `handleBlur` **Formik** puede controlar qué campos de nuestro formulario han sido visitados pero ¿dónde guarda **Formik** esta información? La respuesta es en el objeto que estaá asignado al atributo `touched` del objeto **Formik**. Este objeto tiene el mismo aspecto que el objeto que está asignado al atributo `values` del objeto **Formik** en el sentido de que contendrá un atributo por cada uno de los campos que forman el formulario.

¿Y cómo se irá poblando este objeto? La forma de hacerlo es la siguiente cada vez que un usuario sitúa el cursor en uno de los campos del formulario no se producirá ningún tipo de cambio en el objeto pero el foco del control estará en dicho campo. Es en el momento en el que el campo pierde el foco (por ejemplo, haciendo click en otro de los campos del formulario) que **Formik** poblará el objeto `touched` con un nuevo atributo cuyo nombre será el mismo que el nombre del campo del formulario y su valor pasará a ser `true`. 

---
**Nota:** si previamente ya se había visitado el campo del formulario **Formik** no hará nada ya que el objeto `tocuhed` ya contendrá el valor del atributo con el mismo nombre del campo del formulario con el valor `true`.

---

Así pues el objeto que está asociado al atributo `touched` del objeto **Formik** nos va a dar información acerca de los campos de nuestros formularios que han sido visitados o no de tal manera que si un campo ha sido visitado el nombre de dicho campo aparecerá como un atributo del objeto `touched` y el valor que estará asociado a dicho atributo será `true`.

---
**Nota:** al igual que sucede con el objeto `errors` de **Formik** la primera vez que se carga el formulario en la página el objeto que está asignado al atributo `touched` estará vacío no poblándose hasta que se produce la primera interacción con uno (cualquiera) de los campos de nuestro formulario.

----

Con todo esto que acabamos de explicar en el siguiente punto del tutorial vamos a ver qué es lo que tenemos que hacer para mejorar la lógica que se encargará de mostrar los errores a los usuarios.

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
