# Componentes



### ¿Que son?

En React existe una unidad básica de desarrollo denominada **Componente**, permiten separar la interfaz de usuario en piezas independientes, **reutilizables** y pensar en cada pieza de forma aislada. 

Conceptualmente, los componentes son como las funciones de JavaScript. **Aceptan entradas arbitrarias (llamadas “props”) y devuelven a React elementos** que describen lo que debe aparecer en la pantalla.

Se debe buscar que elementos se repiten y que elementos cumplen funciones muy especificas (tanto visual como de funcionalidad). Los componentes **sirven para encapsular lógica** y **agrupar comportamientos y aspectos visuales en un único lugar**.

> Una barra de búsqueda y una barra de navegación son ejemplos perfectos de un componente. Identificar componentes es una habilidad esencial para poder desarrollar aplicaciones de React.
>



### Definir un componente

Es recomendable guardar los componentes en una ruta del tipo *"/src/components"*



### Componentes funcionales y de clase



**Componente funcional**

La forma más sencilla de definir un componente es escribir una función de JavaScript:

```react
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

> Esta función es un componente de React válido porque acepta un solo argumento de objeto “props” (que proviene de propiedades) con datos y devuelve un elemento de React. **Llamamos a dichos componentes “funcionales” porque literalmente son funciones JavaScript**.



**Componente de clase**

También puedes utilizar una [clase de ES6](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Classes) para definir un componente:

```react
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

> Los dos componentes anteriores son equivalentes desde el punto de vista de React.
>
> A diferencia del componente funcional en el componente de clase debemos anteponer la palabra clave "**this**" para obtener los props del componente y no se necesitan definir las funciones en su interior.



### Renderizar un componente

Anteriormente, sólo encontramos elementos de React que representan las etiquetas del DOM:

```react
const element = <div />;
```

Sin embargo, los elementos también pueden representar componentes definidos por el usuario:

```react
const element = <Welcome name="Carlos" />;
```

Cuando React ve un elemento representando un componente definido por el usuario, pasa atributos JSX a este componente como un solo objeto. **Llamamos a este objeto “props”**.

Por ejemplo, este código muestra “Hola, Carlos” en la página:

```react
function Welcome(props) {
  return <h1>Hola, {props.name}</h1>;
}

const element = <Welcome name="Carlos" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```



### Extracción de componentes

Es posible dividir los componentes en otros más pequeños, por ejemplo el siguiente componente utilizado para renderizar unos comentarios en una web de redes sociales:

```react
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

> Este componente puede ser difícil de cambiar debido a todo el anidamiento, y también es difícil reutilizar partes individuales de él. 



Podemos extraer algunos componentes del mismo en este caso el fragmento que incorpora el Avatar:

```react
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```



Ahora podemos simplificar `Comment` un poquito:

```react
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```



Podemos repetir la extracción del componente aún más, podemos extraer un componente `UserInfo` que renderiza un `Avatar` al lado del nombre del usuario:

```react
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```



Finalmente el componente fue extraído tantas veces que termino así:

```react
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

> Esto es mucho más simple y escalable. Extraer componentes puede parecer un trabajo pesado al principio, pero tener una paleta de componentes reutilizables vale la pena en aplicaciones más grandes. Una buena regla en general es que si una parte de su interfaz de usuario se usa varias veces (`Button`, `Panel`, `Avatar`), o es lo suficientemente compleja por sí misma (`App`, `FeedStory`, `Comment`), es buen candidato para ser un componente reutilizable.



**Los props son de solo lectura**

Ya sea que declares un componente como una función o como una clase, **este nunca debe modificar sus props**.

```js
function sum(a, b) {
  return a + b;
}
```

