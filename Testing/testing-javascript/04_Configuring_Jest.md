# Configure Jest for Testing JavaScript Applications.

- Introduction.
- Install and Run Jest.

# Introduction.

En este capítulo vamos a aprender a configurar y utilizar Jest para realizar nuestros test dentro de las aplicaciones JavaScript. A lo largo de los ejemplos que vamos a ir desarrollando a lo largo del capítulo nos vamos a apoyar en la definición de una aplicación de React que ha sido creada y configurada utilizando webpack (en otras palabras, que no ha sido creada utilizando `create-react-app` ni ninguna otra utilidad).

La aplicación una vez que arranca lo que hará será mostrar en el navegador de usuario una interfaz de una calculadora con la que podrá actuar cuyo aspecto será muy parecido al que se muestra en la siguiente imagen.

<div>
  <img src="./images/ch04/04_01.png">
</div>
<br/>

## Install & Run Jest.

Lo primero que tendremos que hacer para poder trabajar con Jest dentro de nuestros proyectos es instalar la libería como una dependencia de desarrollo lo cual lo haremos mediante la ejecución del siguiente comando desde la terminald el sistema:

```bash
$ npm install --save-dev jest
```

Cuando todo el proceso de instalación de las dependecias de desarrollo finalice en el fichero `package.json` del proyecto vamos a poder ver como se ha añadido el atributo la versión de la librería con la que estaremos trabajando (que en nuestro caso será la 24.9.0):

```json
"devDependencies": {
  "jest": "^24.9.0"
}
```

De hecho una vez finaliza la instalación de Jest si ahora nos vamos al directorio `node_modules` de nuestro proyecto y dentro del mismo a la directorio `bin` donde se recogerán todos aquellos comandos que van a poder ser invocados desde la línea de comandos gracias al uso de npm vemos que tenemos el archivo `jest`. ¿Qué significará esto? Pues simplemente que ahora dentro del archivo `package.json` vamos a poder añadir un nuevo script que sirva para ejecutar la librería de la siguiente manera:

```json
"script": {
  "test": "jest"
}
```

¿Qué quiere decir esto? Pues que ahora si abrimos una nueva terminal de nuestro sistema vamos a poder ejecutar jest sin más que hacer uso de una de las siguientes tres alternativas:

```bash
$ npm run test
$ npm test
$ npm t
```

Sin embargo ¿qué es lo que ocurre si ahora lo ejecutamos desde la terminal de comandos? Pues la respuesta la encontramos a continuación:

```bash
> testing@1.0.0 test
> jest

No tests found, exiting with code 1
[...]
```

donde podemos ver que Jest nos informa de que no ha encontrado ningún fichero que contenga al menos un test que haya de ser ejecutado. Ahora bien, ¿cómo podemos crear un test dentro de Jest? Uno de los parámetros que tiene configurados por defecto Jest es que cualquiera de los archivos que esté contenido en cualquiera de los directorios `__tests__` de nuestro proyecto va a poder ser considerado como un test (realmente como una suite de test ya que puede contener más de uno) y ejecutará el código que hay definido dentro del mismo y que esté establecido por defecto.

Por lo tanto vamos a crear el directorio `__tests__` dentro del directorio `src` de nuestro proyecto y dentro del mismo vamos a crear un nuevo fichero al que denominaremos `example.js` que contiene el siguiente código (en este momento no nos interesará mucho entenderlo sino comprobar que se ejecuta correctamente por lo que dejaremos su explicación para más adelante):

```js
test('it works', () => {})
```

Si ahora guardamos la información del fichero y volvemos a pedirle a Jest que ejecute nuestros test gracias a npm podemos ver cómo ahora sí que tenemos un único test, que dicho test es ejecutado y que este no lanza ningún error durante su ejecución (o dicho de otra manera, nuestro test pasa).

```bash
$ npm run test

> testing@1.0.0 test
> jest

 PASS  src/__tests__/example.js
  ✓ it works (2ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        3.361s
Ran all test suites
```

