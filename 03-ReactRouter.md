# React Router

React Router es una librería que ayuda a generar Single Page Apps.



**Multi page Apps**: Cada página o enlace implica una petición al servidor. La respuesta usualmente posee todo el contenido de la página por lo que esta se debe volver a renderizar cada vez. 

**Single page Apps**(SPA): Aplicaciones que cargan una sola página de HTML y cualquier actualización la hacen re-escribiendo el HTML que ya poseían. No se debe volver a renderizar toda la página es más fluida.



`<BrowserRouter>`: es un componente que se debe declarar al comienzo de la aplicación, permite que el resto de herramientas de React Router en su interior funcionen correctamente.



`<Route />`: representa una dirección de internet donde son incluidos los componentes.

```react
/*EXAMPLE*/
<Route path="/" component={home} />
```

> `path=""`: donde se renderizara el componente.
>
> `component={}`: que componente será renderizado. Recibirá tres props (match, history y location).



`<Switch/>`: es un componente que sirve para presentar una única ruta de varias que se podrán incluir dentro del componente.



`<Link/>`: Reemplaza al elemento `<a>`, evita que se recargue la página y actualiza la URL. 



**Instalar React Router**

```bash
npm install react-router-dom
```



### Dividir aplicación en rutas

Como ejemplo convertiremos las páginas Badges y BagesNew en rutas y las enlazaremos.

* Importamos un nuevo componente que crearemos denominado App en index.js.

```react
import React from 'react'
import {BrowserRouter, Route, Switch} from 'react-router-dom'
import BadgeNew from '../pages/BadgeNew'
import Badges from '../pages/Badges'


function App(){
    return(
        <BrowserRouter>
            <Switch> /* Lo utilizamos para no renderizar dos páginas al mismo tiempo */
                <Route exact path="/badges" component={Badges} /> // Utilizamos exact para que las rutas no sean relativas, pues nos quedariamos en bucle unicamente en Badges
                <Route exact path="/badges/new" component={BadgeNew} />
            </Switch>
        </BrowserRouter>
    )
}   

export default App
```

> Al utilizar las rutas http://localhost:3000/badges/ y http://localhost:3000/badges/new accedemos a las rutas que hemos definidos con `Route` dentro de `Switch` que a su vez deben estar en `BrowserRouter`.



Finalmente para poder garantizar el correcto uso de una Single Page App debemos reemplazar los elementos `<a>` con elementos `<Link>` para que se rendericen ciertos elementos y no el sitio completo.

```react
import React from 'react'
...
import {Link} from  'react-router-dom'

export class Badges extends React.Component {
    render() {
        return (
		...

                <div className="Badge__container">
                    <div className="Badges__buttons">
                        <Link to="/badges/new" className="btn btn-primary" >New Badge</Link>
                    </div>
                </div>

		...
```

> A diferencia de la etiqueta de enlace original con `Link` utilizamos el atributo `to` en reemplazo de `href`. Obviamente debemos importar react-router-dom.



### User interface con Layout

En cada sitio del proyecto existe en común un Navbar que se muestra igual en todos los casos.

* Creamos el componente denominado Layout.js

```react
import React from 'react'
import Navbar from './Navbar'

function Layout(props){
    return (
        <React.Fragment>
            <Navbar/>
            {props.children}
        </React.Fragment>
        )
}

export default Layout
```

> `<React.Fragment>` permite reemplazar los `<div>` vacíos que no poseen ninguna funcionalidad, los cuales son únicamente utilizados para ser utilizados como elemento principal para ser renderizado.

* Eliminamos todas las declaraciones del componente Navbar de las páginas.



### Generar 404

**Declaramos la ruta**

```react
import React from 'react'
import {BrowserRouter, Route, Switch} from 'react-router-dom'
import BadgeNew from '../pages/BadgeNew'
import Badges from '../pages/Badges'
import Layout from '../components/Layout'

function App(){
    return(
        <BrowserRouter> 
            <Layout>
                <Switch> /* Lo utilizamos para no renderizar dos páginas al mismo tiempo */
                    ...
                    <Route component={NotFound} />
                </Switch>
            </Layout>
        </BrowserRouter>
    )
}   

export default App
```



**Generamos la página**

```react
import React from 'react'

function NotFound(){
    return <h1>404: NOT FOUND</h1>
}

export default NotFound
```

