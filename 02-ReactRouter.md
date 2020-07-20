# React Router

React Router es una librería que ayuda a generar Single Page Apps.

**Multi page Apps**: Cada página o enlace implica una petición al servidor. La respuesta usualmente posee todo el contenido de la página por lo que esta se debe volver a renderizar cada vez. 

**Single page Apps**(SPA): Aplicaciones que cargan una sola página de HTML y cualquier actualización la hacen re-escribiendo el HTML que ya poseían. No se debe volver a renderizar toda la página es más fluida.



**Instalar React Router**

```bash
npm install react-router-dom
```



**Crear directorio de rutas**

> Se genera el archivo /src/routes/AppRoutes.jsx

```jsx
import React from 'react'
import {
  BrowserRouter as Router,
  Switch,
  Route
} from "react-router-dom";
import { Navbar } from '../components/Navbar';
import { Works } from '../pages/Works';
import { Skills } from '../pages/Skills';
import { Notes } from '../pages/Notes';
import { Contact } from '../pages/Contact';
import { App } from '../pages/App';

export const AppRouter = () => {
  return (
    <Router>
      <div>
        <Navbar />
        <Switch>
          <Route exact path="/list-dogs" component={ ListOfDogs } />
          <Route exact path="/add-dog" component={ AddToList } />
          <Route exact path="/map" component={ MapPage } />
          <Route exact path="/" component={ App } />
          <Redirect to="/map" />
        </Switch>
      </div>
    </Router>
  )
}
```

> Si introducimos una ruta aleatoria es posible redirigir a una ruta por defecto con `<Redirect to="/PAGE" />`. Este funciona igual que un `<Link>`.



**Generamos navbar para rutas**

```jsx
import React from 'react'
import { Link, NavLink } from 'react-router-dom'
import './styles/Navbar.sass'

export const Navbar = () => {
  return (
    <header>
      <nav>
        <div className="img_container">
          {/* <img src="" alt=""/> */}
          <h1>Carlos Brignardello</h1>
        </div>
        <ul>

          <li>
            <Link to="/">Inicio</Link>
          </li>
          <li>
            <Link to="/works">Trabajos</Link>
          </li>
          <li>
          <Link to="/skills">Skills</Link>
          </li>
          <li>
          <Link to="/notes">Apuntes</Link>
          </li>
          <li>
          <Link to="/contact">Contacto</Link>
          </li>
        </ul>
      </nav>
    </header>
  )
}
```

> Si pueden manejar varios sistemas de rutas, pero debe existir un principal que es el que maneja el `<Router />`. El resto de ellos solo requieren de un `<Switch />`



**Manejar multiples rutas**

Si queremos tener más de un conjunto de rutas podemos separarlo teniendo un archivo de rutas principal que importe los conjuntos de rutas y en cada conjunto de rutas van más rutas en su interior que no deben incorporar el componente `<Router />`.



**Redirigir a una ruta con un evento**

Al utilizar rutas ahora vienen varios props adicionales en cada componente de ruta. Estos nos permitirá manejar lógicamente su comportamiento.

```js
export const LoginScreen = ({ history }) => {
    
  const handleLogin = () => {
    // history.push('/')
    history.replace('/')
  }
  ...
```

> Este manejador utiliza el prop `history` para manejar las páginas recorridas. En este caso para redirigirnos con un botón podemos usar `history.push()` o `history.replace()`. El primero te envía a una nueva ruta en una nueva página sin abrir una página en blanco o refrescar. El segundo te re envía pero no guarda la página anterior, no identifica la página nueva como tal. No la guarda en la historia.
>
> También existen métodos adicionales como `history.goBack()` que permite devolver al usuario una página hacia atrás. Se puede validar para que si no existen páginas previas te mande a la raiz.



**Redirigir a una ruta dinámica**

```jsx
// En un componente
	<Link to={`./hero/${id}`}>Ver</Link>

// En las rutas
        <Switch>
            <Route exact path="/list-dogs" component={ ListOfDogs } />
            <Route path="/list-dogs/dog/:dogId" component={ DogPage } />
            <Route exact path="/add-dog" component={ AddToList } />
            <Route exact path="/map" component={ MapPage } />
            <Route exact path="/" component={ App } />
            <Redirect to="/map" />
        </Switch>
```

> Primero se debe definir mediante un Link que ruta se generará, en este caso, se generara una ruta según el ID obtenido en el componente.
>
> En las rutas debemos crear una ruta que NO sea exacta y el componente que se utilizará para modelar la información.



**Leer argumentos por URL**

En base a lo anterior se pueden leer los argumentos otorgados a las URLs dinámicas. Para ello utilizamos un Hook y una función que mediante el parámetro ingresado nos devuelva la data.

