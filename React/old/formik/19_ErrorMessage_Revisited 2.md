# Revisando el componente `ErrorMessage`

En este punto vamos a deternernos en estudiar con más detalle el componente `ErrorMessage` de **Formik** que [hemos estudiado anteriormente](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/formik/16_ErrorMessage_Component.md). Si recordamos el componente `ErrorMessage` acepta una prop denominada `name` que tendrá como valor el nombre del componente para el cual se ha de reenderizar el mensaje de error, además de garantizarnos que el mensaje de error únicamente se mostrará en el caso de que el campo en cuestión haya sido visitado y además que exista un mensaje de error para el mismo.

Lo primero que tenemos que entender es que el mensaje de error que esté asociado al campo del formulario se muestra como un nodo de texto dentro del DOM que representa a la página en la que se está mostrando el formulario. Esto se ve mejor en nuestro ejemplo donde el código para mostrar el mensaje de error para el campo `name` de nuestro formulario es el siguiente:

```javascript
<Field
  type='text'
  id='name'
  name='name'
/>
<ErrorMessage name='name' />
```

Si cargamos el formulario dentro de un navegador web y nos situamos en el campo para recoger el nombre del usuario, no escribimos nada en el mismo y pulsamos fuera (lo que garantizará, por una parte, que el campo `name` ha sido visitado y que además está vacío) se mostrá el mensaje de error de validación sobre dicho campo. Es más, si inspeccionamos el código html dentro del DOM donde se muestra el mensaje veremos lo siguiente:

```html
<div class='form-control'>
  <label for='name'>Name</label>
  <input type='text' name='name' id='name' value='' />
  Required!
</div>
```

Es decir que el mensaje que recoge el error sobre el campo `name` no aparece contenido dentro de ningún elemento html. ¿Qué podemos hacer para conseguir que el mensaje de error se muestre dentro de un elemento html que creemos para ese cometido? La respuesta pasa por hacer uso de una de las props que acepta el componente `ErrorMessage`, en concreto la prop `component`. Ahora bien, ¿qué valores acepta esta prop? Pues un string que identifique el elemento html con el que queremos que se rodee al mensaje de error.

De esta manera si queremos rodear el mensaje de error en nuestro ejemplo con un elemento `<div>` escribiremos lo siguiente:

```javascript
<ErrorMessage
  name='name'
  component='div'
/>
````

Con esto si ahora volvemos a repetir la secuencia de pasos para volver a reenderizar el formulario dentro de un navegador web y observamos el marcado html dentro del DOM ahora aparecerá de la siguiente manera:

```html
<div class='form-control'>
  <label for='name'>Name</label>
  <input type='text' name='name' id='name' value='' />
  <div>Required!</div>
</div>
```

Por lo tanto, con esto hemos conseguido que el mensaje de texto aparezca rodeado de un elemento `<div>` como pretendíamos. Pero no solamente esto, la prop `component` puede aceptar como valor un componente de React en vez de un string. Pero ¿cómo podemos hacer esto? Pues siguiendo con nuestro ejemplo crearemos un componente de React que lo único que hará será mostrar el mensaje de error dentro de un elemento `<div>` en el que el texto que contiene se mostrará en color rojo.

Comenzamos creando un nuevo fichero dentro del directorio `src/component` al que vamos a llamar `TextError.js` el cual va a contener un nuevo **Functional Component** de React y cuyo código será el siguiente:

```javascript
import React from 'react'

function TextError(props) {
  return (
    <div className='error'>
      { props.children }
    </div>
  )
}

export default TextError
```

Lo único que hace este functional component es renderizar una elemento `<div>` que tendrá la clase CSS `error` (que ya está definido dentro del fichero CSS que tiene los estilos que se están aplicando en nuestro formulario) y dentro del mismo cualquier cosa que se haya pasado como `children` del componente. En nuestro caso en concreto, es decir el mostrar los mensajes de error de nuestro formulario, será el mensaje texto que renderizaremos al no pasar la validación del campo.

Ahora ya simplemente utilizamos este nuevo componente en primer lugar lo tendremos que importar en el código: 

```
```javascript
import React from 'react'
import { Formik, Form, Field } from 'formik'
import * as Yup from 'yup'
import TextError from './TextError'
```

Y simplemente utilizamos este componente como el valor que asignamos a la prop `component` del componente `ErrorMessage` de **Formik** de la siguiente manera:

```javascript
<ErrorMessage
  name='name'
  component={ TextError }
/>
```

Al igual que hemos hecho en [punto anterior cuando hemos estudiado en profundidad el componente `Field`](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/formik/18_Field_Revisited.md) podemos utilizar el patrón **render props** para escribir el mensaje de error asociado a un campo utilizando el componente `ErrorMessage`. Vamos a ver cómo podemos aplicarlo en el caso del mensaje de error que se muestra asociado en el campo `email` lo que supone que en primer tendremos que cambiar el componente `ErrorMessage` para que muestre la etiqueta de apertura y cierre:

```javascript
<Field
  name='email'
  id='email'
/>
<ErrorMessage name='email'></ErrorMessage>
```

El siguiente paso será definir com hijo del componente `ErrorMessage` una función (en nuestro caso una arrow function) la cual recibirá como parámetro el mensaje de error y que ha de retornar el código JSX que se utilizará para mostrar el mensaje de error asociado al campo. En nuestro caso:

```javascript
<Field
  name='email'
  id='email'
/>
<ErrorMessage name='email'>
  { errorMsg => <div className='error'>{ errorMsg }</div> }
</ErrorMessage>
```

---
**Nota:** los conceptos propios de **ES6**, como las **arrow functions** quedan fuera del ámbito de estudio de este manual por lo que recomentamos a todos aquellos lectores que estén interesados en aprender más al respecto que [leen la información en MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) dedicadas a ellas.

---

A modo de resumen antes de continuar con nuestro tutorial:

* Para poder mostrar un mensaje de error asociado a un campo de un formulario con **Formik** podemos utilizar el componente `ErrorMessage` que nos proporciona la librería.
* Si queremos mostrar un elemento html para mostrar el contenido del mensaje de error podremos utilizar la prop `component` asignándole como valor un string con el nombre de la etiqueta que rodeará al mensaje de error o bien utilizar un componente de React que será el encargado de mostrar el mensaje de error de la forma en la que queramos (no estando limitados únicamente a utilizar un único elemento html).
* Además podemos utilizar el patrón **render props** para definir una función que recibirá como parámetro el mensaje de error que queremos que se muestre asociado al campo del formulario.

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

        <button type='submit'>Submit</button>
      </Form>
    </Formik>
  )
}

export default YoutubeForm
````
