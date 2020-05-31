# Validaciones

En los siguiente puntos vamos a estudiar las diferentes maneras que tenemos de poder llevar a cabo la validación de los datos que los usuarios nos proporcionan dentro de los formularios gestionados por **Formik**.

Para realizar la explicación volveremos a apoyarnos en el formulario de ejemplo que hemos desarrollado hasta este punto (recordemos que ya sabemos cómo gestionar el estado del mismo además de trabajar con el evento `submit`) por lo que antes de seguir vamos a establecer las validaciones que se han de cumplir sobre estos tres campos:

1. En primer lugar estableceremos que los tres campos del formulario (`name`, `email` y `channel`) serán obligatorios (es decir, que deberán poseer un valor para que el formulario pueda ser enviado).
2. En el caso del campo `email` tendremos que asegurar que el valor que se proporciona sigue las reglas que definen una dirección de correo electrónico válida.
