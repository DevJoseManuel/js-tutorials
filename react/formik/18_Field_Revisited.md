# Revisando el componente `Field`

En este punto vamos a volver a revisar el componente `Field` que nos proporciona **Formik** viendo todos aquellos aspectos que nos permitirán utilizarlo para gestionar formularios mucho más complejos. Cuando hemos [estudiado por primera vez](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/formik/15_Field_Component.md) el componente `Field` vimos dos aspectos básicos del mismo:

* Por defecto cuando se renderiza el componente `Field` se mostrará un elemento html `<input>`.
* Se estará realizando la vinculación del campo de texto del formulario que se renderiza con el objeto **Formik** que se utiliza para gestionar el estado de nuestro formulario. Esto quiere decir que se produce la vinculación del atributo `name` del elemento html y un atributo homónimo dentro del atributo `values` del objeto **Formik** además de vincular el atributo (evento) `onChange` con el método `handleChange` del objeto **Formik** y el atributo (evento) `onBlur` con el método `handleBlur` del objeto **Formik**.

En este punto vamos a seguir profundizando en el componente `Field` para ver qué posibilidades nos ofrece. En primer lugar tenemos que saber que a la hora de definir el componente `Field` vamos a poderle pasar más props que nos ayuden a especificar cómo queremos que se comporte y se muestre. La primera de estas props es `id` que nos va a servir para esficar un valor para el atributo `id` de la etiqueta html que se utilizará para renderizar nuestro campo del formulario. Así si definimos lo siguiente:

```javascript
<Field
  type='text'
  id='name'
  name='name'
/>
```

será renderizado de la siguiente manera:

```html
<input type='text' name='name' id='name' />
```

Otra de las props que podemos especificar es aquella que nos va a permitir especificar el placeholder que queremos que se muestre en el campo de texto. Esta props, como podemos imaginar tiene el nombre `placeholder`. Así si especificamos lo siguiente:

```javascript
<Field
  type='text'
  id='channel'
  name='channel'
  placeholder='Youtube channel name'
/>
```

si ahora vemos el marcado hmtl que será renderizado asociado al campo anterior será el siguiente:

```html
<input type='text' name='channel' id='channel' placeholder='Youtube channel name' />
```

Así pues, a modo de resumen, podemos decir que cualquier prop adicional que definamos en el componente `Field` pasará a ser renderizado por el elemento html o del componente de React que se encargará de renderizar el campo del formulario (cosa que veremos más adelante). En otras palabras, pasará a través del componente `Field` hacia el elemento o componente a renderizar.

Vamos a ver ahora cómo podemos utilizar el componente `Field` pero esta vez para renderizar un campo de nuestro formulario que no tiene por qué ser un campo de texto. Por ejemplo, vamos a suponer que en formulario de ejemplo que hemos estado siguiendo hasta ahora en el tutorial queremos añadir un campo adicional que será un `<textarea>` para que el usuario introduzca todos aquellos comentarios adicionales que quiera aportar asociados a la votación que ha realizado. 

Para ello lo primero que tendremos que hacer será crear un nuevo atributo en el objeto `initialValues` donde recogemos los valores iniciales para cada uno de los campos del formulario. Si llamamos a este nuevo campo `comments` definiremos el siguiente atributo:

```javascript
const initialValues = {
  name: '',
  email: '',
  channel: '',
  comments: ''
}
```

Como podemos observar el valor que asignamos a este nuevo campo `comments` será el string vacío ya que no queremos se muestre nada dentro del mismo.

En segundo lugar tendremos que definir el código JSX que permita renderizar este nuevo campo. Lo que queremos será renderizar el nuevo campo tras el campo `channel` por lo que escribimos lo siguiente:

```javascript
<div className='form-control'>
  <label htmlFor='comments'>Comments:</label>
  <Field
    id='comments'
    name='comments'
  />
</div>
````

Si esto lo dejamos así cuando se muestre el nuevo campo del formulario será un campo de texto (en otras palabras un elemento html `<input>`) y no un `textarea` que es lo que deseamos porque un usuario podrá escribir cuantas líneas desee como comentarios. Es en este punto donde entra en juego una nueva prop de componente `Field` que recibe el nombre de `as` donde simplemente especificaremos cómo queremos que sea renderizado el campo del formulario. Así, en nuestro, ejemplo, como queremos renderizar el campo como un `<textarea>` simplemente definiremos:

```javascript
<div className='form-control'>
  <label htmlFor='comments'>Comments:</label>
  <Field
    id='comments'
    name='comments'
    as='textarea'
  />
