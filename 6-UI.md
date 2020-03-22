# UI



Las aplicaciones deben permitir al usuario crear, editar, listar, actualizar o borrar recursos. 



### Añadir detalles a los Badge (Páginas dinámicas)



**Creamos el componente**

```react
import React from 'react'
import confLogo from '../images/platziconf-logo.svg'
import './styles/BadgeDetails.css'
import PageLoading from '../components/PageLoading'
import api from '../api'
import Badge from '../components/Badge'
import {Link} from 'react-router-dom'

class BadgeDetails extends React.Component{
    
    state ={
        loading: true,
        error: null,
        data: undefined
    }

    componentDidMount(){
        this.fetchData()
    }

    fetchData = async () => {
        this.setState({ loading: true, error: null })

        try{
            const data = await api.badges.read(
                this.props.match.params.badgeId
            )
            this.setState({ loading: false, data: data })
        }catch(error){
            this.setState({ loading: false, error: error })
        }
    }
    
    render(){
        if(this.state.loading){
            return <PageLoading/>
        }
        if(this.state.error){ // Mostramos un texto mientras se realiza la petición.
            return `Error: ${this.state.error.message}`
        }
        return(
            <div>
                <div className="BadgeDetails__hero">
                    <div className="container">
                        <div className="row">
                            <div className="col-6">
                                <img src={confLogo} alt="Logo de la conferencia"/>
                            </div>
                            <div className="col-6 BadgeDetails__hero-attendant-name">
                                <h1>{this.state.data.firstName} {this.state.data.lastName}</h1>
                            </div>
                        </div>
                    </div>
                </div>
                <div className="container">
                    <div className="row">
                        <div className="col">
                            <Badge 
                                firstName={this.state.data.firstName}
                                lastName={this.state.data.lastName}
                                email={this.state.data.email}
                                jobTitle={this.state.data.jobTitle}
                            />
                        </div>
                        <div className="col">
                            <h2>Actions</h2>
                            <div>
                                <div>
                                    <Link className="btn btn-primary mb-4" to={`/badges/${this.state.data.id}/edit`} >Edit</Link>
                                </div>
                                <div>
                                    <button className="btn btn-danger">Delete</button>
                                </div>

                            </div>
                        </div>
                    </div>   
                </div>
            </div>
        )
    }
}

export default BadgeDetails
```

> La página muestra el badge con toda su información y presenta unos botones que permitirán o editar o borrar el badge seleccionado.



**Añadir ruta**

```react
<Route exact path="/badges/:badgeId" component={BadgeDetails}/>
```



### UI Componentes y Container Components

El badge que acabamos de generar se encarga de dos cosas: mostrar la interfaz y de obtener los datos necesarios para hacer la presentación, sin embargo siempre es mejor separar las tareas en pequeñas funciones, cuando un componente hace demasiado es mejor partirlo en dos.



Procederemos a dividir el componente en dos (BadgeDetails y BadgeDetailsContainer).



El componente actual se encargara de la lógica y pasara a llamarse BadgeDetailsContainer siendo considerado un Container Component:

```react
import React from 'react'
import PageLoading from '../components/PageLoading'
import api from '../api'
import BadgeDetails from '../pages/BadgeDetails'

class BadgeDetailsContainer extends React.Component{
    
    state ={
        loading: true,
        error: null,
        data: undefined
    }

    componentDidMount(){
        this.fetchData()
    }

    fetchData = async () => {
        this.setState({ loading: true, error: null })

        try{
            const data = await api.badges.read(
                this.props.match.params.badgeId
            )
            this.setState({ loading: false, data: data })
        }catch(error){
            this.setState({ loading: false, error: error })
        }
    }
    
    render(){
        if(this.state.loading){
            return <PageLoading/>
        }
        if(this.state.error){ // Mostramos un texto mientras se realiza la petición.
            return `Error: ${this.state.error.message}`
        }
        return(
            <BadgeDetails badge={this.state.data}/>
        )
    }
}

export default BadgeDetailsContainer
```



Mientras que el componente BadgeDetails se encargara de la vista siendo un componente UI:

