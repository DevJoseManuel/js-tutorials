# Función de validación.

Para poder realizar las validaciones de los campos de nuestro formulario **Formik** nos permitirá definir lo que se conoce como una **validation function** (validation function) y posteriormente asignar dicha función al atributo `validate` del objeto que se pasa como parámetro al hook `useFormik`.

La **validation function** recibirá como parámetro el objeto que contiene el estado del formulario, es decir, el objeto que contiene la información de cada uno de los campos que forman el formulario así como los valores que tienen asignados.

Si retomamos el ejemplo con el que estamos trabajando partimos vamos a definir el atributo `validate` dentro de este objeto de la siguiente manera:

```javascript
const formik = useFormik({
  initialValues: {
    name: '',
    email: '',
    channel: ''
  },
  onSubmit: values => {
    console.log('Form data ', values)
  },
  validate: values => {
    // aquí irá el código de validación.
  }
})
```

Siguiendo con el ejemplo, el objeto que recibe como parámetro la función asignada al atributo `validate`, es decir, el objeto `values` contiene un total de tres atributos (uno por cada uno de los campos del formulario) que en este caso son `name`, `email` y `channel`. Es decir que el objeto `values` tendrá los siguientes atributos:

```javascript
values.name = // valor del campo name.
values.email = // valor del campo email.
values.channel = // valor del campo channel.
```

Pero ¿qué es lo que tendremos que hacer dentro de la función `validation` para que **Formik** sepa que los valores establecidos en los campos de nuestro formulario son los correctos? Lo primero que tenemos que saber es que **Formik** espera que esta función retorne un objeto que contendrá todos los errores que han sido detectados en los datos del formulario. Así pues, vamos a crear un nuevo objeto dentro de nuestra función `validate` (al que vamos a denominar `errors`) el cual estará inicialmente vacío y hacer este objeto sea el que será retornado como resultado de la invocación de la función:

```javascript
validate: values => {
  let errors = {}
  // código de valición de los campos del formulario.
  return errors
}
```

La segunda condición que nos impone **Formik** para la construcción del objeto retornado por la función `validate` es que la definición de los atributos que lo compondrá han de ser los mismos que los atributos que forman parte del objeto que contiene los datos del formulario. En nuestro ejemplo esto se traduce en que el objeto `errors` podría tener únicamente los atributos `name`, `email` y `channel`.

---
**Nota:** es importante entender que el objeto `errors` no tiene por qué definir un atributo por cada uno de los campos que forman nuestro formulario. Lo que sí que nos tiene que quedar claro es que ha de tener un atributo por cada uno de los campos del formulario que sí que tienen un error asociado.

---

¿Y qué valor han de tener cada uno de estos atributos? La respuesta es que deberemos asignarles un string que contendrá el mensaje de error que estará asociado al campo del formulario que representan siendo esta la tercera condición que **Formik** nos impone sobre el objeto `errors`.

El objetivo que tenemos que perseguir ahora es escribir el código que nos permitirá realizar todas las validaciones que hemos descrito en el [punto anterior](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/formik/06_Form_Validation.md).

Comenzando con el campo `name` vemos que la única validación que hay que llevar a cabo sobre el mismo es que es obligatorio por lo que realizaremos la siguiente comprobación en el código:

```javascript
validate: values => {
  let errors = {}

  if (!values.name) {
    errors.name = 'Required'
  }

  return errors
}
```
El código anterior es realmente simple: con la sentencia `ìf` lo que estamos haciendo es comprobar si dentro del objeto `values` que se recibe como parámetro de la función está definido el atributo `name` (que sabemos que así es, porque tenemos que recordar que en el objeto `initialValues` que hemos definido a la hora de ejecutar el hook `useFormik` hemos establecido este atributo con un valor inicial igual a la cadena vacía) pero al hacer la comprobación `!values.name` en el caso de tratarse de una cadena vacía retornará el valor `true` lo hará que dentro del objeto `errors` creemos el atributo `name` asignándole como valor un texto que indicará que el campo es requeido (obligatorio).

De forman análoga tendremos que añadir la misma lógica pero en este caso asociada al los atributos `email` y `channel` ya que ambos campos son también obligatorios:

```javascript
validate: values => {
  let errors = {}

  if (!values.name) {
    errors.name = 'Required'
  }

  if (!values.email) {
    errors.email = 'Required'
  }

  if (!values.channel) {
    errors.channel = 'Required'
  }

  return errors
}
```

En el caso del campo `email` nos queda una validación más que realizar y es que se ha de comprobar que el valor que se ha introducido sigue el formato de un string que sirve para recoger una dirección de correo electrónico (en otras palabras, que el valor que se ha proporcionado puede ser verificado gracias a la aplicación de una expresión regular). Así pues añadimos una segunda validación sobre el campo email de la siguiente manera:

