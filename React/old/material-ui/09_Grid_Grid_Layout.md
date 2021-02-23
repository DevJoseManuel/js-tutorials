# `Grid`

En este punto del tutorial vamos a estudiar uno de los aspectos más importantes y que es preciso conocer con muy bien a la hora de trabajar con **Material UI** que no es otro que el componente `Grid`. Este es un tema complejo de entender si el lector no ha trabajado previamente con un sistema de Grid (Grid Layout) pero vamos a intentar irlo haciendo poco a poco hasta conseguir dominarlo.



```javascript
import React from 'react'
import { Grid } from '@material-ui/core'
import { makeStyles } from '@material-ui/core/styles'

const useStyles = makeStyles({
  blueItem: {
    backgroundColor: 'blue',
    height: '200px'
  },
  redItem: {
    backgroundColor: 'red'
  },
  greenItem: {
    backgroundColor: 'green'
  }
})

export default function App() {
  const classes = useStyles() 
  return (
    <Grid container>
      <Grid item xs={ 4 } className={ classes.blueItem } />
      <Grid item xs={ 4 } className={ classes.redItem } />
      <Grid item xs={ 4 } className={ classes.greenItem } />
    </Grid>
  )
}
```





## Enlaces de interés

* [Grid en Material UI](https://material-ui.com/components/grid/#grid).
* [API de `Grid`](https://material-ui.com/api/grid/).

