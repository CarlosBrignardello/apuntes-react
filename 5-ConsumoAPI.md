# REACT APIS

Las llamadas a una API poseen un patrón muy similar siempre que se realizan. Las llamadas a una API constan de tres estados:

* **Loading**: la petición se envía y esperamos | Es recomendado mostrar una animación o un indicador de que se esta procesando la petición.
* **Error**: respuesta como error. | En caso de existir un error es importante presentarlo, mostrando una pantalla especifica.
* **Datos**: respuesta como un dato. | Al obtener una información debemos presentarla. La respuesta pueden ser datos vacíos o varios de ellos.



### Traer datos de un API

Como ejemplo inicial utilizaremos la API de rick and morthy:

* https://rickandmortyapi.com



```react
this.state = { // Definimos el formato de guardado de los datos.
    data: {
        results: [],
    }
}

componentDidMount(){
    // Al montar el componente se inicia la petición.
	this.fetchCharacters()
}

fetchCharacters = async () => {
    const response = await fetch('https://rickandmortyapi.com/api/characters') // Permite hacer una petición GET.
    const data = await response.json() // Obtenemos los datos en formato .json.
    
    this.setState({ // Guardamos los datos en el estado del componente.
        data: data
    })
}

render(){
    return(
    ...
    // Renderizamos los datos obtenidos.
    <ul className="row"> 
    	{this.state.data.results.map(character =>(
            <li className="col-6 col-md-3 key={character.id}">
            	<CharacterCard chatacter={character} />    
            </li>
            ))}    
    </ul>
    )
}
```



**Añadir loading y error**

```react
state = {
	loading: true, // Los declaramos al inicio del estado.
    error: null,
	data: {
		resuls: [],
	}
}

fetchCharacters = async () => {
    this.setState({ loading: true, error: null }) // Definimos el estado.
}
	try{ // Intentamos realizar la petición
        const response = await fetch('https://rickandmortyapi.com/api/characters')
        const data = await response.json()
        
        this.setState({ // Al finalizar perdemos el estado de loading y obtenemos los datos.
            loading: false,
            data: data,
        })
    } catch(error){ // En caso de fallar activamos el error.
        this.setState({
            loading: false,
            error: error
        })
    }
}
render(){
    if(this.state.error){ // En caso de que exista un error.
        render `Error: ${this.state.error.message}`
    }
    return()
......

{this.state.loading &&( // Mostramos cuando se este cargando la petición.
 	<div className="loader"> <Loader /> </div>
 )}
```



### Solicitando datos (GET)



**Badges.js**

```react
export class Badges extends React.Component {

    state = { // Definimos el estado.
        loading: true,
        error: null,
        data: undefined,
      };

    componentDidMount(){ // Realizamos la petición al montar el componente.
        this.fetchData()
    }
        
        
    fetchData = async () =>{ // Declaramos la petición.
        this.setState({ loading: true, error: null })
        try{
            const data = await api.badges.list()
            this.setState({ loading: false, data: data })
        }catch(error){
            this.setState({ loading: false, error: error })
        }
    }
    
    render() {

        if(this.state.loading === true){ // Mostramos un texto mientras se realiza la petición.
            return 'Loading...'
        }

        if(this.state.error){ // Mostramos un texto mientras se realiza la petición.
            return `Error: ${this.state.error.message}`
        }

        return (
		....
```



**api.js**

```react
const BASE_URL = 'http://localhost:3001';

const delay = ms => new Promise(resolve => setTimeout(resolve, ms));
const randomNumber = (min = 0, max = 1) =>
  Math.floor(Math.random() * (max - min + 1)) + min;
const simulateNetworkLatency = (min = 30, max = 1500) =>
  delay(randomNumber(min, max));

async function callApi(endpoint, options = {}) {
  await simulateNetworkLatency();

  options.headers = {
    'Content-Type': 'application/json',
    Accept: 'application/json',
  };

  const url = BASE_URL + endpoint;
  const response = await fetch(url, options);
  const data = await response.json();

  return data;
  // return [];
}

const api = {
  badges: {
    list() {
      // throw new Error('Not Found') // Simulamos un error.
      return callApi('/badges');
    },
    create(badge) {
      return callApi(`/badges`, {
        method: 'POST',
        body: JSON.stringify(badge),
      });
    },
    read(badgeId) {
      return callApi(`/badges/${badgeId}`);
    },
    update(badgeId, updates) {
      return callApi(`/badges/${badgeId}`, {
        method: 'PUT',
        body: JSON.stringify(updates),
      });
    },
    // Lo hubiera llamado `delete`, pero `delete` es un keyword en JavaScript asi que no es buena idea :P
    remove(badgeId) {
      return callApi(`/badges/${badgeId}`, {
        method: 'DELETE',
      });
    },
  },
};

export default api;
```

> Este archivo se encarga de procesar los datos solicitados por nuestra petición.



**BadgesList.js**

