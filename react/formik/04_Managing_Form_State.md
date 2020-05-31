# Gestión del estado del formulario

En este punto vamos a ver cómo **Formik** nos puede ayudar para realizar la gestión del estado de un formulario y, lo que también es importante, intentar aclarar qué es lo que entenderemos por el estado de un formulario.

En el punto anterior habíamos creado un formulario para solicitar al usuario un nombre, una dirección de correo electrónico y el nombre de un canal de Youtube. El código de este componente tenía el siguiente aspecto:

```javascript
import React from 'react'
import { useFormik } from 'formik'

const YoutubeForm = () => {
  const formik = useFormik({})

  return (
    <div>
      <form>
        <label htmlFor='name'>Name</label>
        <input type='text' id='name' name='name' />

        <label htmlFor='email'>Email</label>
        <input type='text' id='email' name='email' />

        <label htmlFor='channel'>Channel</label>
        <input type='text' id='channel' name='channel' />
      </form>
    </div>
  )
}

export default YoutubeForm
```

Tal y como está ahora el código no estaremos recogiendo en ningún sitio los valores que los usuarios puedan introducir en cada uno de los tres campos. Sabemos que cuando un usuario teclea un caracter dentro de cualquiera de los tres campos del formulario el valor que está asociado a dicho campo también cambiará. En **React** para poder recoger estos cambios en los valores deberíamos guardarlos dentro del estado que está asociado al componente. Así pues, como nuestro formulario está formado por tres campos de texto deberíamos tener tres atributos dentro del estado del componente para poder recoger los valores asociados a cada uno ellos. Al conjunto formado por estos tres atributos es lo que se conocerá como el **estado del formulario (form state)***.

En **Formik** para recoger el estado de formulario lo que han hecho los desarrolladores de la librería es crear un objeto que contendrá un atributo por cada uno de los campos del formulario (más adelante lo veremos con detalle) y el valor que estará asociado a cada uno de estos atributos será el valor que tiene el campo dentro del formulario. Así, en nuesto ejemplo este objeto estará formado por los campos `name`, `email` y `channel`.

Pero no solamente esto, **Formik** nos va a garantizar que cuando el usuario envíe el formulario (submit) como resultado de la acción se enviará el objeto que recoge el estado del fomulario lo que supone que se enviará no solamente los campos que lo forman sino también los valores que han sido recogidos en cada uno de ellos.

En este punto vamos a detallar los pasos que tenemos que dar para vincular los campos que forman nuestros formulario con los atributos del objeto que recoge el estado del formulario dentro de **Formik**.

Lo primero que tenemos que recordar es que a la hora de invocar al hook `useFormik` hemos visto que se le tiene que pasar como parámetro un objeto:

```javascript
const formik = useFormik({})
```

Pues bien, en este objeto podemos definir la atributo `initialValues` que a su vez deberá tener asignado un objeto donde cada uno de los atributos representará a cada uno de los campos del formulario y su valor será el valor que queremos que inicialmente esté asignado a dicho campo la primera vez que el formulario es mostrado al usuario. Pero ¿cómo se realiza la vinculación entre los atributos de este objeto y cada uno de los campos del formulario? Para ello tenemos que fijarnos en el marcado JSX que está asociado a los campos del formulario. Pensemos en el campo `name`:

```javascript
<input type='text' id='name' name='name' />
```

La respuesta está en el atributo `name` de este código ya que tendremos que crear un atributo dentro del objeto `initialValues` por cada uno de los atributos `name` de nuestro marcado. Por lo tanto, en nuestro ejemplo tendremos que crear los atributos `name`, `email` y `channel` dentro del objeto `initialValues` tal y como sigue:

```javascript
const formik = useFormik({
  initialValues: {
    name: '',
    email: '',
    channel: ''
  }
})
```

Como podemos observar hemos incializado los valores asignados a cada uno de estos atributos a cadenas vacías lo que provocará que al usuario los campos de texto se le mostrarán inicialmente vacíos.

---
**Nota:** se ha de definir un valor inicial por cada uno de los atributos del formulario para que **Formik** pueda gestionar su estado y enviarlos cuando se produzca el evento `submit`.

---

El siguiente paso consistirá en asignar al atributo `onChange` asociado a cada uno de los campos del formulario uno de los métodos del objeto **Formik** que va a permitir recoger cualquier cambio en los valores del formulario que sea realizado por los usuarios en el estado del mismo dentro de objeto **Formik**. Además tendremos que definir el valor del atributo `value` para que asociado al campo se muestre el valor del mismo que está recogido dentro de **Formik**.

Todo esto que parece muy lioso de explicar es muy sencillo de entender cuando lo vemos gracias a nuestro ejemplo. Lo que estamos tratando de explicar es que tenemos que asignar los valores del los atributo `onChange` y `value` dento del código JSX que nos sirve para mostrar cada uno de los campos de nuestro formulario:

```javascript
<input type='text' id='name' name='name' onChange={} value={} />
```

En el caso del atributo `onChange` el valor que tendremos que asignarle será el método `handleChange` del objeto **Formik** (recordemos que este objeto es el que nos devuelve la invocación del hook `useFormik`):

