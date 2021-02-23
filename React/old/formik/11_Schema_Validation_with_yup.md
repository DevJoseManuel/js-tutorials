# Validación utilizando `yup`

En este punto vamos a ver la alternativa que tenemos para poder realizar las validaciones de los campos de nuestros formularios utilizando **Formik** y es utiliar la librería **yup**. 

Como siempre que trabajamos con una nueva librería el primer paso que tendremos que hacer será instalarla en nuestro proyecto por lo que desde la terminal de comandos de nuestro equipo escribiremos:

```javascript
$ npm install yup
```

Una vez finaliza la instalación para poderla utilizar dentro del componente que estamos desarrollando a lo largo del manual, lo primero que tendremos que hacer será importarla dentro del mismo:

```javascript
import React from 'react'
import { useFormik } from 'formik'
import * as Yup from 'yup'
```

---
**Nota:** con la instrucción de importación que hemos realizado lo que estamos consiguiendo es importar todas las funciones y constantes que son exportadas dentro de la librería **yup** para que estén disponibles a través de un objeto al que hemos denominado `Yup`. Así si, por ejemplo, existe una función que es exportada con el nombre del `foo` podremos acceder a la misma como `Yup.foo()`.

---

En este tutorial no vamos a explicar cómo internamente funciona **yup** si no que lo que perseguiremos será ver cómo podemos utilizar esta librería para realizar las validaciones de los formularios de **Formik** dejando al lector que esté interesado en ello que visite la [página del proyecto en GitHub](https://github.com/jquense/yup).

El siguiente paso que tenemos que dar consistirá en la escritura de lo que en **yup** se conoce como un Esquema de Validación (**Validation Schema**) que no es más que un objeto de JavaScript donde tenemos qué recoger las validaciones que queremos que se realicen sobre los campos de nuestro formulario. Así pues comenzamos definiendo un nuevo objeto en el código de nuestro componente al que vamos a denominar `validationSchema` y vamos a asignarlo a uno de los objetos que nos proporciona **yup** en concreto al objeto que representa el **validation schema**:

```javascript
const validationSchema = Yup.object()
```

Vemos, por lo tanto, que para crear el objeto que representa el **validation schema** simplemente deberemos invocar al métood `object` que nos proporciona la librería **yup**. Ahora bien ¿qué es lo que deberemos pasarle como parámetro? La respuesta es un objeto en el que tenemos que definir las reglas de validación que queremos que se cumplan sobre los campos de nuestro formulario. La siguiente pregunta que surge es ¿y cómo haremos para definir estas reglas? Pues tendremos que especificar, por cada uno de los campos que forman nuestro formulario y que queremos que sean validados un atributo cuyo nombre ha de ser el mismo que el del campo del formulario que se va a validar (y por lo tanto, que el nombre del atributo del objeto `values` dentro de **Formikk**) y le asignaremos como valor la regla de validación que queremos que se cumpla.

Esto vuelve a sonar demasiado complejo pero con un ejemplo será muy fácil de entender. Volviendo a nuestro formulario de ejemplo sabemos que sobre el campo `name` hemos definido que no podrá ser vacío, es decir, que el campo es obligatorio (requerido). En **yup** tendremos que especificar que el campo `name` ha de ser un string y además que dicho campo es obligatorio:

```javascript 
const validationSchema = Yup.object({
  name: Yup.string().required('Required')
})
```

Con esto estamos haciendo uso de la función `string()` que nos proporciona **yup** y que nos sirve para indicar que esperamos que el campo `name` contenga un string. Esta invocación devolverá un objeto que nos proporciona el método `required` que se encargará de verificar que dicho campo tiene un valor asignado (es obligatorio). Este método, además, recibe como parámetro un string que será el mensaje que queremos que se muestre cuando no se cumpla la regla de validación.

Ahora bien ¿cómo podemos hacer en el caso de que se tengan que cumplir dos validaciones en uno de nuestros campos del formulario como sucede con el campo `email`? La respuesta es que podemos encadenar llamadas métodos de verificación para lograr el objetivo perseguido ya que siempre estos métodos retornarán el mismo objeto para seguir realizando las validaciones necesarias. En nuesto caso:

```javascript 
const validationSchema = Yup.object({
  name: Yup.string().required('Required'),
  email: Yup.string().required('Required').email('Invalid email format')
})
```

Con esto estamos diciendo que esperamos que el campo `email` contenga un string, que dicho string no sea vacío y además que dicho string siga el formato de una dirección de correo electrónico válida. Esto lo lograremos porque la función `string()` retorna un objeto sobre el que invocamos al método `required` el cual, si no se produce un error, retornará un objeto (que vuelve a ser el objeto que ha retornado la función `string()`) sobre el que invocaremos al método `email` para comprobar que el campo contiene una dirección de correo electrónico válida.

Siguiendo con este razonamiento ya podemos escribir la validación sobre el campo `channel` donde lo único que se ha de comprobar es que es obligatorio:

```javascript 
const validationSchema = Yup.object({
  name: Yup.string().required('Required'),
  email: Yup.string().required('Required').email('Invalid email format'),
  channel: Yup.string().required('Required')
})
```

El siguiente paso que tendremos que dar consistirá en indicarle a **Formik** que queremos utilizar este objecto con el **validation schema** de **yup** para validar los datos de nuestro formulario y para ello volvermos a hacer uso del objeto que pasamos como parámetro al hook `useFormik`. Para ello lo primero que tendremos que hacer es indicar ya no queremos utilizar la función de validación que estamos usando hasta ahora (vamos a usar **yup** para llevar a cabo esta tarea) por lo que ya no necesitaremos el atributo `validate` dentro de este objeto:

```javascript
const formik = useFormik({
  initialValues,
  onSubmit,
  // lo quitamos porque ya no lo vamos a utilizar.
  // validate
})
`````

Y pasamos a definir un nuevo atributo dentro del este objeto que recibe el nombre de `validationSchema` al cual le tenemos que asignar el valor de un objeto que define un **validation schema** de **yup**. En nuesto caso, haciendo uso de la shorthand syntax de ES6 para definir los atirbuto de los objetos, definiremos directamente lo siguiente:

```javascript
const formik = useFormik({
  initialValues,
  onSubmit,
  validationSchema
})
`````

---
**Nota:** al hacer uso de **yup** ya no vamos a necestiar más nuestra función `validate`en el código de nuestro componente por lo que la quitamos.

---

Si nos fijamos en la [documentación de la librería **yup**](https://github.com/jquense/yup) y poco a poco vamos incorporando más y más capacidades acabaremos creando todas las validaciones sobre nuestros formularios haciendo uso de sus capacidades.

Llegados a este punto hemos visto todos aquellos aspectos básicos que tienen que ver con la gestión de los formularios de nuestras aplicaciones que recordemos que son:

1. Gestionar el estado del formulario (qué campos posee y qué valores están asignados a cada uno de ellos).
2. Gestionar el envío del formulario para su tratamiento.
3. Gestión de los errores de validación en los campos de entrada.

Pero esto no quiere decir que hayamos terminado de ver todas las capacidades que nos ofrece **Formik** para gestionar nuestros formularios ya que un conocimiento más profundo del mismo nos ayudará todavía más en la creación y mantenimiento del código de nuestros componentes. En los próximo puntos seguiremos profundizando en esta librería añadiendo más y más herramientas para nuestro desarrollo día a día.

## Componente final

El código completo de nuestro componente tras la finalización de este punto es el que se muestra a continuación:

```javascript
import React from 'react'
import { useFormik } from 'formik'
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
  const formik = useFormik({
    initialValues,
    onSubmit,
    validationSchema
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
