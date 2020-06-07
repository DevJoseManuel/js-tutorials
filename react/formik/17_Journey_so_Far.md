# Lo que hemos visto hasta ahora

En este punto vamos a hacer un pequeño repaso de todos los aspectos que hemos estudiado hasta ahora antes de continuar avanzando hacia conceptos más complejos. Comenzamos nuestro camino creando un formulario muy sencillo en el que mostrábamos tres campos en los que los usuarios deberán proporcionar su nombre, dirección de correo eléctronico y un nomber de un canal de Youtube por el que se quiere registrar un voto. Y no solamente eso, sino que el formulario de partida ha sido construido sin utilizar para nada React.

Tras ello hemos visto cómo podemos instalar **Formik** en nuestro proyecto y cómo comenzar a utilizarla gracias al uso del hook `useFormik` que lo que nos va a permitir es conectar el código html con el código de React. Es más, hemos visto que gracias a **Formik** vamos a poder gestionar tres aspectos que siempre hemos de tener presentes cuando trabajamos con formularios:

1. La gestión del estado de los formularios.
2. La gestión del envío de los campos de los formularios.
3. La gestión de los errores de validación.

Para poder trabajar con el estado del formulario hemos visto que tendremos que utilizar el objeto `initialValues` (donde definimos los valores por defecto o iniciales para los campos de nuestros formularios) junto con el método `handleChange` que nos proporciona el objeto **Formik** que retorna la invocación del hook `useFormik`.

Para poder controlar el envío de los datos de los formularios tenemos a nuestra disposición el atributo `onSubmit` del elemento `<form>` junto con el método `handleSubmit` del objeto **Formik** retornado por la invocación del hook `useFormik`.

En lo que respecta la validación de los datos de los formularios hemos tenido que ver varios puntos que todos juntos nos han permitido lograr el objetivo perseguido: 

* Por una parte hemos visto cómo utilizar el **validation function** la cual ha de retornar un objeto donde cada uno de los atributos será cada uno de los campos del formulario sobre los que hay un error de validación y como valor de estos atributos el mensaje de error que se deberá mostrar al usuario.
* Además hemos visto que podemos utilizar la librería **yup** junto con **Formik** para realizar las validaciones de los datos de nuestros formularios. En concreto hemos visto cómo podemos utilizar el objeto `validationSchema` donde se ha de recoger un atributo por cada uno de los campos de nuestro formulario que queramos validar y como valor asociado las reglas de **yup** que se han de cumplir (el esquema) junto con los mensajes de error que deberán mostrarse en el caso de que no se cumplan.
* Una vez tenemos los errores de los campos de nuestros formularios hemos visto como gracias al objeto que está vinculado al atributo `errors` (que recoge los mensajes de error de cada uno de los campos del formulario) y `touched` (que recoge la información de los campos del formulario definiendo un atributo por cada uno de los campos y el valor `true` en el caso de que el campo haya sido visitado) del objeto **Formik** para determinar si se ha de mostrar un mensaje de error o no.

Una vez hemos estudiado los aspectos básicos en los que se apoya **Formik** para su funcionamiento que hemos dado un paso más viendo alguno de los componentes de React que la librería nos ofrece para poderlos utilizar en nuestros componentes:

* `Formik` componente que no es más que un proveedor del contexto de React que va a permitir que desde otras partes del componente se tenga acceso a todas las funcionalidades de **Formik**.
* `Form` que no es más que un envoltorio para la etiqueta html `<form>`.
* `Field` que nos va a permitir trabajar con los campos de texto que vamos a definir en nuestros formularios.
* `ErrorMessage` que se encargará de tener en cuenta cuándo se ha de mostrar los mensajes de error asociados a cada uno de los campos de nuestros formularios.

Así pues recomendamos a todos los lectores de este tutorial que si no han comprendido alguna de estas partes que se detenga aquí, [acuda al punto](https://github.com/DevJoseManuel/js-tutorials/blob/master/react/formik/00_Cover.md) a partir del cual las cosas no están tan claras antes de continuar con el resto de los puntos de este tutorial para evitar que los conocimientos que vayamos adquiriendo sean cada vez más confusos.
