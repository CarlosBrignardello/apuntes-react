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

```jsx
const element = <h1>Hello, world!</h1>;
```

React no requiere usar JSX, pero la mayoría de la gente lo encuentra útil como ayuda visual cuando trabajan con interfaz de usuario dentro del código JavaScript. Esto también permite que React muestre mensajes de error o advertencia más útiles. 

JSX produce elementos de React, los elementos son bloques de las aplicaciones en React, describen lo que se quiere ver por pantalla. 



**Renderizar React**

```jsx
import React from 'react';
import ReactDOM from 'react-dom'
import Componente from './Componente';
import './index.css'

const divRoot = document.querySelector('#root')
ReactDOM.render(<Componente />, divRoot)
```

> Para trabajar con propiedades JSX debemos importar React. Para renderizar todos los elementos de React existe una función superior denominada `ReactDOM.render()`. En la cual se indican específicamente el componente a cargar y donde.



**Componente funcional base**

```jsx
import React from 'react';

const Componente = () => {
  return ( 
  	<h1>Componente!</h1>
  );
}
 
export default Componente;
```

> Con algunas extensiones de Visual Studio Code se puede generar rápidamente un componente:
>
> 1. `imr` - es un acortador que importa React en el archivo.
> 2. `sfc` - es un acortador que genera una función componente.



**Fragment**

Para renderizar más de un elemento dentro de un componente debemos anidarlos en una etiqueta `div`. El problema con esto es que esto genera un elemento adicional a nuestro documento, ocupando espacio innecesario. Para solucionar esto existe `Fragment` que se trata de un componente más que permite la inclusión de más de un elemento al interior de un componente.

```jsx
import React, { Fragment } from 'react';

const Componente = () => {
  return (
  <Fragment>
    <h1>Componente!</h1>
    <p>Soy un parrafo</p>
  </Fragment>
  
  )}
 
export default Componente;
```

> Como alternativa podemos reemplazar `Fragment` por `<> CONTENIDO </>`.



**Importar estilos**

```jsx
// En un componente sea cual sea
import './index.css'
```



**Imprimir variables en un componente**

Para devolver variables utilizamos expresiones JSX de la siguiente forma:

```jsx
import React, { Fragment } from 'react';

const Componente = () => {

  const name = 'Carlos Brignardello'
  return (
  <Fragment>
    <p>Este componente fue realizado por:</p>
    <h3>{name}</h3>
  </Fragment>
  
  )}
 
export default Componente;
```



**Imprimir una lista**

```jsx
import React, { Fragment } from 'react';

const Componente = () => {

  const pares = [2, 4, 6, 8, 10]
  return (
  <Fragment>
    <p>Lista de números pares</p>
    <h3>{pares}</h3>
  </Fragment>
  
  )}
 
export default Componente;
```

> JSX puede imprimir un Array, más no elementos booleanos o un objeto.



**Imprimir objeto convertido a String**

```jsx
import React, { Fragment } from 'react';

const Componente = () => {

  const objeto = {
    name: 'Carlos',
    edad: 21
  }

  return (
  <Fragment>
    <p>Lista de números pares</p>
    <h3>{ JSON.stringify( objeto, null, 3 ) }</h3>
  </Fragment>
  
  )}
 
export default Componente;
```

> `null, 3` formatea el estilo de JSON.  



### Comunicación entre componentes

Para mantener un comunicación entre componente se utilizan los `props` . 



**Pasar información del componente padre a un componente hijo**

```jsx
// Componente padre
ReactDOM.render(<Componente name="Carlos Brignardello"/>, divRoot)


// Componente hijo
const Componente = ( props ) => {

  return (
  <Fragment>
    <p>Hola mi nombre es</p>
    <h3>{ props.name }</h3>
  </Fragment>
  
  )}
 
export default Componente;
```

> Un componente padre puede enviar información a un componente hijo y un componente hijo puede leer esa información mediante los `props`.
>



**Pasar información del hijo al padre**

Para definir datos desde un componente hijo a un padre la forma más básica es enviando el `setState` de un hook como prop al componente hijo para que desde el hijo se modifique el valor del estado del componente padre. Sin embargo si se desean conservar los datos anteriores en el cambio del estado aún así es posible hacerlo sin pasar esos datos como prop.

```jsx
// Componente padre
<AddCategory setCategories={setCategories} />

// Componente hijo
const AddCategory = ({ setCategories }) => {

  const [ inputValue, setInputValue ] = useState('Ingresa una categoria')

  const handleInputChange = e => {
    setInputValue( e.target.value )
  }

  const handleSubmit = e => {
    e.preventDefault()
    setCategories( categ => [ ...categ, inputValue ] )
  }

  return ( 
    <>
    <form onSubmit={ handleSubmit }>
      <input 
        type="text"
        value={inputValue}
        onChange={ handleInputChange }
      />
    </form>
    </>
   );
}
 
export default AddCategory;
```

