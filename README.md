# REACT 

React.js es una biblioteca de JavaScript para construir interfaces de usuario, es una biblioteca de código. Se trabaja mediante componentes.

Posee alta demanda por que es una librería muy ligera, posee un alto rendimiento y es mantenida constantemente.

JSX es la sintaxis con la que se escribe código en React. Es una especie de fusión entre HTML y JS.



**Virtual DOM**

**DOM**: Document object model, es la estructura con la que se representa un documento HTML. 

El virtual DOM es una copia exacta del DOM que se guarda en memoria, el usuario solo ve el DOM que es visto por el navegador, al existir un cambio en el DOM, el virtual DOM le pide al DOM que solo actualice lo necesario. Permite manipular los cambios para renderizar la parte que nos interesa.



```react
<Layout>
    <Header />
    <Blogspot post={data} />
    <Footer />
</Layout>
```

> Esto es JSX, es una especie de HTML en JavaScript.



**Componentes - ¿Que son?**

Los componentes son fragmentos de código que se pueden re-utilizar.

En un sitio al trabajar con una grilla se repite un mismo diseño en varias ocasiones, por lo que es un lugar indicado para utilizar componentes. Esto permite re-utilizar un trozo de código en varios lugares de la aplicación mediante su nombre, modificando únicamente sus parámetros al indicar su nombre.

Los componentes pueden estar compuestos de otros componentes.



### Create-react-app

Create React App es un ambiente cómodo para aprender React, es la mejor forma de comenzar a construir una nueva aplicación brindando una buena experiencia de desarrollo, y optimizando tu aplicación para producción.  

Se trata de una herramienta que desde la terminal genera un entorno de desarrollo para React.

Es necesario instalar **Node Js**:

* https://nodejs.org/es/