</div>
````

---
**Nota:** nos quedaría por definir los estilos CSS que queremos que se apliquen a los elementos de `<textarea>` dentro de nuestro formulario para que estén en concordancia con el resto de elementos de nuestro formulario. Así pues definimos el siguiente en el archivo `App.css` que contiene los estilos de nuestra aplicación:

```css
input[type='text'],
textarea {
  display: 'block';
  width: 400px;
  padding: 6px 12px;
  font-size: 14px;
  line-height: 1.5;
  color: #555;
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 4px;
}
```

---

¿Qué valores acepta la prop `as` del componente `Field`? La respuesta es el string `input` (que es el valor por defecto), `textarea` (para renderizar un elemento `<textarea>`), `select` (para renderizar un elemento `<select>`) o un componente de React que será el encargado de mostrar el campo del formulario.

---
**Nota:** en este momento no tenemos que preocuparnos por saber cómo se va a renderizar un elemento `<select>` o cómo utilizar un componente de React para mostrar un campo de un formulario ya que es un aspecto que dejamos para más adelante en este tutorial.

---
**Nota:** existe una propiedad que es equivalenete a `as` denominada `component` la cual puede recibir los mismos valores (en otras palabras, son equivalentes) pero cuyo nombre puede llevar a equivocación porque puede parecer que siempre ha de recibir como valor un componente de React para renderizar el campo del formulario cuando puede ser un texto. Es por ello que se recomienda utilizar la prop `as` siempre.

---

El último aspecto que tenemos que tener en cuenta a la hora de trabajar con el componente `Field` tiene que ver con el patrón de diseño **render props** muy común cuando estamos trabajando con aplicaciones escritas en React. Vamos a ver cómo gracias a este patrón tendremos mucho más control sobre cómo queremos que se muestre el campo de nuestro formulario.