> En el ejemplo mediante un input definimos el valor de la nueva categoría y en cuanto hacemos un **submit** se actualiza el valor del componente en el padre.
>
> `setCategories( categ => [ ...categ, inputValue ] )` de esta forma podemos preservar el valor original y adicionar uno nuevo.





**Leer props con desestructuración de objetos**

```jsx
const Componente = ({ name }) => {

  return (
  <Fragment>
    <p>Hola mi nombre es</p>
    <h3>{ name }</h3>
  </Fragment>
  
  )}
 
export default Componente;
```



**PropTypes**

En ocasiones se dan casos en que un componente requiere si o si que se le envié información de parte del componente padre, una de las alternativas que existen en estos casos es definir valores por defecto en un componente mediante JS. Parar eso existe `propTypes`.

Para validar una función de Hook utilizamos lo siguiente: `setCategories: PropTypes.func.isRequired`

```jsx
import React, { Fragment } from 'react';
import PropTypes from 'prop-types'

const Componente = ({ name }) => {

  return (
  <Fragment>
    <p>Hola mi nombre es</p>
    <h3>{ name }</h3>
  </Fragment>
  
  )}

  Componente.propTypes = {
    name: PropTypes.string.isRequired
  }
 
export default Componente;
```

> Tras importar el modulo, definimos que el `prop` **name** reciba solo valores de tipo string en forma obligatoria.
>
> Existe un Snippet que genera todo este código: `rafcp`



**DefaultProps**

Para definir valores por defecto en las propiedades hacemos lo siguiente:

```jsx
  Componente.defaultProps = {
    optional: 'Prefiero no decirlo.'
  }
```

> Lo definimos antes de exportar el componente.





### EVENTOS

React posee sus propios eventos, por lo que estos reemplazan a los nativos de JS.



**onClick**

```jsx
// Alternativa # 1  
  const handleAdd = () => {
    console.log('Button has been Press')
  }

  return ( 
    <Fragment>
      <h1>Conteo</h1>
      <button onClick={ handleAdd }>+1</button>
    </Fragment>
   );

// Alternativa # 2

  return ( 
    <Fragment>
      <h1>Conteo</h1>
      <button onClick={ () => console.log('Button has been Press') }>+1</button>
    </Fragment>
   );
```



**onChange**

```js
  const [ inputValue, setInputValue ] = useState('Ingresa una categoria')

  const handleInputChange = e => {
    setInputValue( e.target.value )
  }

  return ( 
    <>
      <input 
        type="text"
        value={inputValue}
        onChange={ handleInputChange }
      />
    </>
   );
}
```

> En el ejemplo definimos el valor del input mediante el estado inicial del Hook. Posteriormente al modificar este valor disparamos el evento que actualiza el estado del Hook según las interacciones del usuario. Con `e.target.value` accedemos al valor del `<input>`.



**onSubmit**

```jsx
  const handleSubmit = e => {
    e.preventDefault()
    console.log('Submit realizado')
  }

  return ( 
    <>
    <form onSubmit={ handleSubmit }>
      <input 
        name="name"
        type="text"
        value={inputValue}
        onChange={ handleInputChange }
      />
    </form>
    </>
   );
```

```js
  const handleInputChange = ({ target }) => {
    setformState({
      ...formState,
      [ target.name ]: target.value
    })
  }
```

> De esta forma podemos actualizar el valor de un objeto en base al valor de un `input`.
>





### El estado

Los estados **almacenan datos mutables provenientes de los componentes que se pueden importar y utilizar en la aplicación**. Los estados se pueden definir de varias formas, sin embargo en las ultimas actualizaciones de React se tiende a utilizar Hooks.



**HOOKS**

Se trata de funciones que permiten manipular el estado en React y su ciclo de vida mediante componentes funcionales. **Los Hooks más frecuentes son: useState, useEffect y useContext.**



**useState**

`useState` contiene dos valores, el valor del estado y una función para modificarlo. Al definirlo es posible definir un estado inicial. En este caso 0.

```jsx
// Hook
const [counter, setCounter] = useState( 0 )

// Función disparada por el evento
  const handleAdd = () => {
    setCounter( counter + 1 )
  }

  return ( 
    <Fragment>
      <h1>CounterApp</h1>
      <h2>{ counter }</h2>
      <button onClick={ handleAdd }>+1</button>
    </Fragment>
   );
```

> React no permite que se modifique directamente el valor del estado, siempre se deberá hacer mediante la función del `setState`. 
>
> Por ejemplo utilizar funciones de Arrays como `push` u otros no permitirán que él que estado cambie.
>
> Lo que hace este componente es incrementar el valor del estado **counter** mediante un evento clic que dispara la función que incrementa el hook.



**Modificar el estado en un Hook**

Una recomendación para cambiar el estado de un Hook es la siguiente

```js
  const handleAdd = () => {
    const valorUser = prompt('Ingresa un valor:')
    setCategories( [...categories, valorUser] )
    
    //Ingresar nuevo valor primero
    setCategories( [valorUser, ...categories ] )
      
    // Alternativa
    setCategories( cats => [ ...cats, valorUser ] )
  }
  
  // Errores
  // categories.push(valorUser)
  // setCategories( ...categories, valorUser )

```