Por lo tanto la configuración de Jest para la ejecución de los test sobre nuestras aplicaciones es realmente sencilla siempre y cuando utilicemos las convenciones por defecto para detectar en qué lugar están situados los test que van a ser ejecutados. Acabamos de ver la primera de ellas que es situar los archivos que van a contener los test dentro de un directorio `__tests__` como hemos visto anteriormente, pero existe una segunda convención que consiste en que cualquier fichero de la aplicación que finalice con la extensión `.test.js` va a ser considerado por Jest como un fichero que va a contener una suite de test y por lo tanto lo va a ejecutar.

---
**Nota:** normalmente en los proyectos se utilizará una de las dos convenciones para utilizar Jest como framework para la ejecución de nuestros test por lo que una vez adoptado una de ellas se ha de seguir durante toda la vida del proyecto.

---

### CI/CD

Como parte del proyecto que hemos estado desarrollando a lo largo de los capítulos anteriores hemos visto que estamos utilizando Travis para hacer uso de las características que se nos ofrece de hacer Continuous Integration y Continuous Deployment. Además hemos visto que es gracias a las especificaciones que recogemos en el fichero `travis.yml` donde vamos a especificar qué comandos se han de ejecutar para lograr nuestro objetivo. Si ahora abrimos este fichero veremos que el atributo `script` se está recogiendo lo siguiente:

```js
script: npm run setup
```

es decir que el script que queremos que se ejecute como parte del proceso de CD/CI es el que está recogido como `setup` dentro de la sección `scripts` del fichero `package.json`. Ahora, si nos vamos al fichero `package.json` para ver qué es lo que se ejecuta en este script vemos lo siguiente:

```json
"scripts": {
  "setup": "npm install && npm run validate"
}
```
 
La ejecución de `npm install` servirá para poder instalar todas las dependencias del nuestro proyecto (es decir, todas las librerías que son necesarias para su ejecución) mientras que en la segunda parte del script lo que se está pidiendo es la ejecución del script `validate` que también está recogido dentro del fichero (gracias a la ejecución de `npm run`). ¿Cuál es el aspecto de este script? Lo vemos a continuación:

```json
"scripts": {
  "validate": "npm run lint && npm run build",
  "setup": "npm install && npm run validate"
}
```

es decir que se están pidiendo la ejecución del script `lint` que se encargará de hacer el análisis estático del código fuente de nuestro proyecto:

```json
"scripts": {
  "lint": "eslint --ignore-path .gitignore .",
  "validate": "npm run lint && npm run build",
  "setup": "npm install && npm run validate"
}
```

y por último el script denominado `build` en el que se está pidiendo la ejecución de webpack para construir el entregable de nuestra aplicación indicando que el entorno para el cual queremos construirlo es producción:

```json
"scripts": {
  "build": "webpack --mode=production",
  "lint": "eslint --ignore-path .gitignore .",
  "validate": "npm run lint && npm run build",
  "setup": "npm install && npm run validate"
}
```

Lo que nosotros queremos hacer es que en el caso de que se pase la fase de análisis del código estático de nuestra aplicación (la ejecución del script `lint`) lo que vamos a pedir es que también se ejecuten todos los test gracias a la ejecución del script `test` que hemos definido anteriormente antes de construcción del entregable. Así pues modificamos el script `validate` para que quede tal y como sigue:

```json
"scripts": {
  "test": "jest",
  "build": "webpack --mode=production",
  "lint": "eslint --ignore-path .gitignore .",
  "validate": "npm run lint && npm run test && npm run build",
  "setup": "npm install && npm run validate"
}
```

Así garantizamos que cuando se ejecute el script `validate` se realizará el análisis del código estático, se ejcutarán todos los test y, si no se ha producido ningún error, se construirá el entregable de nuestra aplicación cosa que es lo que pretendemos conseguir gracias a nuestro proceso de CD/CI.