Tales funciones son llamadas [“puras”](https://es.wikipedia.org/wiki/Programación_funcional#Funciones_puras) por que no tratan de cambiar sus entradas, y siempre devuelven el mismo resultado para las mismas entradas.

**Todos los componentes de React deben actuar como funciones puras con respecto a sus props.**

Por supuesto, las interfaces de usuario de las aplicaciones son dinámicas y cambian con el tiempo. Por ello existe el concepto de “estado”. El estado le permite a los componentes de React cambiar su salida a lo largo del tiempo en respuesta a acciones del usuario.



## Proyecto

* Generamos el archivo Badge.js

```react
import React from 'react'; // 1.
import confLogo from '../images/badge-header.svg'

class Badge extends React.Component{ // 2.
    render(){ // 3.
        return <div>
            <div>
                <img src={confLogo} alt="Logo de la conferencia"/>
            </div>
            <div>
                <img src="https://www.gravatar.com/avatar?d=identicon" alt="Avatar" />
                <h1>Carlos <br/>Brignardello</h1>
            </div>
            <div>
                <h3>Frontend Developer</h3>
                <div>@cbrigcode</div>
            </div>
        </div>
    }
}

export default Badge; // 4.
```

> 1. Como utilizaremos JSX importamos las librería de React.
> 2. Generaremos un componente de clase que heredara de la clase `React.Component`
> 3. Todos los componentes requieren al menos del método `render()`, este define cual será el resultado que se vera en pantalla.
> 4. Exportamos el componente.



* Importamos el componente en index.js y mostramos su contenido.

```react
import React from 'react';
import ReactDOM from 'react-dom';
import Badge from './components/Badge'  // 1.

const container = document.getElementById('app');

ReactDOM.render(<Badge />, container);  // 2.
```

> 1. Importamos el componente.
> 2. Al cargar un componente debemos cargarlo en formato de elemento.



### Aplicar estilos a un componente

**Estilos convencionales**

Para insertar estilos utilizamos CSS, para ello debemos hacer referencia a ello en el componente.

**Nota:** Generamos la ruta **/src/styles/components** o **/src/components/styles** para guardar todos los estilos que utilizaremos para los componentes.

```css
.Badge {
  background: #FFFFFF;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.10);
  border-radius: 8px 8px 8px 8px;
  overflow: hidden;
  height: 380px;
}

.Badge__header {
  padding: 0.5rem 0;
  height: 80px;
  background: #1B1B25;
  display: flex;
  justify-content: center;
}

.Badge__section-name {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem 0;
}

.Badge__section-info {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 0.5rem 0;
  background: #F4F4F7;
}


.Badge__avatar {
  border-radius: 50%;
  margin-right: 1rem;
  width: 120px;
  height: 120px;
}

.Badge__footer {
  height: 54px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #98CA3F;
  font-weight: bold;
  font-style: italic;
}
```

> Generamos el estilo para el componente.



Para vincular un estilo a un componente debemos importar el archivo, sin guardarlo en una variable, basta solo con traerlo y como es costumbre utilizamos las clases para vincular estilos, sin embargo **en React utilizamos el atributo `class=""` como `className=""`.**

```react
import './styles/Badge.css'
/*
import confLogo from '../images/badge-header.svg'
import React from 'react';

class Badge extends React.Component{
    render(){
		...
*/
```



### Importar Bootstrap



**Desde la consola:**

```bash
# Dentro del proyecto
npm install bootstrap
```

En nuestro **index.js** declaramos:

```react
import "bootstrap/dist/css/bootstrap.css";
# Esta es una versión, existen varias más en la ruta.
```



### Importar estilos globales

**Nota:** en **/src/** generamos un archivo denominado `global.css` que aplicara un mismo estilo a todos los elementos del proyecto.

Lo importamos:

> index.js

```react
import './global.css'
```

Generamos el archivo:

```css
@import url('https://fonts.googleapis.com/css?family=Lato:300,400,700');
@import url('https://fonts.googleapis.com/css?family=Anton');


* {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

body {
  font-family: 'Lato', 'Helvetica Neue', sans-serif;
  background-color: #F4F4F7;
}

h1 {
  font-family: 'Anton', sans-serif;
}

a.link-unstyled {
  color: inherit;
}

a.link-unstyled:hover {
  color: inherit;
  text-decoration: none;
}

.btn {
  padding: 0.375rem 2rem;
}

.btn.btn-primary {
  color: #fff;
  background-color: #7DCD40;
  border-color: #7DCD40;
}

.btn.btn-primary:not(:disabled):not(.disabled):hover {
  color: #fff;
  background-color: #7DCD40;
  border-color: #7DCD40;
}

.btn.btn-primary:not(:disabled):not(.disabled):active {
  color: #fff;
  background-color: #7DCD40;
  border-color: #7DCD40;
}

.btn.btn-primary:not(:disabled):not(.disabled):focus {
  box-shadow: 0 0 0 0.2rem rgba(125, 205, 64, 0.5);
}

.btn.btn-danger {
  color: #fff;
  background-color: #CD4040;
  border-color: #CD4040;
}

.btn.btn-danger:not(:disabled):not(.disabled):hover {
  color: #fff;
  background-color: #CD4040;
  border-color: #CD4040;
}

.btn.btn-danger:not(:disabled):not(.disabled):active {
  color: #fff;
  background-color: #CD4040;
  border-color: #CD4040;
}

.btn.btn-danger:not(:disabled):not(.disabled):focus {
  box-shadow: 0 0 0 0.2rem rgba(255, 64, 64, 0.5);
}
```



### Props

El componente generado anteriormente no es reusable, debido a que posee un  nombre definido en el mismo documento como es "Carlos Brignardello".



**Definir props en el componente**

```react
class Badge extends React.Component{
    render(){
        return <div className="Badge">
            <div className="Badge__header">
                <img src={confLogo} alt="Logo de la conferencia"/>
            </div>
            <div className="Badge__section-name">
                <img  className="Badge__avatar" src="https://www.gravatar.com/avatar?d=identicon" alt="Avatar" />
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
```



**Generar props**

Para enviar las propiedades lo hacemos al mostrar el componente desde **index.js**

```react
ReactDOM.render(<Badge 
    firstName="Carlos"
    lastName="Brignardello"
    jobTitle="Frontend Developer"
    twitter="cbrigcode"
    />, 
container); 
```



### Insertar componente en un sitio web

Una página es un componente que en su interior posee más componentes.

**Nota:** para almacenar las paginas generadas creamos la ruta **/src/pages**



* Generamos un componente que actuara como página denominado BadgeNew.js

```js
import React from 'react';
import './styles/BadgeNew.css';
import header from '../images/badge-header.svg';
import Navbar from '../components/Navbar';
import Badge from '../components/Badge'

class BadgeNew extends React.Component {
    render(){
        return(
            <div>
                <Navbar /> // Agregamos el componente
                <div className="BadgeNew__hero">
                    <img className="img-fluid" src={header} alt="Logo"/>
                </div>

                <div className="container">
                    <div className="row">
                        <div className="col">
                            <Badge firstName="Carlos" lastName="Brignardello" jobTitle="Frontend Developer" twitter="cbrigcode"/> // Agregamos las propiedades del componente Badge.
                        </div>
                    </div>
                </div>
            </div>
        )
    }
}
export default BadgeNew;
```



> Modificamos index.js

```react
import React from 'react';
import ReactDOM from 'react-dom';
import BadgeNew from './pages/BadgeNew'
import "bootstrap/dist/css/bootstrap.css";
import './global.css'

const container = document.getElementById('app');

ReactDOM.render(<BadgeNew 
    />, 
container); 
```



> Generamos un componente extra para la página denominado Navbar.js

```react
import React from 'react';
import './styles/Navbar.css';
import logo from '../images/logo.svg'

class Navbar extends React.Component {
    render(){
        return <div className="Navbar" >
            <div className="container-fluid">
                <a  className="Navbar__brand" href="/">
                    <img  className="Navbar__brand-logo" src={logo} alt="Logo"/>
                    <span className="font-weight-light" >Platzi</span>
                    <span className="font-weight-bold" >Conf</span>
                </a>
            </div>
        </div>
    }
}

export default Navbar;
```



Eventos



## Proyecto

### Enlazando eventos

* Generamos un nuevo componente denominado BadgeForm

```react
import React from 'react'

class BadgeForm extends React.Component{

    handleChange = (e) =>{ // Creamos un método que nos muestra el estado del evento al escribir.
        console.log({ 
            name: e.target.name,
            value: e.target.value });
    }

    handleClick = e =>{
        console.log("Button was clicked")
    }

    handleSubmit = e =>{ // Denegamos el refresco de la página al pulsar un boton
        e.preventDefault(); // No enviamos el formulario, no refrescamos.
        console.log("Form was submitted")
    }

    render(){
        return (
            <div>
                <h1>New Attendant</h1>
                <form onSubmit={this.handleSubmit}>
                    <div className="form-group">
                        <label>First Name</label>
                        <input  onChange={this.handleChange} className="form-control" type="text" name="firstName"/>
                    </div>
                    <button onClick={this.handleClick} className="btn btn-primary">Save</button>
                </form>
            </div>
        )
    }
}

export default BadgeForm;
```

> Con este componente enlazamos eventos, conectamos una acción del usuario con un componente de React, con este formulario podemos leer eventos del usuario, podemos leer cada carácter que escribe, cuando realiza un clic y cuando es enviado el formulario detenemos el proceso.



# Estado

El uso de los estados permite hacer a los componentes verdaderamente reutilizables y encapsulados. El estado es similar a las props, pero es privado y está completamente controlado por el componente. **Para utilizar estados es necesario trabajar con componentes de clase**.



### Manejo de estado

Hasta el momento los componentes han obtenido información mediante props provenientes de otros componentes. Existe una forma en que los componentes puedan producir y guardar su propia información.

La información a otros componentes pasaría en una sola dirección, sin embargo no podrá ser modificada.

El método `render` se invocará cada vez que ocurre una actualización. Esto nos permite utilizar características adicionales como el estado local y los métodos de ciclo de vida.



### Agregar estado local a una clase

1. Para poder trabajar con el estado utilizamos la palabra clave "**this.state.date**" en lugar de "**this.props.date**".

   ```react
   class Clock extends React.Component {
     render() {
       return (
         <div>
           <h1>Hello, world!</h1>
           <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
         </div>
       );
     }
   }
   ```

2. Debemos añadir un constructor de clase que asigne el estado inicial:

   ```react
   /*
   class Clock extends React.Component {
   */
     constructor(props) {
       super(props);
       this.state = {date: new Date()};
     }
   /*
     render() {
       return (
         <div>
           <h1>Hello, world!</h1>
           <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
         </div>
       );
     }
   }
   */
   ```

   > Los componentes de clase siempre deben invocar al constructor base con `props`.



**No modifiques el estado directamente**

Por ejemplo, esto no volverá a renderizar un componente:

```react
// Incorrecto
this.state.comment = 'Hello';
```

En su lugar utiliza `setState()`:

```react
// Correcto
this.setState({comment: 'Hello'});
```

El único lugar donde puedes asignar `this.state` es el constructor.



**Las actualizaciones del estado pueden ser asíncronas**

React puede agrupar varias invocaciones a `setState()` en una sola actualización para mejorar el rendimiento.

Debido a que `this.props` y `this.state` pueden actualizarse de forma asincrónica, no debes confiar en sus valores para calcular el siguiente estado.

```react
// Correcto
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```

> Acepta una función en lugar de un objeto. Esa función recibirá el estado previo como primer argumento, y las props en el momento en que se aplica la actualización como segundo argumento



**Los datos fluyen hacia abajo**

Ni los componentes padres o hijos pueden saber si un determinado componente tiene o no tiene estado y no les debería importar si se define como una función o una clase.

Por eso es que el estado a menudo se le denomina local o encapsulado. No es accesible desde otro componente excepto de aquel que lo posee y lo asigna.

**Un componente puede elegir pasar su estado como props a sus componentes hijos.** A esto comúnmente se le llama flujo de datos «descendente» o «unidireccional». Cualquier estado siempre es propiedad de algún componente específico, y cualquier dato o interfaz de usuario derivados de ese estado solo pueden afectar los componentes «debajo» de ellos en el árbol.

Si imaginas un árbol de componentes como una cascada de props, **el estado de cada componente es como una fuente de agua adicional que se le une en un punto arbitrario, pero también fluye hacia abajo.**





## Proyecto



**Declaramos el estado**

```react
import React, { Component } from 'react';

class App extends Component {
constructor(){
  super()
  this.state = {
    usuarios: [
      {
        nombre: 'Carlos',
        correo: 'carlos.brignardello@gmail.com',
        enlace: 'carlosbrignardello.com'
      },
      {
        nombre: 'Damaris',
        correo: 'damaris.bejar@gmail.com',
        enlace: 'damarisbejar.com'
      },
      {
        nombre: 'Riter',
        correo: 'Riter.dragon@gmail.com',
        enlace: 'Riterdragon.com'
      }
    ]
  }
}
```

> Con la función propia de los componentes denominada `constructor()` podemos generar unos estados base.



**Utilizar estados**

```react
	ponerFilas = () => (
    this.state.usuarios.map((usuario) => (
      <tr>
			<td>
				{ usuario.nombre }
			</td>
			<td>
				{ usuario.correo }
			</td>
			<td>
				{ usuario.enlace }
			</td>
		</tr>
    ))
  )
...
```

> Mediante el uso del método `map()` podemos generar contenido de acuerdo a la cantidad de estados y acceder a ellos según su nombre.



**Asignar un estado**

```react
    handleChange = (e) =>{
        /*console.log({ 
            name: e.target.name,
            value: e.target.value });
        */
        this.setState({firstName: e.target.value}) // 1. 
    }
```

> 1. Al escribir información esta será guardada en estado, esta información podrá ser vista con la herramientas de React.



**Definimos el formulario**

```react
render(){
    return (
        <div>
            <h1>New Attendant</h1>
            <form onSubmit={this.handleSubmit}>
                <div className="form-group">
                    <label>First Name</label>
                    <input  onChange={this.handleChange} className="form-control" type="text" name="firstName"/>
                </div>
                <div className="form-group">
                    <label>Last Name</label>
                    <input  onChange={this.handleChange} className="form-control" type="text" name="lastName"/>
                </div>
                <div className="form-group">
                    <label>Email</label>
                    <input  onChange={this.handleChange} className="form-control" type="email" name="email"/>
                </div>
                <div className="form-group">
                    <label>Job Title</label>
                    <input  onChange={this.handleChange} className="form-control" type="text" name="jobTitle"/>
                </div>
                <div className="form-group">
                    <label>Twitter</label>
                    <input  onChange={this.handleChange} className="form-control" type="text" name="twitter"/>
                </div>
                <button onClick={this.handleClick} className="btn btn-primary">Save</button>
            </form>
        </div>
    	)
	}
}
```



**Estado dinámico**

```react
    handleChange = (e) =>{
        this.setState({[e.target.name]: e.target.value})
    }
```

> Para guardar la información en el estado se usa una función de la clase component llamada setState a la cual se le debe pasar un objeto con la información que se quiere guardar.



**Manejo de estado**

> Cada input guarda su propio valor y al tiempo la está guardando en setState, lo cual no es ideal. Para solucionarlo hay que modificar los inputs de un estado de no controlados a controlados mediante un valor.

```react
import React from 'react'

class BadgeForm extends React.Component{
    state = {
        firstName: "Carlos",
    }; // REQUERIDO - Inicializamos el estado, puede ser o no vacio.

    handleChange = (e) =>{
        this.setState({[e.target.name]: e.target.value})
    }

    handleClick = e =>{
        console.log("Button was clicked")
    }

    handleSubmit = e =>{
        e.preventDefault();
        console.log("Form was submitted");
        console.log(this.state); // Mostramos todos los estados al enviar.
    }

...
	<div className="form-group">
        <label>First Name</label>
        <input  onChange={this.handleChange} 
            className="form-control" 
            type="text" 
            name="firstName"
        	value={this.state.firstName}/> // Asignamos el estado mediante un valor, para controlarlos.
    </div>
...
```



### Levantamiento del estado

A continuación se modificada el proyecto para que al escribir sobre un componente se vean reflejados los cambios en otros componentes. Levantar el estado es moverlos como props hacia otros componentes. La idea es mover la información en un lugar donde sea compartida por más componentes por ende será movida hacia la página (BadgeNew) que aglomera nuestros componentes.



**Pasar información a otro componente**

```react
class BadgeNew extends React.Component {

    state = {
        form: {} // Guardamos los estados en forma ordenada dentro de "form".
    };

    handleChange = e =>{ // Creamos un método que recibira el evento y lo guardara como valor.
        this.setState({
            // La información en form sera siempre sobre escrita.
            form: { 
                [e.target.name]: e.target.value,
            }
        })
    }

    render(){
        return(
...
            // Asociamos el método al componente.
            <div className="col-6">
                <BadgeForm onChange={this.handleChange} /> 
            </div> 
...
```



```react
class BadgeForm extends React.Component{

    // state = {};

    /*handleChange = (e) =>{ 
        this.setState({[e.target.name]: e.target.value})
    }
    */


...
    render(){
        return (
...
            // Modificamos onChange para asociarlo al método generado en BadgeNew.
            <div className="form-group">
                <label>First Name</label>
                <input  onChange={this.props.onChange} 
                    className="form-control" 
                    type="text" 
                    name="firstName"
                    value={this.state.firstName}/>
            </div>
...
```

Actualmente podremos recibir la información del estado de en el componente BadgeNew sin embargo la información es sobre escrita, es decir: solo puede manejar una información a la vez.



**Solucionar sobre escritura de información**

```react
    handleChange = e =>{
        const nextForm = this.state.form
        nextForm[e.target.name] = e.target.value;
        this.setState({
            form: nextForm
        })
    }
```

o

```react
    handleChange = e =>{
        this.setState({
            form:{
                ...this.state.form,
                [e.target.name]: e.target.value,
            }
        })
    }
```



**Pasar valores de la página al componente**

```react
class BadgeNew extends React.Component {
    state = { // 1. 
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
        return(
...
            <div className="col-6">
                <BadgeForm onChange={this.handleChange} formValues={this.state.form} /> // 2.
            </div>
...
```

> 1. Debemos pre definir los valores del formulario.
> 2. Le pasamos los valores del formulario de la página al componente.



```react
class BadgeForm extends React.Component{

    // state = {};
...
    render(){
        return (
...
            <div className="form-group">
                <label>First Name</label>
                <input  onChange={this.props.onChange} 
                    className="form-control" 
                    type="text" 
                    name="firstName"
                    value={this.props.formValues.firstName}/> // 1.
            </div>
...
```

> 1. Asignamos los valores del formulario provenientes de la página.



```react
class BadgeNew extends React.Component {
...
    render(){
        return(
...
            <div className="col-6">
                <Badge // 1.
                    firstName={this.state.form.firstName} 
                    lastName={this.state.form.lastName} 
                    jobTitle={this.state.form.jobTitle} 
                    twitter={this.state.form.twitter}
                    email={this.state.form.email}/>
            </div>
...
```

> 1. Transferimos los valores a otro componente para que se vean en tiempo real.



Con este método tenemos la información del formulario compartida para los componentes de nuestra página. El estado de "BadgeForm" fue movido a "BadgeNew" quien a su vez transmite su información devuelta al formulario y a "Badge".

**Resultado**

![](\images\BadgeNew.png)



### Generar componentes desde cliente

* Generamos una nueva página denominada Badges.

```react
import React from 'react'
import Navbar from '../components/Navbar'
import './styles/Badges.css'
import confLogo from '../images/badge-header.svg'
import BadgesList from '../components/BadgesList'
export class Badges extends React.Component {

    state = {
        data: [
          {
            id: '2de30c42-9deb-40fc-a41f-05e62b5939a7',
            firstName: 'Freda',
            lastName: 'Grady',
            email: 'Leann_Berge@gmail.com',
            jobTitle: 'Legacy Brand Director',
            twitter: 'FredaGrady22221-7573',
            avatarUrl:
              'https://www.gravatar.com/avatar/f63a9c45aca0e7e7de0782a6b1dff40b?d=identicon',
          },
          {
            id: 'd00d3614-101a-44ca-b6c2-0be075aeed3d',
            firstName: 'Major',
            lastName: 'Rodriguez',
            email: 'Ilene66@hotmail.com',
            jobTitle: 'Human Research Architect',
            twitter: 'MajorRodriguez61545',
            avatarUrl:
              'https://www.gravatar.com/avatar/d57a8be8cb9219609905da25d5f3e50a?d=identicon',
          },
          {
            id: '63c03386-33a2-4512-9ac1-354ad7bec5e9',
            firstName: 'Daphney',
            lastName: 'Torphy',
            email: 'Ron61@hotmail.com',
            jobTitle: 'National Markets Officer',
            twitter: 'DaphneyTorphy96105',
            avatarUrl:
              'https://www.gravatar.com/avatar/e74e87d40e55b9ff9791c78892e55cb7?d=identicon',
          },
        ],
      };

    render() {
        return (
            <div>
                <Navbar/>
                <div className="Badges">
                    <div className="Badges__hero">
                        <div className="Badges__container">
                            <img className="Badges_conf-logo" src={confLogo} alt="Conf Logo" />
                        </div>
                    </div>
                </div>

                <div className="Badge__container">
                    <div className="Badges__buttons">
                        <a href="/badges/new" className="btn btn-primary" >New Badge</a>
                    </div>
                </div>

                <div className="Badges__list">
                    <div className="Badges__container">
                        <BadgesList badges={this.state.data}/>
                    </div>
                </div>
            </div>
        )
    }
}

export default Badges

```



* Generamos otro componente denominado BadgesList

```react
import React from 'react';

import './styles/BadgesList.css';

class BadgesListItem extends React.Component {
  render() {
    return (
      <div className="BadgesListItem">
        <img
          className="BadgesListItem__avatar"
          src={this.props.badge.avatarUrl}
          alt={`${this.props.badge.firstName} ${this.props.badge.lastName}`}
        />

        <div>
          <strong>
            {this.props.badge.firstName} {this.props.badge.lastName}
          </strong>
          <br />@{this.props.badge.twitter}
          <br />
          {this.props.badge.jobTitle}
        </div>
      </div>
    );
  }
}

class BadgesList extends React.Component {
  render() {
    return (
      <div className="BadgesList">
        <ul className="list-unstyled">
          {this.props.badges.map(badge => {
            return (
              <li key={badge.id}>
                <BadgesListItem badge={badge} />
              </li>
            );
          })}
        </ul>
      </div>
    );
  }
}

export default BadgesList;
```



**Resultado**

![](\images\BadgesList.png)