**useEffect**

Al trabajar con APIS o cualquier otro tipo de funciones en React, puede existir el inconveniente de que el renderizar nuevamente un elemento todas las funcionalidades se ejecuten nuevamente, esto incluye a las peticiones HTTP. Para evitar esto recurrimos a useEffect, el cual permite ejecutar código de manera condicional.

```js
  useEffect( () => {
    getGifs()
  }, [])
```

> Para utilizar useEffect pasamos una función como parámetro y en su interior definimos la función que queremos que se ejecute solo una vez al renderizar por primera vez. Como segundo argumento indicamos un arreglo vacío.



**Custom Hook**

A la hora de generar Custom Hooks se recomienda generar un directorio de Hooks. Los Hooks no son más que funciones que pueden contener otros hooks en su interior.

```jsx
// Hook generado
export const useFetchGifs = () => {

  const [state, setstate] = useState({
      data: [],
      loading: true
  })

  setTimeout(() => {
    setstate( {...state, loading: false } )
  }, 3000);
  
  return state
}


// Llamando al Hook
import React from 'react'
import { useFetchGifs } from '../hooks/useFetchGifs'

export const GifGrid = ({ category }) => {
  const { loading } = useFetchGifs()

  return (
    <>
    <h3> {category} </h3>
    { loading ? 'Cargando...' : 'Data Cargada'}
    </>
  )
}
```

> En este caso tenemos un Hook personalizado que genera un objeto el cual modifica su propio estado pasados tres segundos. Al cargar el hook en el componente des estructuramos uno de los valores retornados y lo utilizamos en la renderización.



**Custom Hook para esperar petición**

```js
import { useState, useEffect } from "react"
import { getGifs } from '../helpers/getGifs'


export const useFetchGifs = ( category ) => {

  const [state, setstate] = useState({
      data: [],
      loading: true
  })

  
  useEffect( () => {
    getGifs( category ).then( imgs => {
      setstate({
        data: imgs,
        loading: false
      })
    } )
  }, [category], )
  
  return state
}

```





### Fetch - Consumir API 

Se recomienda siempre probar las APIS en Post Man para verificar que se estén obteniendo los datos correctos. 



**Obtener valor especifico de una API**

```js

  const getGifs = async() => {

    const URL = 'https://api.giphy.com/v1/gifs/search?q=Berzerk&limit=10&api_key=2mFkDLzGzkjoLCesSfoqvNziOwipGLID'
    const resp = await fetch( URL )
    const { data } = await resp.json()
    const gifs = data.map( img => {
      return{
        id: img.id,
        title: img.title,
        url: img.images.downsized_medium.url
      }
    })
    console.log(gifs)

  }
```







**COSAS UTILES SIN CATEGORIA**



**Cargar varios elementos en una etiqueta**

En este caso se carga un Array de elementos a un elemento `<li>`.

```jsx
const [ categories, setCategories ] = useState(["Howl's Moving Castle", 'Berzerk', 'Attack On Titan'])

  return ( 
    <>
      <ol>
        { 
        categories.map( ( cat ) => {
        return <li key={ cat }>{ cat }</li>
        }) 
        }
      </ol>
    </>
   );


// NO HACER ESTO

  { 
    categories.map( ( cat, i ) => {
    return <li key={ i }>{ cat }</li>
    }) 
  }

```

> Siempre es importante agregar la propiedad `key` en React. Nunca utilizar el índice para estos casos, por que es muy volátil, puede cambiar y generar errores.
>
> En casos reales los keys que se pasan son ids de bases de datos.







**Helpers**

Los helpers son funciones que realizan un cierto trabajo en especifico que reciben argumentos y devuelven algo.









### DEPLOYMENT

Esta sección añadirla en otro archivo o algún documento especial.



**Crear Build con create-react-app**

```js
npm run build
```



**Desplegar a Github Pages**

Renombramos la carpeta de producción como "docs" para que sea entendido por GitHub. De esta forma mediante la configuración del repositorio serviremos la página. sin usar otras ramas adicionales.





------------------





**Cargar componente según valor del state**

Podemos definir si cargar o no un componente dependiendo del valor de su estado asociado.

```jsx
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



**Ex. Función para agregar productos**

```jsx
  const selectProduct = id => {
    const product = products.filter(product => product.id === id)[0]
    setCart([...cart, product])
  }
```

> Al hacer clic actualizaremos el estado, sin embargo cada valor nuevo añadido reemplazara al anterior. Por lo que en el nuevo estado pasamos todo el contenido del estado anterior y el nuevo.

**Ex. Función para borrar producto**

```jsx
const removeProduct = id => {
  const products = cart.filter( product => product.id !== id )
  setCart(products)
}
```

> Mediante filter removemos todo elemento que coincida con el elemento a borrar.


