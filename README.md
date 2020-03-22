# REACT 

React.js es una biblioteca de JavaScript para construir interfaces de usuario. Se trabaja mediante componentes. Posee alta demanda debido a que es una librería muy ligera, posee un alto rendimiento y es mantenida constantemente. JSX es la sintaxis con la que se escribe código en React. Es una especie de fusión entre HTML y JS.



### Contenido

* **Sobre React**
  * Virtual DOM
  * Componentes
* **Create React App**
* **Conceptos básicos**
  * JSX
  * Crear componentes
  * Reglas de los componentes
  * Expresiones JSX
* **Flujo básico en React**
  * Renderizado en React
  * Renderizado de componentes
* **State**
  * **Hooks**
    * useState
    * useEffect
  * Compartir estados
* **Eventos**
  * onClick
  * onChange
* **Documentar Proyectos**
  * Prop Types



### Sobre React

**Virtual DOM**

**DOM**: Document object model, es la estructura con la que se representa un documento HTML. 

El virtual DOM es una copia exacta del DOM que se guarda en memoria, el usuario solo ve el DOM que es visto por el navegador, al existir un cambio en el DOM, el virtual DOM le pide al DOM que solo actualice lo necesario. Permite manipular los cambios para renderizar la parte que nos interesa.



**Componentes - ¿Que son?**

Los componentes son fragmentos de código que se pueden re-utilizar. En un sitio al trabajar con una grilla se repite un mismo diseño en varias ocasiones, por lo que es un lugar indicado para utilizar componentes. Esto permite re-utilizar un trozo de código en varios lugares de la aplicación mediante su nombre, modificando únicamente sus parámetros al indicar su nombre. Los componentes pueden estar compuestos de otros componentes.





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



### Conceptos Básicos en React

**JSX**

Es de una extensión de la sintaxis de JavaScript. Es recomendado utilizarlo con React para describir cómo debería ser la interfaz de usuario. 

**Ejemplo**:

```react
const element = <h1>Hello, world!</h1>;
```

React no requiere usar JSX, pero la mayoría de la gente lo encuentra útil como ayuda visual cuando trabajan con interfaz de usuario dentro del código JavaScript. Esto también permite que React muestre mensajes de error o advertencia más útiles. 

JSX produce elementos de React, los elementos son bloques de las aplicaciones en React, describen lo que se quiere ver por pantalla. 



**CREAR COMPONENTE**

La forma más simple de definir un componente es escribir una función de JavaScript. Los componentes aceptan un argumento denominado "**prop**".

Los componentes poseen la siguiente sintaxis:

```jsx
import React from 'react' // 1. Importamos React.

const Componente = () => { // 2. Generamos un función componente.
  return (  );
}
 
export default Componente; // 3. Exportamos la función componente.
```

> Con algunas extensiones de Visual Studio Code se puede generar rápidamente un componente:
>
> 1. `imr` - es un acortador que importa React en el archivo.
> 2. `sfc` - es un acortador que genera una función componente.



**REGLAS ACERCA DE LOS COMPONENTES**

1. **Componentes con padre único:** todos los componentes deben tener un único elemento padre, el resto de elementos pueden ir anidados en su interior. Podemos recurrir a la etiqueta `<Frament>` que viene disponible al importar react para envolver a todos los elementos en un único padre. 

2. **Etiquetas self-closing invalidas**: Las etiquetas ***self-closing*** deben ser cerradas en react.

3. **class y camelCase**: la propiedad class queda sustituida por className y a su vez todos los atributos especiales.

4. **No condicionales**: en JSX no podemos utilizar if, else o while.
5. **Uppercase File:** Los nombres de los archivos de un componente deben comenzar por mayúsculas.

*Para renderizar más de un componente debemos crear la ruta **"/src/components"**, crear componentes y luego importarlos.*



**AGREGAR EXPRESIONES JSX**

Como se menciono anteriormente los componentes al utilizar JSX no pueden albergar condiciones ni estructuras de control por lo que debemos recurrir a las expresiones JSX.

