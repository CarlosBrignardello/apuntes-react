# REACT 

React.js es una biblioteca de JavaScript para construir interfaces de usuario, es una biblioteca de código. Se trabaja mediante componentes.

Posee alta demanda por que es una librería muy ligera, posee un alto rendimiento y es mantenida constantemente.

JSX es la sintaxis con la que se escribe código en React. Es una especie de fusión entre HTML y JS.

React pe, cada vez que se realizaba un cambio, este se identificaba y se cambiaba. React incorpora el Dom Virtual que incluye una copia del DOM. Al utilizar JSX ya identificamos las etiquetas HTML por lo que no se deben ir a buscar, al existir una diferencia entre las copias son modificados los componentes en especifico.



```react
<Layout>
    <Header />
    <Blogspot post={data} />
    <Footer />
</Layout>
```

> Esto es JSX, es una especie de HTML en JavaScript. En este caso Blogspot es un componente.



**Herramientas recomendadas**

* **React developer tools**

* **Prettier**



### Agregar React a un sitio web con JavaScript



**Añadir un contenedor en HTML**

En un documento HTML añadimos la etiqueta `<div>` para marcar el lugar donde se busca visualizar algo con React.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Add React in One Minute</title>
  </head>
  <body>

    <h2>Agregar React a un sitio web</h2>
    <p>Es posible utilizar React sin una herramienta de compilación.</p>
    <p>React se esta cargando como un script.</p>
      
    <!-- El componente de React será cargado al interior de esta etiqueta. -->
    <div id="like_button_container"></div>

  </body>
</html>
```

> En este caso agregamos un atributo id que permitirá manejarlo desde JavaScript más adelante y así visualizar un componente de React dentro del mismo.
>
> React reemplazara cualquier contenido existente dentro de los contenedores del DOM.



**Añadir React mediante Scripts**

A continuación integramos React al documento mediante el uso de scripts.

```html
  <!-- ... HTML anterior ... -->

  <!-- Cargar React. -->
  <!-- Nota: cuando se despliegue, reemplazar "development.js" con "production.min.js". -->
  <script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
  <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>

  <!-- Cargamos nuestro componente de React. -->
  <script src="like_button.js"></script>

</body>
```

> El ultimo script permite importar nuestro archivo HTML que generara el componente de React.



**Crear un componente en React**

creamos el archivo "like_button.js" con el siguiente contenido:

```js
'use strict';

class LikeButton extends React.Component {
  constructor(props) {
    super(props);
    this.state = { liked: false };
  }

  render() {
    if (this.state.liked) {
      return 'Le diste like.';
    }

    return React.createElement(
      'button',
      { onClick: () => this.setState({ liked: true }) },
      'Like'
    );
  }
}

const domContainer = document.querySelector('#like_button_container');
ReactDOM.render(React.createElement(LikeButton), domContainer);
```

> Este script ocasiona que al pulsar el botón cambie su mensaje.



### Crear una aplicación con React

El equipo de React principalmente recomienda las siguientes soluciones:

- Si estás **aprendiendo React** o creando una nueva aplicación de página única, usa [Create React App](https://es.reactjs.org/docs/create-a-new-react-app.html#create-react-app).
- Si estás construyendo un **sito web renderizado en servidor con Node.js,** prueba [Next.js](https://es.reactjs.org/docs/create-a-new-react-app.html#nextjs).
- Si estás construyendo un **sitio web orientado a contenido estático,** prueba [Gatsby](https://es.reactjs.org/docs/create-a-new-react-app.html#gatsby).



#### Create-react-app

Create React App es un ambiente cómodo para aprender React, es la mejor forma de comenzar a construir una nueva aplicación brindando una buena experiencia de desarrollo, y optimizando tu aplicación para producción.  

Se trata de una herramienta que desde la terminal genera un entorno de desarrollo para React.



**Instalar create-react-app:**

```bash
npm install -g create-react-app
```

Se recomienda conocer acerca de WebPack.

**Crear un proyecto**

```bash
create-react-app *NOMRE_PROYECTO*
```

**Iniciar el proyecto**

```bash
npm run start
```



**Create React App** no se encarga de la lógica de `backend` o de bases de datos; tan solo crea un flujo de construcción para `frontend`, de manera que lo puedes usar con cualquier `backend`. 



**Compactar JavaScript para producción**

* https://gist.github.com/gaearon/42a2ffa41b8319948f9be4076286e1f3



### Hola mundo

El ejemplo más básico de React:

```react
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