```javascript
<input
  type='text'
  id='name'
  name='name'
  onChange={ formik.handleChange }
  value={}
/>
```

En el caso del atributo `value` en primer lugar tenemos que entender que **Formik** recoge los valores que actualmente tienen cada uno de los campos del formulario (es decir, los valores que está asociados a estos atributos en el estado del formulario) dentro de un objeto. Dicho objeto está vinculado al atributo `values` del objeto **Formik** el cual define un atributo por cada uno de los campos del formulario y el valor que tendrá asignado será el valor de dicho campo.

Nuevamente lo anterior es más complicado de explicar que de entender. Volviendo a nuestro ejemplo lo que tratamos de explicar es que en **Formik** tendremos un objeto que tendrá un atributo por cada uno de los campos del formulario (en nuestro caso los atributos `name`, `email` y `channel`) y el valor asignado a cada uno de ellos es el valor de los campos del formulario. Este objeto a su vez está asignado al atributo `values` del objeto **Formik**.

Es por ello que la asignación entre el valor de los campos del formulario y el valor que está recogido en el estado de **Formik** se realiza a través del objeto `values`. Así pues, en el caso del campo `name` del formulario escribiriamos lo siguiente:

```javascript
<input
  type='text'
  id='name'
  name='name'
  onChange={ formik.handleChange }
  value={ formik.values.name }
/>
```

Y simplemente con esta asignación tendremos la garantía de que **Formik** se encargará de gestionar la recogida y la muestra de los valores que están asociados a cada uno de los campos del formulario sin que nosotros tengamos que hacer nada más al respecto.

Esto mismo que hemos realizado dentro del código JSX que se encarga de definir el campo `name` tendremos que hacerlo también en la definición de los otros dos campos del formulario (`email` y `channel`).

---
**Nota:** inicialmente lo que **Formik** realiza cuando crea el formulario es crear un atributo dentro del objeto `values` por cada uno de los atributos recogidos en el objeto `initialValues` que se utiliza durante la invocación del hook `useFormik` asignándole como valores los que están recogidos dentro de `initialValues`.

Esto quiere decir que si queremos darle un valor inicial a alguno de los campos de nuestro formulario simplemente tedremos que asignarlo en el atributo asociado a dicho campo dentro del objeto `initialValues` y **Formik** se encargará de asignárselo al atributo correspondiente dentro del objeto `values` y monstrarlo al usuario la primera vez que se renderiza en la página (y en sucesivas ocasiones siempre que no cambie su valor).

---
**Tip:** podemos decir que el objeto `values` que está asociado a **Formik** se encargará de en todo momento reflejar el estado del formulario y por lo tanto es necesario que siempre lo vinculemos a cada uno de los campos del formulario.

---
**Tip:** por su parte `handleChange` es un método que nos ofrece **Formik** de los denominados **helpers** siendo el propósito del mismo el gestionar los cambios en los valores de los campos del formulario.

De hecho lo que básicamente hace este método es leer el valor del atributo `name` del campo sobre el que está definido y actualizar el valor de dicho atributo que tiene asociado dentro del objeto `values` del objeto **Formik**.

---

Recapitulando, **Formik** realiza los siguientes pasos cuando se utiliza para gestionar el estado de los formularios de nuestras aplicaciones:

1. Utiliza los atributos y los valores que está recodigos en el atributo `initialValues`dentro del objeto que se le pasa cuando se invoca al hook `useFormik` para crear dentro del objeto `values` un atributo por cada uno de ellos asignándole además los valores recogidos en `initialValues`. Esto lo que provocará es que la primera vez que se le muestre el formulario al usuario se mostrarán los valores iniciales que nosotros hayamos especificado para cada uno de los campos del formulario.
2. Cada vez que se produce un cambio en uno de los valores de los campos del formulario se disparará al evento `onChange` lo que hace que se invoque al método `handleChange` de **Formik** el cual, basándose en el valor del atributo `name` del elemento del formulario será capaz de actualizar el valor del atributo homónimo en el objeto `values` para que recoja la nueva entrada del usuario.
3. Como consecuencia del cambio del valor de uno de los atributos del objeto `values` se volverá a mostrar el campo del formulario (nuevo renderizado del componente) por lo que el campo recogerá el nuevo valor del mismo.

---
**Nota:** todos estos pasos suceden tan rápidamente que para el usuario son imperceptibles en el sentido de que percibirá que cuando modifica el valor de uno de los campos automáticamente se muestra el nuevo valor.

---

En definitiva ¿qué pasos tenemos que seguir para poder lograr que **Formik** gestione el estado de nuestros formularios por nosotros?

1. Definir los valores iniciales para cada uno de los campos del formulario gracias a la utilización del objeto `initialValues` cuando invocamos al hook `useFormik`.
2. Definir el valor de los atributos `onChange` para cada uno de los campos del formulario.
3. Definir el valor del atributo `value` asociado a cada uno de los campos del formulario para que recoja el valor que está asociado al mismo dentro del objeto **Formik**.

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
    }
  })

  return (
    <div>
      <form>
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
      </form>
    </div>
  )
}

export default YoutubeForm
```