Al instalar Node podemos utilizar el administrador de paquetes de node (npm), permite administrar los paquetes que utilicemos en los proyectos JavaScript.



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
cd /*NOMBRE_PROYECTO*
npm run start
```



**Create React App** no se encarga de la lógica de `backend` o de bases de datos; tan solo crea un flujo de construcción para `frontend`, de manera que lo puedes usar con cualquier `backend`. 

De esta forma se genera todo de forma automática, incluso la inicialización de git y un commit. Este comando nos crea un componente denominado App.js, archivo CSS para el componente y un archivo index.js que renderiza el componente App. 

En el proyecto nos encontramos con un archivo denominado **package.json** que se trata de un objeto JavaScript, posee toda la configuración del proyecto, se puede modificar el nombre del proyecto, están declaradas en el las dependencias y librerías del proyecto, en este caso existen (react, react-dom, react-scripts). Poseemos también la configuración de los comandos del proyecto. Los archivos .test permiten construir y ejecutar pruebas.





### JSX

Es de una extensión de la sintaxis de JavaScript. Es recomendado utilizarlo con React para describir cómo debería ser la interfaz de usuario. 

**Ejemplo**:

```react
const element = <h1>Hello, world!</h1>;
```



React no requiere usar JSX, pero la mayoría de la gente lo encuentra útil como ayuda visual cuando trabajan con interfaz de usuario dentro del código JavaScript. Esto también permite que React muestre mensajes de error o advertencia más útiles. 

JSX produce elementos de React, los elementos son bloques de las aplicaciones en React, describen lo que se quiere ver por pantalla. 



**Renderizado en React**

En el archivo index.js cargamos nuestro componente App mediante el siguiente código:

```js
ReactDOM.render(<App />, document.getElementById('root'));
```

Con `ReactDOM.render(elemento, donde)`, renderizamos un componente en una ubicación especifica.

+ **DONDE**: por defecto en nuestro documento **index.html** posee un contenedor con un id denominado "***root***", este ultimo es manejado en el `ReactDom.render` mediante la función `document.getElementById("root")`. 

De esta manera conseguimos que un componente sea cargado en un archivo html.



**Renderizar componente externo desde index.js**

> **App.js**
>
> Generamos el componente, en este caso el componente simplemente devuelve un h1.

```jsx
import React from 'react';

const App = () => <h1>h1 desde App.js</h1>

export default App;
```



> **index.js**
>
> Importamos App.js y lo declaramos como elemento a renderizar.

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(<App />, document.getElementById("root"));
```



**Crear componente**

Como ejemplo generamos el siguiente componente basado en un navbar: 

```jsx
import React from 'react';
import {BrowserRouter, Link} from 'react-router-dom'

const App = () => (
  <BrowserRouter>
    <header className="navbar">
      <h1>LOGO</h1>
      <nav>
        <ul>
          <li> <Link to="#">Pagina uno</Link> </li>
          <li> <Link to="#">Pagina dos</Link> </li>
          <li> <Link to="#">Pagina tres</Link> </li>
        </ul>
      </nav>
    </header>
  </BrowserRouter>
)

export default App;
```

**Componentes con padre único:** todos los componentes deben tener un único elemento padre, el resto de elementos pueden ir anidados en su interior. Podemos recurrir a la etiqueta `<Frament>` que viene disponible al importar react para envolver a todos los elementos en un único padre. 

**Etiquetas self-closing invalidas**: Las etiquetas ***self-closing*** deben ser cerradas en react.

**class y camelCase**: la propiedad class queda sustituida por className y a su vez todos los atributos especiales.

**No condicionales**: en JSX no podemos utilizar if, else o while.

Para renderizar más de un componente debemos crear la ruta **"/src/components"**, crear componentes y luego importarlos.



**Agregar expresiones JavaScript en JSX**

Las expresiones deben ser incluidas de la siguiente forma en JSX:

1. `{ 1 + 1 }`

2. ```jsx
   const user= { "name": "Carlos Brignardello", "user": "cbrigcode" }
   /*
   const Card = () => (
       <div className="card">
       	*/
           <p>Hola { user.name }</p>
   		/*
   ```

3. ```jsx
   const user = { "name": "Carlos Brignardello", "user": "cbrigcode", "premium": true }
   /*
   const Card = () => (
       <div className="card">
       */
           <p> { user.premium ? "Bienvenido a tu cuenta premium" : null } </p>
   ```

   De esta forma utilizamos condicionales en JSX. Los String pueden poseer etiquetas en su interior. Las expresiones pueden ser usadas para completar atributos html.



En el ejemplo a continuación, declaramos una variable llamada `name` y luego la usamos dentro del JSX envolviéndola dentro de llaves:

```react
const name = 'Carlos Brignardello';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(element, document.getElementById('root')
);
```



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



**Nota**: No utilices comillas rodeando llaves, las "comillas" son para valores de atributos con strings, las {llaves} son para expresiones JavaScript.



**React solo actualiza lo que es necesario**

React DOM compara el elemento y su hijos con el elemento anterior, y solo aplica las actualizaciones del DOM que son necesarias para que el DOM esté en el estado deseado. Puedes verificar esto inspeccionando con las herramientas del navegador.

Aunque creamos un elemento que describe el árbol de la interfaz de usuario en su totalidad en cada instante, React DOM solo actualiza el texto del nodo cuyo contenido cambió.

En nuestra experiencia, pensar en cómo la interfaz de usuario debería verse en un momento dado y no en cómo cambiarla en el tiempo, elimina toda una clase de errores.



### Props

Las propiedades nos sirven básicamente para cargar de datos a los componentes, de modo que éstos puedan personalizar su comportamiento, o para transferir datos de unos componentes a otros para producirse la interoperabilidad entre componentes.



Podemos utilizarlo para otorgar valores a un componente al momento de declararlo de la siguiente forma:

1. ```jsx
   const Card = props => (
       <div className="card">
           <p>Hola { user.name }</p>
           <p> { user.premium ? "Bienvenido a tu cuenta premium" : null } </p>
           <img src={props.url} alt=""/>
           <h2>Este es {props.name}.</h2>
       </div>
   )
   ```

   Debemos entregar como atributo "props" al componente para dentro del mismo poder utilizar la notación "**props.PROPIEDAD**" que definira el valor asignado al ser declarado.

2. ```jsx
       <Card 
         name="Mojito" url="https://cdn140.picsart.com/295235072129211.png?r1024x1024"
       />
       <Card 
           name="Tequila"
           url="https://i.pinimg.com/564x/82/1c/8c/821c8c26d29feb57f3feae5171123901.jpg"
       />
       <Card />
   ```

   En este caso en el componente App.js fue declaro el componente y se le asignaron las propiedades "name", "url".

Si revisamos desde la consola de React podremos ver esas propiedades.

Al declarar el objeto **props** en un componente se permite la lectura de las propiedades al crear los componentes.



También es posible en el componente utilizar props al denotar únicamente las propiedades que se desean trabajar:

```jsx
const Card = ({ url, name }) => (
    <div className="card">
        <p>Hola { user.name }</p>
        <p> { user.premium ? "Bienvenido a tu cuenta premium" : null } </p>
        <img src={url} alt=""/>
        <h2>Este es {name}.</h2>
    </div>
)
```



### Tipos de componente



**Componente funcional o presentacional**

Su única función es presentarse en la interfaz, no posee lógica, no puede presentar eventos o cambiar por si solo, solo recibe propiedades y las procesa.

Su sintaxis es la siguiente:

```jsx
import React from 'react'

