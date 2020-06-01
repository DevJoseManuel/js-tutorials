# Introducción

Todas las personas que nos dedicamos al desarrollo FrontEnd sabemos que los formularios que se muestran a los usuarios son una parte fundamental de casi todas las aplicaciones web que podamos desarrollar (pensemos solamente en la cantidad de veces que nos encontramos que formularios para acceder con nuestra cuenta a un sitio web, crear una nueva, etc.).

También sabemos que no solamente tenemos que preocuparnos porque el formulario se comporte como esperamos a nivel de lógica de código sino que hemos de ser capaces de crear una experiencia de usuario **UX** que ayude a que cuando se esté interaccionando con los campos que lo forman se haga de una manera intuitiva y eficiente.

Normalmente como desarrolladores a las tareas que nos tenemos que enfrentar en todos y cada uno de los formularios que formen parte de nuestra aplicación serán:

* Cómo gestionar los datos que son introducidos por el usuario.
* Cómo realizar la validación de estos datos.
* Cómo mostrar los errores asociados a los datos introducidos.
* Cómo realizar el envío del formulario.

En este tutorial nos vamos a preocupar de entender cómo se pueden llevar a cabo cada uno de estos cuatro puntos utilizando la librería **Formik**.

## ¿Qué es **Formik**?

**Formik** es una pequeña librería que podemos incorporar dentro de nuestras aplicaciones de React que nos ayudará a trabajar con los formularios. Ahora bien, ¿por qué íbamos a querer trabajar con ella? La respuesta la obtenemos comprendiendo en qué aspectos nos va a ayudar durante el desarollo:

1. En primer lugar **Formik** se encargará de la gestión de todos los datos que el usuario introduzca en nuestros formularios guardando todo ellos en lo que se conoce como el estado del formulario (**form state**).
2. **Formik** nos ayudará de forma sencilla a controlar cuando se producirá el envío de los formularios de nuestras aplicaciones (**form submission**).
3. **Formik** nos ofrece un mecanismo sencillo para poder realizar la validación de los datos de los usuarios y mostrar los mensajes de error asociados derivados de los mismos.

Si nos paramos a pensar un poco podemos llegar a la conclusión de que **Formik** realmente no nos está ofreciendo nada nuevo a la hora de gestionar los formularios de nuestras aplicaciones ya que todos los puntos que hemos mencionado se pueden llevar a cabo directamente con React sin la utilización de una librería. Ahora bien, lo que sí que nos garantizará **Formik** es que, con su utilización, la gestión de los formularios se va a poder hacer de forma sencilla, eficiente y fácilmente escalable. En concreto lo que hará es abstraer todas aquellas partes más complejas de la gestión de los formularios dentro de las aplicaciones haciendo que nosotros únicamente nos tengamos que preocipar por lo que es el formulario en sí mismo (los campos que lo forman, la relación entre los mismos, etc.).

## Prerequisitos

Para sacar el máximo partido de este tutorial el lector debería estar familirizados con:

* HTML
* CSS
* JavaScript + ES6
* React (Hooks)

## Estructura del tutorial

Este tutorial trata de ser práctico en el sentido de que vamos a estudiar **Formik** gracias a la creación de un hipotético formulario dentro de una aplicación. Gracias al hook `useFormik` que nos proporciona la librería veremos cómo podemos relacionar nuestro formulario con **Formik**.

Hecho esto el siguiente paso será ver cómo podemos gestionar el estado del formulario (qué campos lo componen y qué valores tienen asociados), cómo gestionar el envío del formulario (**form submission**) y cómo podemos realizar las validaciones de los datos que nos son proporcionados.

Una vez tengamos lo anterior claro veremos cuáles son los componentes que **Formik** pone a nuestra disposición para poder evitar repeticiones en nuestro código y así lograr que nuestro código sea mucho más limpio.

Además nos preocuparemos por estudiar algunos aspectos adicionales que nos ofrece **Formik** y que hará que la gestión de nuestros formularios sea todavía más sencilla.

Veremos cómo podemos crear una serie de componentes para la recogida de los datos dentro de nuestras aplicaciones que podamos reutilizar una y otra vez y que estén vinculados con **Formik** con el fin de facilitarnos su gestión. Así crearemos componentes para mostrar `input`, `textarea`, `checkboxes`, `radio buttons` y `select dropdown`.

Una vez hecho todo lo anterior veremos cómo vamos a crear una de los formularios más comunes dentro de nuestras aplicaciones: el formulario de registro de una nueva cuenta en el sistema en el cual aplicaremos todos los conceptos que hemos ido estudiando a lo largo de este tutorial.

Ya para finalizar veremos cómo podemos vincular una librería de componentes visuales (como puede ser, por ejemplo, **material-ui**) para que los campos de nuestros formularios se muestren utilizando dicha librería de componentes y la gestión de los datos asociados sea llevada a cabo por **Formik**.

Al finalizar el estudio de este tutorial podemos estar segudos de que vamos a ser capaces de afrontar la gestión de cualquiera de los formularios que forman parte de nuestras aplicaciones (ya sean estos sencillo o complejos) de una forma eficiente, clara, sencilla y fácil de mantener.