> Este muestra un encabezado con el texto “Hello, world!” en la página. 



### JSX

Es de una extensión de la sintaxis de Javascript. Es recomendado utilizarlo con React para describir cómo debería ser la interfaz de usuario. 

Ejemplo:

```react
const element = <h1>Hello, world!</h1>;
```



React no requiere usar JSX, pero la mayoría de la gente lo encuentra útil como ayuda visual cuando trabajan con interfaz de usuario dentro del código Javascript. Esto también permite que React muestre mensajes de error o advertencia más útiles. 



**Insertando expresiones en JSX**

En el ejemplo a continuación, declaramos una variable llamada `name` y luego la usamos dentro del JSX envolviéndola dentro de llaves:

```react
const name = 'Carlos Brignardello';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(element, document.getElementById('root')
);
```

Puedes poner cualquier expresión de JavaScript dentro de llaves en JSX. Por ejemplo, `2 + 2`, `user.firstName`, o `formatName(user)` son todas expresiones válidas de Javascript.



En el ejemplo a continuación, insertamos el resultado de llamar la función de JavaScript, `formatName(user)`, dentro de un elemento `h1`.

```react
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Carlos',
  lastName: 'Brignardello'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

> De esta forma podemos insertar el nombre completo formateado de un usuario.



Puedes usar JSX dentro de declaraciones `if` y bucles `for`, asignarlo a variables, aceptarlo como argumento, y retornarlo desde dentro de funciones:

```react
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```



**Especificando atributos con JSX**

Puedes utilizar comillas para especificar strings literales como atributos:

```react
const element = <div tabIndex="0"></div>;
```

También puedes usar llaves para insertar una expresión JavaScript en un atributo:

```react
const element = <img src={user.avatarUrl}></img>;
```

No pongas comillas rodeando llaves cuando insertes una expresión JavaScript en un atributo. Debes utilizar comillas (para los valores de los strings) o llaves (para las expresiones), pero no ambas en el mismo atributo.

> **Advertencia:**
>
> Dado que JSX es más cercano a JavaScript que a HTML, React DOM usa la convención de nomenclatura `camelCase` en vez de nombres de atributos HTML.
>
> Por ejemplo, `class` se vuelve [`className`](https://developer.mozilla.org/es/docs/Web/API/Element/className) en JSX, y `tabindex` se vuelve [`tabIndex`](https://developer.mozilla.org/es/docs/Web/API/HTMLElement/tabIndex).



**JSX previene ataques de inyección**

Es seguro insertar datos ingresados por el usuario en JSX:

```react
const title = response.potentiallyMaliciousInput;
// Esto es seguro:
const element = <h1>{title}</h1>;
```

Por defecto, React DOM [escapa](https://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html) cualquier valor insertado en JSX antes de renderizarlo. De este modo, se asegura de que nunca se pueda insertar nada que no esté explícitamente escrito en tú aplicación. Todo es convertido en un string antes de ser renderizado. Esto ayuda a prevenir vulnerabilidades [XSS (cross-site-scripting)](https://es.wikipedia.org/wiki/Cross-site_scripting).



**JSX representa objetos**

Babel compila JSX a llamadas de `React.createElement()`.

Estos dos ejemplos son idénticos:

```react
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

`React.createElement()` realiza algunas comprobaciones para ayudarte a escribir código libre de errores, pero, en esencia crea un objeto como este:

```reac
// Nota: Esta estructura está simplificada
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```



### Renderizando los elementos

Un elemento describe lo que quieres ver en la pantalla:

```react
const element = <h1>Hello, world</h1>;
```

A diferencia de los elementos del DOM de los navegadores, los elementos de React son objetos planos, y su creación es de bajo costo. React DOM se encarga de actualizar el DOM para igualar los elementos de React.



**Renderizando un elemento en el DOM**

Digamos que hay un `<div>` en alguna parte de tu archivo HTML:

```react
<div id="root"></div>
```

Lo llamamos un nodo “raíz” porque todo lo que esté dentro de él será manejado por React DOM.

Las aplicaciones construidas solamente con React usualmente tienen un único nodo raíz en el DOM. Dado el caso que estés integrando React en una aplicación existente, puedes tener tantos nodos raíz del DOM aislados como quieras.