const Component = props => (
    <div>
    	ESTRUCTURA JSX
    </div>
)

export default Component
```

1. Debemos **importar react.**
2. Utilizamos una constante y le damos un **nombre basado en el nombre del componente** que será igual a la función que agregaremos que en este caso posee los **props como argumento**. En su interior retornaremos una **estructura JSX**, es por eso que utilizamos "**()**" en el arrowFunction.
3. Exportamos el componente.



**Utilizar Ternarios para undefined**

En caso de que un componente no posea una imagen o un contenido al ser definido podemos definir un dato predeterminado:

```jsx
const Card = ({ url, name }) => (
    <div className="card">
        <p>Hola { user.name }</p>
        <p> { user.premium ? "Bienvenido a tu cuenta premium" : null } </p>
        {
            url ? <img src={url} alt=""/>
            : <img src="https://www.tuotrodiario.com/imagenes/en-la-red/2018060174867/conoce-luhu-gatito-mas-triste-del-mundo/0-245-54/gatito3-z.jpg" alt=""/>
        }
        <h2>
        {name ? name  
        : <h2>No se pudo encontrar el gatito.</h2>}
        </h2>
    </div>
)
```

Al no enviar datos en el prop se mostrara una imagen y un texto por defecto.



**Librería prop-types**

Permite definir tipos de propiedades, entre ellas propiedades por defecto.

**Instalar**: npm install prop-types

Para hacer esta tarea simplemente importamos la librería en el componente en el que la buscamos aplicar y declaramos lo siguiente antes de exportar el componente:

```jsx
import PropTypes from 'prop-types'

/* COMPONENTE */

Card.propTypes = {
    url: PropTypes.string,
    name: PropTypes.string
}

Card.defaultProps = {
    url: "https://www.tuotrodiario.com/imagenes/en-la-red/2018060174867/conoce-luhu-gatito-mas-triste-del-mundo/0-245-54/gatito3-z.jpg",
    name: "No se pudo encontrar el gatito."
}
```

De esta forma declaramos el tipo de dato esperado y además un comportamiento por defecto.



**Recorrer Arrays de componentes**

Definimos en App.js una lista de objetos:

```jsx
const gatitos = [
  {"name": "Mojito", "url": "https://cdn140.picsart.com/295235072129211.png?r1024x1024"},
  {"name": "Tequila", "url": "https://i.pinimg.com/564x/82/1c/8c/821c8c26d29feb57f3feae5171123901.jpg"},
  {"name": "Gustavo", "url": "https://cdn140.picsart.com/286770672011211.png?r1024x1024"},
  {"name": "Snowball", "url": "https://i.pinimg.com/originals/17/0a/81/170a815040a32fcc2f596c59c9284c15.jpg"},
  {"name": "Jaskier", "url": "https://i.pinimg.com/originals/68/6c/25/686c25b3885ec377547d2b73c2c51eb0.jpg"},
]
```



Para recorrer el componente varias veces hacemos lo siguiente:

```jsx
const App = () => (
  <div>
    <Navbar />
    {
      gatitos.map( gato => 
        <Card 
          name={gato.name} 
          url={gato.url}
        />
      )
    }
  </div>
)
```