```react
class BadgesListItem extends React.Component {
  render() {

    return (
        ...
        )
  }
}
class BadgesList extends React.Component {
  render() {

    if(this.props.badges.length === 0){
      return(
        <div>
          <h3>No badges were found.</h3>
          <Link className="btn btn-primary" to="/badges/new">Create new badge</Link>
        </div>
      )
    }

    return (
    ...
    );
  }
}

export default BadgesList;
```



### Mejorar la experiencia de usuario durante una petición



* Generamos un componente denominado PageLoading para utilizarlo en todos los procesos de carga de la aplicación.

```react
import React from 'react'
import './styles/PageLoading.css'
import Loader from '.Loader'

function PageLoading(){
    return(
        <div className="PageLoading">
            <Loader />
        </div>
    ) 
}

export default PageLoading
```

```react
import React, { Component } from 'react';

import './styles/Loader.css';

export default class Loader extends Component {
  render() {
    return (
      <div className="lds-grid">
        <div />
        <div />
        <div />
        <div />
        <div />
        <div />
        <div />
        <div />
        <div />
      </div>
    );
  }
}
```



### Enviar datos (POST)



**Otorgar valor por omisión**

```react
<Badge 
    firstName={this.state.form.firstName || 'FIRST_NAME'} 
    lastName={this.state.form.lastName || 'LAST_NAME'} 
    jobTitle={this.state.form.jobTitle || '"ENGINER"'} 
    twitter={this.state.form.twitter || 'NICK_NAME'}
    email={this.state.form.email || ''}
/>
```



**Trabajar avatars**

```react
import React from 'react';
import confLogo from '../images/badge-header.svg'
import './styles/Badge.css'
import Gravatar from '../components/Gravatar'

class Badge extends React.Component{
    render(){
        return <div className="Badge">
            <div className="Badge__header">
                <img src={confLogo} alt="Logo de la conferencia"/>
            </div>
            <div className="Badge__section-name">
                <Gravatar className="Badge__avatar" email={this.props.email} alt="Avatar" />
                <h1>{this.props.firstName} <br/>{this.props.lastName}</h1>
            </div>
            <div className="Badge__section-info">
                <h3>{this.props.jobTitle}</h3>
                <div>@{this.props.twitter}</div>
            </div>
            <div className="Badge__footer" >#platziconf</div>
        </div>
    }
}

export default Badge;
```

```react
import React from 'react'
import md5 from 'md5'

function Gravatar(props){
    const email = props.email
    const hash = md5(email);

    return(
        <img className={props.className} src={`https://www.gravatar.com/avatar/${hash}?d=identicon`} alt="Avatar" />
    )
}

export default Gravatar
```



**Enviar Datos**

```react
class BadgeNew extends React.Component {

    state = { // Debemos pre definir los valores del formulario.
        form: {
            firstName: '',
            lastName: '',
            email: '',
            twitter: '',
            jobTitle: ''
        }
    };

handleChange = e =>{
    this.setState({
        form:{
            ...this.state.form,
            [e.target.name]: e.target.value,
        }
    })
}

handleSubmit = async e => {
    e.preventDefault()
    this.setState({ loading: true, error: null })

    try{
        await api.badges.create(this.state.form)
        this.setState({ loading: false })
    } catch(error){
        this.setState({ loading: false, error: error })
    }
}

render(){
    return(
		...
        <div className="col-6">
            <BadgeForm 
                onChange={this.handleChange}
                onSubmit={this.handleSubmit} //
                formValues={this.state.form} />
        </div>
        ...
    )
}
}

export default BadgeNew;
```

> Añadimos como nuevo prop onSubmit={this.handleSubmit} y generamos una función denominada handleSubmit que generara un evento en el cual se realizara una llamada a la api.



```react
import React from 'react'

class BadgeForm extends React.Component{
...