```react
import React from 'react'
import confLogo from '../images/platziconf-logo.svg'
import Badge from '../components/Badge'
import {Link} from 'react-router-dom'
import './styles/BadgeDetails.css'


function BadgeDetails(props){

    const badge = props.badge

    return(
        <div>
            <div className="BadgeDetails__hero">
                <div className="container">
                    <div className="row">
                        <div className="col-6">
                            <img src={confLogo} alt="Logo de la conferencia"/>
                        </div>
                        <div className="col-6 BadgeDetails__hero-attendant-name">
                            <h1>{badge.firstName} {badge.lastName}</h1>
                        </div>
                    </div>
                </div>
            </div>
            <div className="container">
                <div className="row">
                    <div className="col">
                        <Badge 
                            firstName={badge.firstName}
                            lastName={badge.lastName}
                            email={badge.email}
                            jobTitle={badge.jobTitle}
                        />
                    </div>
                    <div className="col">
                        <h2>Actions</h2>
                        <div>
                            <div>
                                <Link className="btn btn-primary mb-4" to={`/badges/${badge.id}/edit`} >Edit</Link>
                            </div>
                            <div>
                                <button className="btn btn-danger">Delete</button>
                            </div>

                        </div>
                    </div>
                </div>   
            </div>
        </div>
    )
}

export default BadgeDetails
```



Esto es mucho más fácil de leer.



### Portales

En ocasiones resulta complicado realizar ciertas acciones como mostrar un modal o realizar acciones relacionadas a CSS debido a las propiedades que poseen otros componentes que evitan que se puedan realizar ciertas acciones. Para solucionar eso es recomendado renderizar en un nodo aparte.



**Generar portal**

```react
<div>
	<button className="btn btn-danger">Delete</button>
	{ReactDOM.createPortal(<h1>PORTAL</h1>, document.getElementById('modal'))}
</div>
```

> Generamos un portal que obtenemos desde index.html



```react
/* index.html */

<body>
  <div id="app"></div>
  <div id="modal"></div>
</body>
```

> De esta forma hemos conseguido renderizar en un nodo aparte.



### Modal

```react
/* Modal.js */

function Modal (props){
    if(!props.isOpen){
        return null
    }
    return ReactDom.createPortal(
        <div className="Modal">
            <div className="Modal__container">
                <button onClick={props.onClose} className="Modal__close-button">X</button>
                {props.children}
            </div>
        </div>
        , 
        document.getElementById('modal'))
}

export default Modal
```

> Cargamos el modal en un portal, y mediante funciones y props revisamos el estado de cada componente.
>
> En este caso no renderizamos nada en caso de que el modal no haya sido abierto, en caso de que si renderizamos el componente y recibimos un prop que nos permite realizar la acción de cerrar.



```react
/* DeleteBadgeModal */

function DeleteBadgeModal(props){
    return <Modal 
        isOpen={props.isOpen} onClose={props.onClose}
    >
        <div className="DeleteBadgeModal">
            <h1>Are you sure?</h1>
            <p>You are about to delete this badge.</p>

            <div>
                <button onClick={props.onDeleteBadge} className="btn btn-danger mr-4">Delete</button>
                <button onClick={props.onClose} className="btn btn-primary">Cancel</button>
            </div>
        </div>
    </Modal>
}

export default DeleteBadgeModal
```

> Este componente utiliza el componente Modal que creamos previamente para permitir tanto eliminar un Badge como cancelar la operación todas las acciones son escuchadas mediante los props que importamos de los componentes en los que se realizan los cambios.



```react
/* BadgeDetails */

...
<div>
    <button onClick={props.onOpenModal} className="btn btn-danger">Delete
    </button>
    <DeleteBadgeModal 
        isOpen={props.modalIsOpen} 
        onClose={props.onCloseModal}
        onDeleteBadge={props.onDeleteBadge}
        />
</div>
...
```

> BadgeDetails se encarga de mostrar la interfaz de BadgeDetails por lo que en ella definimos los props enviados a DeleteBadgeModal.