```javascript
if (!values.email) {
  errors.email = 'Required'
} else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email))
  errors.email = 'Invalid email format'
}
```

---
**Nota:** la expresión regular que hemos utilizado para validar el campo `email` no cubrirá al totalidad de casos que se nos puedan producir pero sirve para darnos una idea de cómo podemos realizar una validación de formato sobre los campos de nuestros formularios.

---

El código de nuestra función `validate` estaría completado. Ahora bien, si nos fijamos un poco en el código que hemos desarrollado hasta ahora la invocación del hook `useFormik` se hace a través de un código que no es nada limpio por lo que parece lógico que realicemos algún tipo de refactorización del mismo. En general lo que haremos será sacar la definición de cada uno de los objetos que asignaremos a al objeto que se pasa como parámetro al hook `useFormik` fuera de la inicialización del mismo para posteriormente utilizarlo en dicha invocación.

Esto que parece muy complicado de explicar es muy sencillo de entender cuando nos centramos en nuestro código de ejemplo. Así comenzaremos definiendo un nuevo objeto al que le asignaremos los atributos y valores de los mismos que tiene el objeto que hemos pasado en el atributo `initialValues` a la hora de invocar al hook `useFormik`. Así, el aspecto de este objeto sería el siguiente:

```javascript
// Objeto que define los atributos y valores iniciales para cada uno de los 
// campos de nuestro formulario.
const initialValues = {
  name: '',
  email: '',
  channel: ''
}
```
 
 Esto mismo lo hacemos con la definición de la función que queremos que sea ejecutada cuando se produzca el evento `submit` en nuestro formulario:

 ```javascript
 const onSubmit = values => {
   console.log('Form data ', values)
 }
```

Y por último definimos sacamos la definición de la función `validate` fuenra de la llamada al hook `useFormik` de la siguiente forma:

```javascript
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
```

Tras esto la definición de la invocación al hook `useHook` dentro del código de nuestro componente quedaría de la siguiente manera:

```javascript
const YoutubeForm = () => {
  const formik = useFormik({ 
    initialValues,
    onSubmit,
    validate
  })
  // Resto del código del componente.
```

---
**Tip:** para la realización de los atributos y los valores asociados a los mismos estamos utilizando **shorthand syntax** que nos ofrece ES6. Si no se está familiarizado con su utilización se recomienda visitar [Object Initializer](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Operators/Object_initializer).

---

A modo de resumen ¿cuáles son los pasos que tenemos que llevar a cabo cuando queremos definir las validaciones que queremos que se lleven a cabo sobre los campos de los formularios que gestionaremos mediante **Formik**?

1. Lo primero que tenemos que hacer es definir la que se conoce como **validation function** la cual recibirá como parámetro el objeto que contiene los campos y los valores asociados a los campos del formulario. Esta función será la que posteriormente asignaremos al atributo `validate` del objeto que pasaremos como parámetro al hook `useFormik`.
2. Para construir el cuerpo de la **validation function** tenemos que tener siempre presente tres condiciones que se tienen que cumplir:
    * la primera de ellas es que la función deberá retornar un objeto, el cual normalmente se deonima `errors`.
    * el nombre de los atributos de este objeto `errors` han de ser los mismos que los de los valores de los campos del formulario sobre el que se están realizando las validaciones. En este sentido los atributos de este objeto serán los mismos que los que definimos para el objeto `initialValues` cuando estamos definiendo el formulario. 
    * la tercera condición que se ha de cumplir es que a cualquiera de los atributos de este objeto `error` será un string con el mensaje de error que estará asignado al campo del formulario al que representa.

Ahora que sabemos cómo realizar las validaciones sobre los campos de nuestro formulario en el siguiente punto veremos qué es lo que tenemos que hacer para poder mostrar el valor de los errores que han sido asignados a los campos del formulario en la **validation function**.

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
        <label htmlFor='name'>Name</label>
        <input
          type='text'
          id='name'
          name='name'
          onChange={ formik.handleChange }
          value={ formik.values.name }
        />

        <label htmlFor='email'>Email</label>
        <input
          type='text'
          id='email'
          name='email'
          onChange={ formik.handleChange }
          value={ formik.value.email }
        />

        <label htmlFor='channel'>Channel</label>
        <input
          type='text'
          id='channel'
          name='channel'
          onChange={ formik.handleChange }
          value={ formik.value.channel }
        />

        <button type='submit'>Submit</button>
      </form>
    </div>
  )
}

export default YoutubeForm
```