| Operación                                     | Expresión                                                    |
| --------------------------------------------- | ------------------------------------------------------------ |
| Devolver operaciones                          | `{ 1 + 1 } o { a + b }`                                      |
| Devolver variables, objetos, etc...           | `<p>Hola { user.name }</p>`                                  |
| Añadir condicionales con operadores ternarios | `<p> { user.premium ? "Bienvenido a tu cuenta premium" : null } </p>` |

> Las expresiones pueden ser incluidas en cualquier lugar dentro de un bloque JSX.
>





### Flujo básico en React

**RENDERIZADO EN REACT**

En React se renderizan los componentes mediante un vinculo entre un archivo html y un archivo JavaScript. El primero se encarga de disponer el DOM y el segundo se encarga del renderizado de React y sus componentes. Con Create React App opera de la siguiente forma:



En el archivo **index.js** cargamos nuestro componente **App** mediante el siguiente código:

```js
ReactDOM.render(<App />, document.getElementById('root'));
```

Con `ReactDOM.render(elemento, donde)`, renderizamos un componente en una ubicación especifica.

+ **DONDE**: por defecto en nuestro documento **index.html** posee un contenedor con un id denominado "***root***", este ultimo es manejado en el `ReactDom.render` mediante la función `document.getElementById("root")`. 

De esta manera conseguimos que un componente sea cargado en un archivo html.



**RENDERIZAR COMPONENTES EXTERNOS**

En el archivo index.js importamos un componente único denominado App.js, este se encarga de contener al resto de componentes que conformaran la lógica de la aplicación.



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



### STATE

Los estados **almacenan datos mutables provenientes de los componentes que se pueden importar y utilizar en la aplicación**. Los estados se pueden definir de varias formas, sin embargo en las ultimas actualizaciones de React se tienden a utilizar Hooks.



**HOOKS**

Se trata de funciones que permiten manipular el estado en React y su ciclo de vida mediante componentes funcionales (que son los que siempre vamos a utilizar). **Los Hooks más frecuentes son: useState, useEffect y useContext.**



**useState**: Permite manipular el estado en un componente al declarar este Hook. Mediante las constantes se puede obtener el valor del estado y actualizarlo en cualquier componente que se vincule.

* **Sintaxis**

  ```jsx
  const [state, setState] = useState(initialState)
  ```

  > `state` devuelve el valor del estado y `setState` permite actualizarlo.

Al principio `state` posee el mismo valor pasado como primer argumento, es decir, lo definido en `initialState`.

La función `setState` se utiliza para actualizar el estado. Acepta un nuevo valor de estado y lo renderiza.

```jsx
setState(newState)
```



**useEffect:** Este Hook permite indicarle al componente que debe ejecutar una tarea al renderizarse. Se utiliza para manejar el ciclo de vida del componente pues reemplaza a las siguientes funciones: ~~componentDidMount~~, ~~componentDidUpdate~~ y ~~componentWillUnmount~~. 

* **Sintaxis**

  ```jsx
  useEffect( () => {
      ...., []
  })
  ```

> **NOTA:** Todos los hooks deben ser importados.



Por ejemplo, a continuación se presenta un estado mediante Hooks que contienen una lista de objetos.

```react
import React, { Fragment, useState } from 'react';

function App() {
  const [ products, setProducts ] = useState([
    { id: 1, name: 'Camisa ReactJS', price: '10.000' },
    { id: 2, name: 'Camisa AngularJS', price: '10.000' },
    { id: 3, name: 'Camisa VueJS', price: '10.000' },
    { id: 4, name: 'Camisa JavaScript', price: '10.000' },
    { id: 5, name: 'Camisa Python', price: '10.000' }
  ])
```

> Para manipular los estados con Hooks utilizamos `useState`.



**COMPARTIR ESTADOS**

En las aplicaciones de React es común que se comparta un estado con varios componentes, en el siguiente ejemplo se procede a listar cada estado mediante un componente.