---
**Nota:** no nos vamos a entretener tratando de explicar en profundidad cómo funciona el patrón **render props** ya que queda fuera del alcance de este tutorial pero para todos aquellos lectores que estén interesados en profundizar en el tema les recomendamos leer [la documentación oficial](https://reactjs.org/docs/render-props.html) donde se explica cómo funciona.

---

Vamos a ver cómo podemos utilizar el patrón **render props** dentro de nuestro formulario de ejemplo con el fin de mostrar un campo de texto similar al que estamos utilizando hasta ahora gracias a las posibilidades que nos ofrece el componente `Field` de **Formik**. Para ello vamos a crear un nuevo campo dentro de nuestro formulario con el fin de recoger la dirección del usuario que realiza la votación. 

---
**Tip:** para recoger la dirección del usuario podríamos hacer los mismo que estamos haciendo con los campos `name`, `email` o `channel` que hemos utilizado hasta ahora, pero tenemos que entender que queremos ir un paso más allá viendo cómo podemos utilizar el patrón **render props**.

---

El primer paso, como sucede con cualquier otro campo del formulario, será definir el valor inicial para el mismo como un atributo más del objeto `initialValues`. Así definimos el atributo `address` dentro de este objeto y como queremos que el campo no tenga inicialmente ningún valor asociado, le asignaremos la cadena vacía:

```javascript
const initialValues = {
  name: '',
  email: '',
  channel: '',
  comments: '',
  address: ''
}
```

El siguiente paso consistirá en añadir el código JSX que se encargue de mostrar el nuevo campo del formulario siguiendo un patrón similar al del resto de los campos de nuestro formulario:

```javascript
<div class='form-control'>
  <label htmlFor='address'>Address</label>
  <Field name='address' />
</div>
```
---
**Nota:** Este campo queremos que sea mostrado tras el campo `comments` por lo que nos aseguramos que el código JSX aparezca a continuación del código JSX que se encarga de mostrar el campo `comments`.

---

Con el código JSX anterior el nuevo campo de nuestro formulario funcionaría correctamente (se mostraría un elemento `<input>`para recoger la entrada del usuario) pero vamos a dar un paso más y aplicar ahora el patrón **render props**. Lo primero que tenemos que saber es que en este patrón tenemos que definir una función como el elemento hijo (en la terminología de React `children`) de nuestro componente. Esto quiere decir que el nuestros componente `Field` ahora tendrá un elemento de apertura y otro de cierre:

```javascript
<div class='form-control'>
  <label htmlFor='address'>Address</label>
  <Field name='address'>
  </Field>
</div>
```

Vamos ahora a ocuparnos de la función hija (`children`). Lo primero que tenemos que saber es que se tratará de una **arrow function** de ES6 la cual no recibirá, en principio, ningún parámetro y que ha de retornar el código JSX que será el que mostrará el campo de nuestro formulario:

```javascript
<div class='form-control'>
  <label htmlFor='address'>Address</label>
  <Field name='address'>
    () => {
      return <input type='text' id='address' />
    }
  </Field>
</div>
```

---
**Nota:** los conceptos propios de **ES6**, como las **arrow functions** quedan fuera del ámbito de estudio de este manual por lo que recomentamos a todos aquellos lectores que estén interesados en aprender más al respecto que [leen la información en MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) dedicadas a ellas.

---

Si dejamos el código anterior tal cual el nuevo campo de texto para recoger la información de la dirección del usuario no estará vinculado a **Formik** siendo este punto en el que entrarán en jueego las props que se reciben como parámetro de la arrow function que estamos utilizando. De hecho si modificamos el cuerpo de la arrow function para que muestre por al consola de JavaScript este objeto escribiríamos algo como lo siguiente:

```javascript
<div class='form-control'>
  <label htmlFor='address'>Address</label>
  <Field name='address'>
    props => {
      console.log('render props', props)
      return <input type='text' id='address' />
    }
  </Field>
</div>
```

Y si guardamos nuestro formulario y lo mostramos dentro de un navegador web con la consola de JavaScript dentro de la herramientas para desarrolladores veremos que en la consola se muestra algo como lo siguiente:

```javascript
Render props
> { field: {...}, form: {...}, meta: {...}}
  > field: { name: 'address', value: '', onChange: f, onBlur: f }
  > form: { values: {...}, errors: {...}, touched: {...}, status: undefined...}
  > meta: { value: '', error: undefined, touched: false, initalValue: ...}
  __proto__: Object
```

Es decir, que estamos viendo que el objeto props está formado a su vez por tres atributos `field`, `form` y `meta` los cuáles a su vez tienen asignados objetos. De hecho, el objeto asignado al atributo `field` contiene los atributos `name` (con el valor `address`) que define el nombre del campo del formulario dentro del estado del mismo en **Formik**, `value` que contiene el valor actual del campo del formulario dentro del estado en **Formik** (que inicialmente será la cadena vacía ya que así lo hemos definido), además de los métodos `onChange` (que se corresponderá con el método `handleChange` de **Formik**) y `onBlur` (que se correspondenrá con el método `handleBlur` de **Formik**). En definitiva, podemos concluir que dentro del objeto asociado al atributo `field` tendremos todos los elementos que **Formik** precisa para manejar el estado de este nuevo campo.

El objeto asociado al atributo `form` es realmente el objeto **Formik** con todos los métodos y atributos que están a nuestra disposición para trabajar con los formularios. Se trata pues, del objeto **Formik** que retornaba la invocación del hook `useFormik` que [hemos visto anteriormente](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/formik/03_useFormik_hook.md) y que pondrá a nuestra disposición todos las propiedades los métodos que nos van a permitir trabajar con nuestros formularios. De hecho, si nos fijamos en la salida por la consola, podemos el objeto `values`, el objeto `errors`, el objeto `touched`, etc.

---
**Nota:** en este punto no tenemos que preocuparnos por el resto de atributos ni métodos del objeto **Formik** ya que más adelante en el tutorial veremos cómo podemos sacar más partido de los mismos.

---

El tercer atributo que nos ofrece el objeto props es `meta` el cual está asignado a su vez a un objeto el cual nos sirve para indicarnos si el campo del formulario en cuestión ha sido visitado o no (gracias a su atributo `touched` siendo `true` en el caso de que haya sido visitado o `false` en caso contrario), si hay algún error relacionado con el mismo (gracias al atributo `error`) además de su valor actual (gracias al atributo `value`). 

---
**Nota:** el objeto `meta` nos ofrece otros atributos como `initialError`, `initialTouched` o `initialValue` que sirven para indicar si existe un primer error en el campo del formulario (si no lo hay el valor del campo `initialError` será `undefined`), si se ha de considerar inicialemente cómo que ha sido visitado (que por defecto será el valor `false` indicando que todavía no ha sido visitado) o el valor inicial del campo (que será el valor del atributo homónimo al campo en el objeto `initialValues` que utilizamos para crear nuestro formulario). Con todos ellos vamos a poder controlar mucho mejor cómo queremos que se muestre nuestro campo del formulario.

---

Al final, en lo que se traduce todo esto, es que el objeto `meta` que recibimos como uno de los atributos props nos servirá para determinar si se ha de mostrar el mensaje de error asociado al campo del formulario o no. Es más podemos concluir que de los tres objetos que recibimos serán los objetos `field` y `meta` se utilizarán para renderizar el campo en cuestión miestras que `form` se utiliza para controlar el formulario en sí mismo.

Lo primero que vamos a hacer dentro de la función es deconstruir las propiedades del objeto prop que se reciben como parámetro gracias a la deconstrucción de ES6 (además quitamos la sentencia `console.log`): 

```javascript
<div class='form-control'>
  <label htmlFor='address'>Address</label>
  <Field name='address'>
    props => {
      const { field, meta } = props
      return <input type='text' id='address' />
    }
  </Field>
</div>
```

Si [recordamos de anteriormente](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/formik/04_Managing_Form_State.md) para vincular un campo de texto con **Formik** tendremos que, en el elemento html `<input>` asignarle el atributo `name` con el nombre del atributo dentro del estado del formulario que se encargará de guardar su valor, así como los eventos `onChange` y `onBlur` que tendremos que vincularlos a los **helper methods** de **Formik** `handleChange` y `handleBlur` respectivamente. Sabiendo que toda esta información la tenemos en el objeto `field` que acabamos de desconstruir podemos hacer uso del spread operator de ES6 para asignar el valor de estos atributos html:

```javascript
<div class='form-control'>
  <label htmlFor='address'>Address</label>
  <Field name='address'>
    props => {
      const { field, meta } = props
      return <input type='text' id='address' { ...field }/>
    }
  </Field>
</div>
```

---
**Nota:** no es nuestro objetivo profundizar en cómo se utiliza la desconstrucción ni el spread operator de ES6 por lo que dejamos que el lector que esté interesado en profundidad que consulte MDN para obtener más información al respecto ([desconstrucción](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) y [spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax).

---

El siguiente paso que tendremos que dar será escribir el código que se encargará de mostrar el mensaje de error que estará asociado a nuestro campo (si es que hay algún error). Para ello tendremos que utilizar el objeto `meta`. Aunque ahora mismo no tenemos ninguna regla de validación asociada a este campo dentro del formulario (no hemos definido ninguna información para el mismo en la objeto `validationSchema`) vamos a mostrar código JSX que se encargaría de mostrar el error:

```javascript
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
```

Sabemos que el objeto `meta` asociado a las props que recibe la función contiene información sobre los errores de validación sobre el campo que se va a mostrar por lo que lo que hemos hecho ha sido, en primer lugar, envolver el código de nuestro nuevo campo en un `<div>` con el fin de que no haya errores en la estructura html y justo tras mostrar el campo de texto lo que hacemos es comprobar si el campo ha sido visitado (gracias a que existe el campo `meta.touched` y además que el valor del mismo `true`) y si existe un mensaje de error (gracias a que existe el campo `meta.error` y además que tiene asignado un campo que no es una cadena vacía); si es así, se mostrará este mensaje de error dentrod e un elemento `<div>`

Todo este patrón **render props** para escribir los campos de nuestros formularios puede parecer a simple vista complejo por lo que es más que probable que tengamos que volver a leer este punto más de una vez para poderlo comprender en su totalidad. Ahora bien, la mayoría de las veces no será necesario que las utilicemos en nuestro día a día pero sí que es conveniente que sepamos de su existencia para poder afrontar casos de uso que en principio podrían ser más complejos de solucionar.

En resumen en este punto hemos visto lo siguientes:

* Todas las props que se utilizan para definir el componente `Field` y que no son utilizadas por el mismo serán pasadas a resto de componentes que se utilicen para mostrar los campos de nuestros formulario.
* Deberemos utilizar la prop `as` del componente `Field` para determinar cómo queremos que se muestre nuestro campo del formulario (por defecto será como un campo de texto).
* Hemos visto cómo podemos utilizar el patrón **render props** para poder ligar **Formik** con nuestros propios componentes que utilicemos para crear nuestros formularios.

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
