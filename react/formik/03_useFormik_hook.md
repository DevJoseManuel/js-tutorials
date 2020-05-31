## useFormit hook

Supongamos que partimos del siguiente formulario dentro de uno de los componentes de nuestra aplicación donde le pedimos al usuario un nombre, una dirección de correo electrónico y el nombre de un canal de Youtube:

```javascript
import React from 'react'

const YoutubeForm = () => (
  <div>
    <form>
      <label htmlFor='name'>Name</label>
      <input type='text' id='name' name='name' />

      <label htmlFor='email'>Email</label>
      <input type='text' id='email' name='email' />

      <label htmlFor='channel'>Channel</label>
      <input type='text' id='channel' name='channel' />

      <button>Submit</button>
    </form>
  </div>
)

export default YoutubeForm
```

Al igual que sucede con cualquier otra librería con la que vayamos a trabajar en nuestras aplicaciones, lo primero que tenemos que hacer es instalar **Formik** en nuestro proyecto. Para ello abrimos una terminal de comandos en nuestra máquina y escribiremos:

```console
$ npm install formik
+ formik@2.1.4
added 13 packages from 11 contributors and audited 13 packages in 1.127s
found 0 vulnerabilities
```

Una vez finaliza la instalación ya podemos comenzar a trabajar con la librería. Lo primero que tenemos que hacer es importar dentro del código del componente el hook `useFormik`:

```javascript
import React from 'react'
import { useFormik } from 'formik'
```

Básicamente un hook es una función por lo que para poderlo utilizar tendremos que invocarla dentro del código de nuestro componente. Esto quiere decir que tenemos que modificar el código de nuestro componente para que acepte la ejecución de código antes de retornar e invocar a este hook:

```javascript
const YoutubeForm = () => {
  // Invocamos el hook (más adelante estudaremos los parámetros)
  useFormik()
  return (
    <div>
    //...
    </div>
  )
}
```

El hook `useFormik` acepta como parámetro un objeto (el cual iremos estudiando de forma sucesiva a lo largo de este tutorial) y retornará un objeto que nos ofrecerá una serie de atributos y métodos que podremos utilizar para construir nuestros formularios. Teniendo esto en cuenta modificamos nuestro código para que la invocación del hook se realice pasándole como parámetro un objeto vacío y asignamos el resultado de la invocación a una variable que denominaremos `formik`:

```javascript
const formik = useFormik({})
```

El objeto `formik` que nos retorna el hook `useFormik` es el que nos va a permitir realizar las tres tareas que están asociadas al trabajo con los formularios dentro de nuestras aplicaciones:

1. Gestionar el estado del formulario (es decir, qué campos tenemos y qué valores tienen asociados).
2. Gestionar el envío del formulario.
3. Realizar la validación de los campos del formulario y gestionar los mensajes de error.

---
**Nota:**
A lo largo de este tutorial volveremos a mencionar varias veces estos tres puntos ya que son la razón de ser de una librería como **Formik**.

---


## Componente final

El código completo de nuestro componente tras finalizar este apartado es el que mostramos a continuación:

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

        <button>Submit</button>
      </form>
    </div>
  )
}

export default YoutubeForm
```