**Cargar estados a un componente**

Para cargar un estado debemos pasarlo como props al componente al que deseamos compartir los datos.

```react
import React, { Fragment, useState } from 'react';

function Component() {
  
  // 1. Se define el estado.
  const [ products, setProducts ] = useState([
    { id: 1, name: 'Camisa ReactJS', price: '10.000' },
    { id: 2, name: 'Camisa AngularJS', price: '10.000' }
  ])

  return (
    <Fragment>
      <h1>Lista</h1>
      
      <!--- 2. Cargamos el estado varias veces con map --->
      { products.map( product => (
        <!--- 3. Declaramos los props --->
        <Product key={product.id} product={product}/>
      ))}
          
    </Fragment>
  );
}

export default Component;
```

Declaramos un estado y luego lo cargamos varias veces mediante `map` a un componente. Para que el estado pueda ser manipulado por el componente externo debemos pasarlos al componente como props en su interior de la siguiente forma:

```jsx
<Component state={state} key={key} />
```



**Manipular estado desde otro componente**

Para manipular los estados manejamos los props obtenidos y los presentamos como se requiera.

```react
import React from 'react'

// 1. Extraemos los props mediante el uso de llaves {}.
const Product = ({ product }) => {
  // 2. En el caso de un objeto desestructuramos los datos.
  const {name, price, id} = product

  return ( 
    <div>
      <!--- 3. Presentamos los datos obtenidos --->
      <h3>{name}</h3>
      <p>{price}</p>
    </div>
   );
}
 
export default Product;
```

> En el componente externo simplemente presentamos la información.



**Cargar componente según valor del state**

Podemos definir si cargar o no un componente dependiendo del valor de su estado asociado.

```react
{cart.length === 0
  ? <p>No hay elementos</p>
  : 
  cart.map(product => (
    <Product
      key={product.id}
      product={product}
    />
  ))
}
```



### Eventos

En React se pueden programar varios tipos de eventos de forma distinta a como se desarrolla en JQuery y JavaScript.



**ONCLICK**

```jsx
      <button 
        type="button"
        onClick = { () => selectProduct(id) }
        Agregar al carro
      </button>
```

> Para trabajar el evento utilizamos la palabra clave `onClick` y le otorgamos como atributo el llamado a una función que ejecutara la tarea deseada.

**Ex. Función para agregar productos**

```react
  const selectProduct = id => {
    const product = products.filter(product => product.id === id)[0]
    setCart([...cart, product])
  }
```

> Al hacer clic actualizaremos el estado, sin embargo cada valor nuevo añadido reemplazara al anterior. Por lo que en el nuevo estado pasamos todo el contenido del estado anterior y el nuevo.

**Ex. Función para borrar producto**

```react
const removeProduct = id => {
  const products = cart.filter( product => product.id !== id )
  setCart(products)
}
```

> Mediante filter removemos todo elemento que coincida con el elemento a borrar.



**ONCHANGE**

El evento onChange nos permite ejecutar una tarea cuando se realiza un cambio en un elemento.



**Extraer contenido escrito por el usuario para definir el state**

```react
const Form = () => {
  /* Añadimos el state */
  const [ task, setTask ] = useState({
    user_name: '',
    task_name: '',
    date: '',
    hour: '',
    description: ''
  })

  /* Añadimos función para cuando se ingresen datos. */
  const handleChange = e => {
    setTask({
      ...task, [e.target.name] : e.target.value
    })
  }

...

	/* Mediante el atributo name de los inputs obtenemos su contenido en la función */
        <label htmlFor="">Usuario</label>
        <input 
          type="text"
          name="user_name"
          className="u-full-width"
          placeholder="Ingresa tu nombre de usuario"
          onChange={ handleChange } /* Utilizamos la función */
        />
```





### Documentar proyectos

**PROP TYPES**

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

**ACTUALIZAR ESTE APARTADO ...**