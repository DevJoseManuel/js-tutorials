# Mejoras en la validación (UX)

En el punto anterior hemos estado viendo el atributo `touched` que el objeto **Formik** pone a nuestra disposición para saber qué campos de nuestros formularios han sido visitados y cuáles no. Ahora vamos a centrarnos en cómo podemos utilizar el objeto que está asociado a este atributo para mejorar la experiencia de usuario cuando esté trabajando con nuestros formularios.

Sabemos que el objeto que está asignado al atributo `touched` del objeto **Formik** presenta un atributo por cada campo de nuestro formulario que ha sido visitado. El nombre del atributo será el nombre del campo del formulario al que hace referencia y su valor será `true`. En el ejemplo que hemos estado desarrollando hasta ahora el criterio que seguíamos para mostrar un mensaje de error era realizando la siguiente comprobación:

```javascript
{ formik.errors.name && <div className='error'>{ formik.errors.name }</div> }
```

Esto lo que provocaba es que, pese a que el usuario no estuviese interactuando con uno de los campos del formulario (en el caso del código anterior, en el caso de que el usuario no estuviese trabajando con el campo `name`) se mostrase el mensaje de error siempre que este campo estuviese vacío pese a que el usuario esté introduciendo información en otro de los campos del formulario lo que se traduce en una mala experiencia de usuario.

---
**Nota:** es importante recordar que cada vez que el usuario escribe un nuevo caracter en uno de los campos de texto de nuestro formulario se producirá un nuevo renderizado del mismo lo que implicará que se ejecute la **validation function** haciendo que todos los campos que están vacíos muestren el mensaje de error pese a no estar interactuando con ellos.

---

Para lograr una mejor experiencia de usuario únicamente deberíamos mostrar el mensaje de error no solamente cuando no se pasa la validación sobre el campo que se esté realizando dentro de la **validation function** sino que además se deberá cumplir que el campo haya sido visitado previamente.

La forma de conseguir nuestro objetivo para decidir si se ha de mostrar un mensaje de error o no además de comprobar que el atributo dentro del campo `errors` del objeto **Formik** no está vacío añadiremos la condición de que el existe el mismo campo como un atributo del atributo `touched` y además que este atributo tiene el valor `true`. Esto que resulta muy engorroso de explicar con palabras es muy sencillo cuando tratamos de expresarlo en código:

```javascript
{
  formik.errors.name &&
  formik.touched.name &&
  <div className='error'>{ formik.errors.name }</div>
}
```
Lo interesante ahora es que cuando un usuario se sitúa en uno de los campos de nuestro formulario, mientras esté interactuando con el mismo no se mostrará ningún mensaje de error y es únicamnete cuando se abandona dicho campo (en otras, palabras cuando se produce el evento `onBlur` en dicho campo del formulario) cuando se establecerá el valor de atributo que tiene asignado dentro del objeto `touched` de **Formik** a `true` indicando de esta manera que el campo ha sido visitado.

Al final esto se traducirá en que tendremos un mejor control en la experiencia de usuario **UX** a la hora de trabajar con nuestro formularios lo que mejorará no solamente la calidad de nuestra código sino la valoración que nuestros usuarios percibirán de nuestro trabajo.

Con este punto damos por finalizado el estudio de los aspectos básicos de la validación de los campos de nuestros formularios en **Formik**. Ahora bien, es necesario conocer que **Formik** nos proporcionará una forma alternativa de poder realizar las validaciones sobre los campos de nuestros formularios aspecto del que nos vamos a ocupar en el siguiente punto de nuestro tutorial.

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
