# Cómo centrar elementos con `Grid`

En este punto del tutorial vamos a aprender a centrar elementos dentro de nuestra interfaz de usuario utilizando el componente `Grid` de **Material UI** ya que es uno de los problemas más comunes a los que nos podemos enfrentar cuando estamos diseñando nuestras interfaces de usuario.

Antes de nada lo primero que tenemos que saber es que el componente `Grid` posee un total de tres props que nos van a ayudar a centrar su contenido: `alignContent`, `alignItems` y `justify`. Es más [si vamos a la documentación](https://material-ui.com/api/grid/#props) del componente podemos ver que los valores que admiten cada una de estas props sin similares a los valores que estaríamos utilizando en el caso de que utilizar Flexbox para maquetar nuestra aplicación y la razón no es otra que **Material UI** utiliza internamente Flexbox para lograr el posicionamiento de los componentes.

---
**Nota:** para todos aquellos lectores que puedan estar más interesados en saber cómo se utiliza Flexbox les recomendamos [leer la sección en MDN](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Flexbox) donde se explica detalladamente.

---

## `direction`

Lo primero que tenemos que entender para saber cómo centrar los componentes de nuestras aplicaciones es cómo funciona la prop `direction` la cual no será nueva para todos aquellos lectores que ya conozcan Flexbox. Si partimos de un componente `Grid` que hace la funcionalidad de un contenedor de otros grid (lo que hacemos estableciendo la prop `container`) y creamos un disposición como la siguiente:

```javascript
<Grid container>
  <Grid item><Paper>Cell 1</Paper></Grid>
  <Grid item><Paper>Cell 2</Paper></Grid>
  <Grid item><Paper>Cell 3</Paper></Grid>
</Grid>
```

Lo que provocará cada uno de los posibles valores que le podemos asignar a esta prop es cambiar la disposición y el orden de los componentes en el renderizado. En la siguiente tabla recogemos estos valores:

|direction|descripción|
|----|---|
|`row`| los distintos items se muestran en horizontal (fila) y de izquierda a derecha, es decir, en el mismo orden que son recogidos en el marcado JSX. 
|`row-reverse`| los items se muestran también en horizontal (fila) pero de derecha a izquierda lo que quiere decir que el primer elemento en el marcado JSX aparecerá más pegado a la derecha, el siguiente a continuación y el último pegado a la izquieda.
|`column`|los items se muestran en vertical (columna) y de arriba hacia abajo siguiendo el mismo orden recogido en el marcado JSX lo que nos viene a decir que el primer elemento del marcado estará en la parte superior de la columna, el segundo a continuación y el último en la base de la columna.
|`column-reverse`| similar a `row-reverse` pero aplicado a las columnas. En este caso los items se muestran en columna pero en el orden contrario al que son recogidos en el marcado JSX. Así, el último dentro del marcado será el que estará en la parte superior de la columna, el penúltimo a continuación, etc. En la base de la columna estará el primero de los items dentro del marcado.

Si no especificamos nada a la hora de definir nuestro `Grid` ¿qué valor tendrá la prop `direction`? La respuesta es `row`.

---
**Nota:** se recomienda probar los ejemplos que están recogidos en [propia web de Material UI](https://material-ui.com/components/grid/#interactive) donde se puede ver de forma gráfica como funciona esta prop.

---

## `justify`

Esta es la primera de las props que tenemos a nuestra disposición para poder alinear el contenido dentro de un grid. Los posibles valores que podemos asignar a esta prop serán los que recogemos en la siguiente tabla:

|justify|descripción|
|---|---|
|`flex-start`| La disposición de los elementos dentro del grid se siturará lo más próxima a la izquierda posible siempre respetando el orden que se ha definido en la prop `direction`. ¿Qué quiere esto decir? Pues básicamente que un grid ocupará todo el espacio horizontal que tiene a su disposición dentro de la página pero es posible que los items que lo forman no precisen de todo este espaciado. Al establecer este valor lo que haremos será indicar que los elementos se han de situar lo más próximos al borde izquierdo del grid como sea posible.
|`flex-end`| La disposición de los elementos dentro del grid se situará lo más próxima a la derecha como sea posible siempre conservando el orden que se ha especificado en la prop `direction`.
|`center`| La disposición de los elementos se situará en el centro lo que nos va **permitir lograr nuestro objetivo** de centrar los items.
|`space-between`| En este caso la disposición de los elementos dentro del grid se realiza situando el primero de ellos (siguiendo el orden establecido en la prop `direction`) lo más próximo a la izquierda posible, el último lo más próximo a la derecha y el resto de ellos estarán situados en el espacio que hay disponible en entre los extremos haciendo que el espaciado sea el mismo en cualquiera de las dos direcciones (dicho de otra manera, quedará centrado con respecto a los items que se han ido a los extremos).
|`space-around`| en este caso lo que se hace es dividir el espacio disponible en el grid entre el número de elementos que lo forman creando una especie de celdas del mismo tamaño y se situarán cada uno de los items en una celda respetando el orden establecido en la prop `direction`.
|`space-evenly`| es similar al anterior pero lo que logrará es que el espaciado que existe entre el item que se está en el centro y los items que están en los extremos sean iguales añadiendo un padding por la izquierda del primer item y un padding por la derecha al último elemento.

En resumen podemos decir que la prop `justify` sirve para determinar cómo se hará la alineación de los elementos siguiendo el eje X (horizontal) de los item que formarán el grid. Además, como el valor por defecto para esta prop es `flex-start` será necesario establecer asignarle valor `center` para lograr nuestro objetivo de centrado de los elementos de un grid.

---
**Nota:** nuevamente se recomienda probar los ejemplos que están recogidos en [propia web de Material UI](https://material-ui.com/components/grid/#interactive) donde se puede ver de forma gráfica como funciona esta prop.

---


## `alignContent`

Esta prop nos puede llevar a confusión con ya que por el nombre puede parecer que se utilizará para alinear el contenido de un grid. Sin embargo no es así, sino que nos va a servir para poder definir el espaciado que existirá entre cada una de las filas que forman un grid.

COMPLETAR LA EXPLICACION DE ARRIBA!!!!











Como en otras ocasiones vamos a comenzar mostrando un pequeño ejemplo sobre el cual iremos desarrollando la explicación:

```javascript
import React from 'react'
import { Grid } from '@material-ui/core/'
import AcUnitIcon from '@material-ui/icons/AcUnit'

export default function App() {
  return (
    <Grid container>
      <Grid item>
        <AcUnitIcon color='primary' />
      </Grid>
      <Grid item>
        <AcUnitIcon color='secondary' />
      </Grid>
    </Grid>
  )
}
```