    render(){
        return (
            <div>
                <h1>New Attendant</h1>
                <form onSubmit={this.props.onSubmit}>
                    <div className="form-group">
                        <label>First Name</label>
                        <input  onChange={this.props.onChange} 
                        className="form-control" 
                        type="text" 
                        name="firstName"
                        value={this.props.formValues.firstName}/>
                    </div>
                    ...
```

> Cuando el formulario sea enviado llamaremos a la propiedad que añadimos en BadgeNew, por lo que lo declararemos en la propiedad `onSubmit` del formulario de BadgeForm.js como {this.props.onSubmit}.



### Manejar estados durante petición en POST

Al enviar los datos también se debe manejar el estado de la petición. Existe un tiempo de espera que debe ser visualizado.



**Añadir Loader al enviar los datos**

```react
class BadgeNew extends React.Component {

    state = {
        // Añadimos los estados iniciales.
        loading: false,
        error: null,
        form: {
            firstName: '',
            lastName: '',
            email: '',
            twitter: '',
            jobTitle: ''
        }
    };

    ...
    render(){
        // En caso de que el loading este activo no queremos regresar el formulario si no más bien la visualización del loader.
        if(this.state.loading){
            return <PageLoading/>
        }
        
	...
```



**Añadir mensaje de error al enviar los datos**

```react
class BadgeNew extends React.Component {

...

    render(){
		...	
        <BadgeForm 
            onChange={this.handleChange}
            onSubmit={this.handleSubmit}
            formValues={this.state.form}
            error={this.state.error}
            />
        ...
```

> Añadimos la propiedad de error a BadgeForm.



```react
import React from 'react'

class BadgeForm extends React.Component{

...

    render(){
        return (
		...
                    <button onClick={this.handleClick} 
                    className="btn btn-primary">Save</button>
                    
                    {this.props.error && (
                    	<p className="text-danger">{this.props.error.message}</p>)}
                </form>
            </div>
        )
    }
}

export default BadgeForm;
```

> Mostraremos un texto de error debajo del botón en caso de que exista un error.



**Enviar al listado de Badges al enviar dato**

```react
class BadgeNew extends React.Component {
...
    handleSubmit = async e => {
        e.preventDefault()
            this.setState({ loading: true, error: null })

        try{
            await api.badges.create(this.state.form)
            this.setState({ loading: false })
            // Regresar automaticamente a la lista de Badges.
            this.props.history.push('/badges')
        } catch(error){
            this.setState({ loading: false, error: error })
        }
    }
...
```

> De esta forma enviamos los datos, manejamos los diferentes estados y los visualizamos cuando es necesario.



**Conservar iconos al enviar los datos**

En *BadgesList* se importa el Componente *Gravatar*.
Y se sustituye la etiqueta `<img>` por `<Gravatar>` en *BadgesListItem*.

```react
classBadgesListItemextendsReact.Component{
render() {
    return (
      <div className="BadgesListItem">
        <Gravatar 
          className="BadgesListItem__avatar"
          email={this.props.badge.email} 
          alt={`${this.props.badge.firstName} ${this.props.badge.lastName}`}
        />
        ...
```



### Actualizando datos (PUT)

* Creamos una nueva página denominada BadgeEdit como una copia de la página BadgeNew(reemplazamos todas las palabras clave "BadgeNew" por "BadgeEdit")



**Generamos ruta dinámica**

```react
<Route exact path="/badges/:badgeId/edit" component={BadgeEdit}/>
```



**Editar al hacer clic**

```react
    return (
      <div className="BadgesList">
        <ul className="list-unstyled">
          {this.props.badges.map(badge => {
            return (
              <li key={badge.id}>
                <Link className="text-reset text-decoration-none" to={`/badges/${badge.id}/edit`}>
                  <BadgesListItem badge={badge} />
                </Link>
              </li>
            );
          })}
        </ul>
      </div>
    );
```

> Mediante la etiqueta `<Link>` convertimos cada vista de badge en un enlace para editar el badge.



**Obtener datos originales para modificarlos**

Si editamos el badge este no poseerá los valores actuales que tiene por lo que tendremos que solicitarlos.

```react
class BadgeEdit extends React.Component {

    state = { 
        loading: true, // Comenzamos en un proceso de carga.
        error: null,
        form: {
            firstName: '',
            lastName: '',
            email: '',
            twitter: '',
            jobTitle: ''
        }
    };

    componentDidMount(){ // Al montar el componente solicitamos datos.
        this.fetchData()
    }

    fetchData = async e => {
        this.setState({ loading: true, error: null})

        try{
            const data = await api.badges.read( // Obtenemos los datos del badge que vamos a editar.
                this.props.match.params.badgeId
            )
            this.setState({ loading: false, form: data })
        }catch(error){
            this.setState({ loading: false, error: error })
        }
    }
    
    ...
    
        handleSubmit = async e => {
        e.preventDefault()
            this.setState({ loading: true, error: null })

        try{
            await api.badges.update(this.props.match.params.badgeId, this.state.form) // Permite actualizar los datos, en lugar de crearlos.
            this.setState({ loading: false })
            this.props.history.push('/badges')
        } catch(error){
            this.setState({ loading: false, error: error })
        }
    }
```



### Actualizaciones automáticas

Buscaremos que cada determinado tiempo se buscaran los datos y se actualizaran de forma automática.

```react
/* Badges.js */
    componentDidMount(){
        this.fetchData()

        this.intervalId = setInterval(this.fetchData(), 15000) //. Realizamos un llamado a los datos cada 15 segundos.
    }

	componentWillUnmount(){ // Desactivamos el intervalo al pasar a otra página.
        clearInterval(this.intervalId)
    }
```

> La única complicación procedente de este cambio es que cada cinco segundos se muestra el loader, lo que se vuelve muy incomodo para el usuario.



**Realizar la animación de carga solo al inicio**

```react
/* Badges.js */    

render() {

        if(this.state.loading === true && this.state.data === undefined){ // Mostramos un texto mientras se realiza la petición.
            return <PageLoading />
        }
```



**Añadir carga discreta**

* Generamos componente "MiniLoader".

```react
<div className="Badges__list">
    <div className="Badges__container">
        {this.state.loading && <MiniLoader/>}
        <BadgesList badges={this.state.data}/>
    </div>
</div>
```



### Extra: Utilizar axios



**Instalar paquete**

```bash
npm install axios
```



**Get**

```react
  async componentDidMount(){
    const response = await Axios.get('https://jsonplaceholder.typicode.com/users')
    this.setState({
      usuarios: response.data
    })
  }
```