```react
/* BadgeDetailsContainer */

class BadgeDetailsContainer extends React.Component{
    
    state ={
        loading: true,
        error: null,
        data: undefined,
        modalIsOpen: false,
    }

	...
    handleOpenModal = e => {
        this.setState({ modalIsOpen: true })
    }

    handleCloseModal = e => {
        this.setState({ modalIsOpen: false })
    }

    handleDeleteBadge = async e => {
        this.setState({ loading: true , error: null })
        try{
            await api.badges.remove(
                this.props.match.params.badgeId
            )
            this.props.history.push('/badges')
        }catch(error){
            this.setState({ loading: false, error: error })
        }
    }
    
    render(){
	...
        return <BadgeDetails 
                onCloseModal={this.handleCloseModal} 
                onOpenModal={this.handleOpenModal}
                modalIsOpen={this.state.modalIsOpen}
                onDeleteBadge={this.handleDeleteBadge}
                badge={this.state.data}
                />
        
    }
}

export default BadgeDetailsContainer
```

> Finalmente en BadgeDetailsContainer generamos toda la lógica y definimos los props además de crear las funciones.



### Hooks

Todos los componentes desarrollados hasta ahora han sido de dos tipos: 

* **Clases**: posee estados propios y ciclos de vida.
* **Funciones**: los componentes no poseen un estado propio que manejar o ciclos de vida.



React posee un feature denominado **Hooks**, el cual permite que las funciones también posean propiedades que poseen las clases.

* **useState**: permite manejar estado en funciones.
* **useEffect**: para suscribir el componente a su ciclo de vida.
* **useReducer**: ejecutar un efecto basado en una acción.



```react
function BadgeDetails(props){

    const [ count, setCount] = React.useState(0)
    const badge = props.badge

    return(
			...
            <button onClick={props.onOpenModal} className="btn btn-danger">Delete</button>
```

> Con la función lo que hacemos es leer el cambio de estado que nos otorga el hook de useState().



**Custom Hooks**

Mediante los hooks fundamentales se pueden crear nuevos hooks custom. Estos irán en su propia función y su nombre comienza con "use".

```react
function useIncreseCount(max){
    const [ count, setCount ] = React.useState(0)
    if(count > max){
        setCount(0)
    }

    return [ count, setCount ]
}

function BadgeDetails(props){

    const [ count, setCount] = useIncreseCount(4)
    const badge = props.badge
```

> Mediante el uso de custom hooks podemos utilizar las propiedades de convencionales de los hooks y modificarlas según las necesidades, en este caso pasar estados para incrementar valores hasta un número máximo.



### Search Filter

El siguiente código permite realizar una barra de búsqueda mediante el nombre y el apellido de la lista de badges.

* Convertimos la clase BadgesList en una función.

```react
function useSearchBadges(badges){
  const [ query, setQuery ] = React.useState('')
  const [ filterBadges, setFilterBadges ] = React.useState(badges)
  React.useMemo(() => {
    const result = badges.filter(badge => {
    return `${badge.firstName} ${badge.lastName}`.toLowerCase().includes(query.toLowerCase())
    })
      setFilterBadges(result)
  }, [ badges, query ])

  return { query, setQuery, filterBadges }
}

function BadgesList (props) {
  const badges = props.badges
  const { query, setQuery, filterBadges } = useSearchBadges(badges)

  if(filterBadges.length === 0){
    return(
      <div>
        <div className="form-group">
          <label>Filter Badges</label>
          <input type="text" className="form-control"
            value={query}
            onChange={(e) => {
              setQuery(e.target.value)
            }}
          />
        </div>

        <h3>No badges were found.</h3>
        <Link className="btn btn-primary" to="/badges/new">Create new badge</Link>
      </div>
    )
  }

  return (
    <div className="BadgesList">
      <div className="form-group">
        <label>Filter Badges</label>
        <input type="text" className="form-control"
          value={query}
          onChange={(e) => {
            setQuery(e.target.value)
          }}
        />
      </div>
        <ul className="list-unstyled">
          {filterBadges.map(badge => {
            return (
              <li key={badge.id}>
                <Link className="text-reset text-decoration-none" to={`/badges/${badge.id}/`}>
                  <BadgesListItem badge={badge} />
                </Link>
              </li>
            );
          })}
        </ul>
    </div>
  );
}

export default BadgesList;
```

Se encapsulo toda la lógica, en un solo sitio.