```jsx
// Función para obtener nuevamente la data
import { dogs } from '../data/dogs'

export const getDogById = id => {
  return dogs.find( dog => dog.id === id )
}


// Página renderizada
import React from 'react'
import { useParams } from 'react-router-dom'
import { getHeroById } from '../../selectors/getHeroesById'

export const HeroScreen = () => {

  /* Mediante el Hook leemos el dato de la URL */
  const { heroId } = useParams()
  /* Utilizamos el dato como queramos */
  const hero = getHeroById( heroId )

  /* Si no coincide con nuestros parametros devolvemos a la ruta por defecto */
  if( !hero ){
    return <Redirect to="/" />
  }

  const { superhero, publisher } = hero

  return (
    <div>
      { /* Renderizamos con la información */}
      <h1>{superhero}</h1>
    </div>
  )
}

```

> En primer lugar debemos obtener el parámetro ingresado, anteriormente lo enviamos como: `<Route path="/list-dogs/dog/:dogId" component={ DogPage } />` . Por lo que si queremos obtener el parámetro de la URL denominado "dogId", debemos utilizar el hook "useParams". 
>
> Al disponer del parámetro que no es más que un strin, debemos encontrar la forma de recuperar la data mediante alguna función en este caso, utilizamos la función descrita que busca en la data según un ID ingresado. De esta forma guardamos cada objeto con des estructuración y lo renderizamos en el sitio.



**useMemo**

Podemos usar useMemo para evitar que se vuelva a buscar el ID si este no cambia.

```js
export const DogPage = ({ history }) => {

  const { dogId } = useParams()
  const { name, description, color, size, id } = useMemo( getDogById( dogId ), dogId)
```



**SearchComponent**

Revisar el archivo descargado y tomar apuntes, hacerlo funcionar en la de perros



Para hacer un componente Search debemos crear un componente en el que ingresemos un formulario con un input y un boton, al pulsar el boton se enviara la información contenida en el input a la url, luego esta url se debe leer mediante useLocation y pasar el objeto search a la libreria:

npm install query-string

Esta libreria leera el valor mediante `queryString.parse( search )`

Luego le pasamos una función que filtre el valor contenido con la data para que nos devuelva un elemento renderizado al hacer click en el boton.



```
import React, { useState, useMemo } from 'react'
import '../styles/pages/Search.sass'
import { dogs } from '../data/dogs' 
import { useLocation } from 'react-router-dom'
import queryString from 'query-string'
import { useForm } from '../hooks/useForm'

export const Search = ({ history }) => {

  const getDogsByName = ( name = '' ) => {

    if ( name === '' ) {
        return [];
    }

    name = name.toLocaleLowerCase();
    return dogs.filter( dog => dog.name.toLocaleLowerCase().includes( name )  );

}

  const location = useLocation();
  const { q = '' } = queryString.parse( location.search );

  const [ formValues, handleInputChange ] = useForm({
    searchText: q
  });
  const { searchText } = formValues;

  const dogsFiltered = useMemo(() => getDogsByName( q ), [q])


  const handleSearch = (e) => {
      e.preventDefault();
      history.push(`?q=${ searchText }`);
  }

  return (
    <div className="search">
      <h1>Buscador</h1>
      <form onSubmit={ handleSearch }>
        <input
          type="text"
          placeholder="Encuentra un perro"
          name="searchText"
          autoComplete="off"
          value={ searchText }
          onChange={ handleInputChange }
        />
        <button >Buscar</button>
      </form >
      <div className="cards">
        {
          dogsFiltered.map( dog => {
          return(
            <div className="result-card" key={dog.id}>
              <h3>{dog.name}</h3>
              <p>{dog.location}</p>
            </div>
            )
          })
        }

      </div>
    </div>
  )
}

```

```
import { useState } from 'react';


export const useForm = ( initialState = {} ) => {
    
    const [values, setValues] = useState(initialState);

    const reset = () => {
        setValues( initialState );
    }


    const handleInputChange = ({ target }) => {

        setValues({
            ...values,
            [ target.name ]: target.value
        });

    }

    return [ values, handleInputChange, reset ];

}
```







### Protección de rutas



**Archivo types**

Para no tener problemas con los types que trabajaremos los almacenaremos en un archivo especial denominado /types/types.js

```js
export const types = {
  login: '[auth] login',
  logout: '[auth] logout',
}
```



**Auth**

Generamos la ruta auth donde generamos un customHook denominado authReducer, en su interior definimos lo siguiente:

```js
import { types } from "../types/types";

export const AuthReducer = ( state = {}, action ) => {

  switch ( action.type ) {
    case types.login:
      return{
        ...action.payload,
        logged: true
      }
    case types.logout:
      return{
        logged: false
      }
    default:
      return state
  }
}
```

> Con este Hook podemos hacer un dispatch de acciones a lo largo de toda la aplicación. 





Generamos un context que permitirá compartir el reducer anterior a lo largo de toda la aplicación.

```
import React, { createContext } from 'react'

export const AuthContext = createContext()
```