Para renderizar un elemento de React en un nodo raíz del DOM, pasa ambos a `ReactDOM.render()`:

```react
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

>  Esto muestra “Hello, world” en la página. 



## Actualizando el elemento renderizado

Los elementos de React son [inmutables](https://es.wikipedia.org/wiki/Objeto_inmutable). Una vez creas un elemento, no puedes cambiar sus hijos o atributos. Un elemento es como un fotograma solitario en una película: este representa la interfaz de usuario en cierto punto en el tiempo.

Con nuestro conocimiento hasta este punto, la única manera de actualizar la interfaz de usuario es creando un nuevo elemento, y pasarlo a `ReactDOM.render()`.

Considera este ejemplo de un reloj en marcha:

```react
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```

Este llama a `ReactDOM.render()` cada segundo desde un callback del [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval).



**React solo actualiza lo que es necesario**

React DOM compara el elemento y su hijos con el elemento anterior, y solo aplica las actualizaciones del DOM que son necesarias para que el DOM esté en el estado deseado. Puedes verificar esto inspeccionando con las herramientas del navegador.

Aunque creamos un elemento que describe el árbol de la interfaz de usuario en su totalidad en cada instante, React DOM solo actualiza el texto del nodo cuyo contenido cambió.

En nuestra experiencia, pensar en cómo la interfaz de usuario debería verse en un momento dado y no en cómo cambiarla en el tiempo, elimina toda una clase de errores.



### Entorno de desarrollo de React

**src:** posee todo el código fuente de la aplicación.

**/src/index.js:** es el punto de entrada de la aplicación, dentro de este archivo se importa App.js que es renderizada.

**/src/App.js:**  posee código JSX, 

**package.json:** 

```json
{
  "name": "01-proyecto-prueba",
  "version": "0.1.0",
  "private": true,
  "dependencies": { // Donde se ubican las dependencias de React.
    "react": "^16.11.0",
    "react-dom": "^16.11.0",
    "react-scripts": "3.2.0"
  },
  "scripts": { // Comandos de desarrollo en React.
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": { // Herramienta para la detección de errores en el código.
    "extends": "react-app"
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}

```





### Clonar un proyecto externo de React



**Clonar proyecto**

```bash
git clone https://github.com/Sparragus/platzi-badges.git
```



**Instalar las dependencias del proyecto**

```bash
cd *CARPETA_GENERADA*
npm install
npm run start
```



### ReactDOM.render

Renderiza un elemento React al DOM en el `contenedor` suministrado y retorna una [referencia](https://es.reactjs.org/docs/more-about-refs.html) al componente (o devuelve `null` para [componentes sin estado](https://es.reactjs.org/docs/components-and-props.html#functional-and-class-components)).

Si el elemento React fue previamente renderizado al `contenedor`, esto ejecutará una actualización en él, y solo mutará el DOM de ser necesario para reflejar el más reciente elemento React.



**Añadir contenido mediante JS**

```js
const name = "Carlos"
const element = document.createElement('h1') // Creamos un elemento
element.innerText = `Hola soy ${name}` // Asignamos un contenido
const container = document.getElementById('app') // Vinculamos mediante ID

container.appendChild(element) // Añadimos el elemento creado
```

> /src/index.js
>
> De esta forma añadimos contendido al sitio (public/index.html) mediante JavaScript.



**Añadir contenido mediante React.createElement()**

```react
import React from 'react';
import ReactDOM from 'react-dom';

const name = 'Carlos';
const element = React.createElement('h1', { }, `Hola soy ${name}`) // Posee tres argumentos | Elemento, Atributos, Contenido
const container = document.getElementById('app');

ReactDOM.render(element, container);
```



**Añadir contenido mediante React & JSX**

```react
import React from 'react'; // Siempre que usemos JSX debemos importar la libreria de react.
import ReactDOM from 'react-dom';

const name = "Carlos"
const element = <h1>Hola soy {name}</h1>
const container = document.getElementById('app');

ReactDOM.render(element, container); // Tomamos dos argumentos, que renderizamos y donde.
```

> Es recomendado utilizar JSX debido a que permite generar muchos más elementos HTML de forma más simple y ordenada, en contra posición con utilizar `React.createElement()` que es menos escalable.