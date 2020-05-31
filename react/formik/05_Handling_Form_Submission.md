# Envío del formulario (submit)

En el [punto anterior](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/formik/04_Managing_Form_State.md) hemos aprendido a gestionar el estado los formularios de nuestras aplicaciones gracias a **Formik**. El siguiente paso que tendremos que dar consistirá en aprender cómo **Formik** nos va a permitir gestionar el evento de envío de un formulario (submit) cuyo estado es gestionado gracias al uso de esta librería.

Como sucede con el estado del formulario la forma de conseguir gestionar el envío del formulario con **Formik** es relativamente sencilla ya que básicamente consiste en la realización de dos pasos. El primero de ellos consiste en indicar en la etiqueta `<form>` asociada al formulario que queremos que el gestor del evento `onSubmit` sobre el formulario sea uno de los métodos del objeto **Formik** que pone a nuestra disposición el objeto que es retornado tras la ejecución del hook `useFormik`. En concreto el método que deberemos utilizar será `handleSubmit`.

Siguiendo con el ejemplo del formulario que estamos siguiendo en este tutorial, realizamos la asignación del método `handleSubmit` como el gestor del evento `onSubmit` asociado a la etiqueta `<form>` que contiene todo el código de nuestro formulario teniendo en cuenta que el objeto que retorna la invocación del hook `useFormik` lo hemos asignado a la variabel `formik`:

```javascript
<form onSubmit={ formik.handleSubmit }>
```

---
**Nota:** el método `handleSubmit` (al igual que sucedía con el método `handleChange`) es considerado un método **helper** de entre todos los que pone a nuestra disposición **Formik** para trabajar con los datos de nuestros formularios.

---

Para aplicar el segundo paso que tenemos que realizar tenemos que centrar nuestra atención en el objeto que pasamos como parámetro a la hora de llamar al hook `useFormik`. Hasta ahora lo que hemos hecho ha sido utilizarlo para definir los valores que inicialmente tendrán asociados cada uno de los campos de nuestro formulario gracias al uso del atributo `initialValues`:

```javascript
const formik = useFormik({
  initialValues: {
    name: '',
    email: '',
    channel: ''
  }
})
```

En este objeto deberemos especificar el atributo `onSubmit` y asignarle una función (es decir, definiremos el método `onSubmit`) la cual recibirá de forma automática el valor del estado en el que se encuentra nuesto formulario (que si recordamos estaba recogido dentro del atributo `values` del objeto **Formik**) de tal manera que podamos aplicar la lógica que precisemos para determinar si el formulario será enviado o no. 

En nuestro ejemplo vamos a definir el método `onSubmit` de tal manera que lo único que haga sea escribir por la consola los valores del estado del formulario:

```javascript
const formik = useFormik({
  initialValues: {
    name: '',
    email: '',
    channel: ''
  },
  onSubmit: value => {
    console.log('Form data ', value)
  }
})
```

**Formik** nos garantizará que cada vez que un usuario pulse el botón **Submit** (es decir, que se produzca el evento `onSubmit`) se ejecutará siempre el método definido en el atributo `onSubmit` del objeto que hemos utilizado como parámetro del hook `useFormik`.

---
**Tip:** es posible que en la consola de JavaScript dentro de las herramientas para desarrolladores web que nos ofrece el navegador nos encontremos con un mensaje de warning donde se nos esté informando de que en el código JSX que hemos definido estemos definiendo un botón `<button>` sin indicar el tipo de botón del que se trata (es decir, sin definir el valor del atributo `type`) lo que hará que, por defecto, se considere un botón `submit` cuando realmente no es así.

```javascript
<button>Submit</button>
```

Para corregirlo lo que tenemos que hacer es definir el valor del atributo `type` para el elemento `<button>` de la siguiente manera:

```javascript
<button type='submit'>Submit</button>
```
---

En resumen ¿qué pasos tenemos que dar para hacer que **Formik** se encargue de gestionar el envío de los formularios:

1. En la etiqueta `<form>` que recoge los campos que forman nuestro formulario deberemos definir el atributo `onSubmit` y asignarle como valor el método `handleSubmit` del objeto **Formik**.
2. Tenemos que definir el atributo `onSubmit` en el objeto que se le pasa como parámetro al hook `useFormik` asignándole como valor la función que queremos que se ejecute cuando se produzca el evento **submit** en el formulario. Esta función recibirá como parámetro un objeto con los valores que tendrán asignados cada uno de los campos del formulario.

Hasta aquí puede parecernos que el uso de **Formik** ha añadido más complejidad a la gestión y el desarrollo de nuestros componentes de React pero es a partir del próximo punto donde las cosas comenzarán a ponerse realmente interesantes ya que poco a poco iremos conociendo aspectos que harán que nuestro código sea más limpio, fácil de mantener y menos propenso a errores.

## Componente final

El código completo de nuestro componente tras la finalización de este apartado es el que se muestra a continuación:

```javascript
import React from 'react'
import { useFormik } from 'formik'

const YoutubeForm = () => {
  const formik = useFormik({
    initialValues: {
      name: '',
      email: '',
      channel: ''
    },
    onSubmit: value => {
      console.log('Form data ', value)
    }
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
