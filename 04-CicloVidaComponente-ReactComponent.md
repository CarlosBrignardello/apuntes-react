# Ciclo de vida de un componente

Cada componente tiene varios “métodos de ciclo de vida” que puedes sobrescribir para ejecutar código en momentos particulares del proceso. Cuando se renderiza un componente se dice que este es **montado**, cuando su estado cambia o recibe props distintos se **actualiza** y cuando se cambia de página el componente se **desmonta**.

En aplicaciones con muchos componentes, es muy importante liberar recursos tomados por los componentes cuando se destruyen.



**MONTAJE**

Representa el momento en que se inserta el código del componente en el DOM. Son llamados tres métodos: **constructor**, **render** y **componentDidMount**.

* **constructor**: .....

* **render()**: Se trata del **único método obligatorio en un componente**. Introduce el elemento que representa un componente en el código. Cuando se llama, debe examinar a `this.props` y `this.state` y devolver uno de los siguientes tipos:

  - **Elementos de React**: normalmente creados a través de JSX. Por ejemplo, `<div />` y `<Component />` son elementos de React que enseñan a React a renderizar un nodo DOM, u otro componente definido por el usuario, respectivamente.
  - **Arrays y fragmentos**: permiten que puedas devolver múltiples elementos desde el render.
  - **Portales**: te permiten renderizar hijos en otro subárbol del DOM.
  - **String y números**: estos son renderizados como nodos de texto en el DOM.
  - **Booleanos o nulos**: no renderizan nada.

  La función `render ()` debe ser pura, lo que significa que no modifica el estado del componente, devuelve el mismo resultado cada vez que se invoca y no interactúa directamente con el navegador.

  Si necesitas interactuar con el navegador, realiza tu trabajo en `componentDidMount()` o en los demás métodos de ciclo de vida.

* **componentDidMount**: es llamado cuando el componente ya es visible en el DOM, directamente cuando un componente es montado. Es el lugar ideal para establecer cualquier suscripción. Si se hace es importante darla de baja en `componentWillUnmount()`.

  **Puedes llamar `setState()` inmediatamente** en `componentDidMount()`. Se activará un renderizado extra, pero sucederá antes de que el navegador actualice la pantalla. 



**ACTUALIZACIÓN**

Ocurre cuando los props o el estado del componente cambian. Estos métodos se llaman en el siguiente orden cuando un componente se vuelve a renderizar.

* **render()**: .....
* **componentDidUpdate**: es un método que recibe dos argumentos(props_anteriores y estado_anterior), esto sirve para comparar con la versión anterior con la actual.



**DESMONTAJE**

Ocurre cuando un componente deja de estar presente y es limpiado. Se llama un método denominado **componentWillUnmount**.

* **componentWillUnmount()**: es invocado inmediatamente antes de desmontar y destruir un componente. En este método se deben realizar las tareas de limpieza como: invalidación de temporizadores, cancelación de solicitudes, eliminar suscripciones creadas en `componentDidMount()`.

  **No debes llamar `setState()`** en `componentWillUnmount()` porque el componente nunca será vuelto a renderizar.



# React.Component

React te permite definir componentes como clases o funciones. Los componentes definidos como clases actualmente proporcionan una serie de características extra.

* **constructor(props)**: El constructor para un componente es llamado antes de ser montado. Al implementarlo se debería llamar a `super(props)` antes que cualquier otra instrucción. De otra forma `this.props` no estará definido en el constructor.

  Los constructores sólo **se utilizan para dos propósitos**:

  * **Inicializar un estado local** asignado a un objeto al `this.state`.
  * Enlazar **manejadores de eventos** a una instancia. 

  **NO debes llamar a `setState()` en el `constructor()`.** En su lugar, si el componente necesita usar un estado local, asigna directamente el estado inicial al `this.state` directamente en el constructor.

  ```react
  constructor(props) {
    super(props);
    // No llames this.setState() aquí!
    this.state = { 
        counter: 0 
    };
    this.handleClick = this.handleClick.bind(this);
  }
  ```

  > El constructor es el único lugar donde debes asignar `this.state` directamente. En todos los demás métodos, debes usar `this.setState()` en su lugar.

  

**Otras APIs**

A diferencia de los métodos de ciclo de vida anteriores (que React llama por ti), los métodos siguientes son los métodos *que* tu puedes llamar desde tus componentes.

* **setState()**: hace cambios al estado del componente y le dice a React que este componente y sus elementos secundarios deben volverse a procesar con el estado actualizado. Este es el método principal que utiliza para actualizar la interfaz de usuario en respuesta a los manejadores de eventos y las respuestas del servidor. 

  No siempre actualiza inmediatamente el componente. Puede procesar por lotes o diferir la actualización hasta más tarde. Esto hace que leer `this.state` después de llamar a `setState()` sea una trampa potencial. En su lugar, usa `componentDidUpdate` o un callback `setState` (`setState(updater, callback)`), se garantiza que cualquiera de los dos se activará una vez la actualización haya sido aplicada.

  

* **forceUpdate()**:



**Propiedades de clase**

* **defaultProps**:
* **displayName**:



**Propiedades de instancia**

* **props**:
* **estado**:



## Proyecto

**Ejemplo con Badge.js**

```react
import React from 'react'
import './styles/Badges.css'
import confLogo from '../images/badge-header.svg'
import BadgesList from '../components/BadgesList'
import {Link} from  'react-router-dom'

export class Badges extends React.Component {

      constructor(props){ // 1.
          super(props)
          console.log('1. constructor()')

          this.state = {
            data: [],
          };
      }

      componentDidMount(){
          console.log('3. componentDidMount()') // 2.

          this.timeoutId = setTimeout(() => {
              this.setState({
                  data:[
                    {
                      id: '2de30c42-9deb-40fc-a41f-05e62b5939a7',
                      firstName: 'Freda',
                      .........]
              })
          }, 3000)
      }

      componentDidUpdate(prevProps, prevState){ // 3.
          console.log('5. componentDidUpdate()')
          console.log({
              prevProps: prevProps, prevState: prevState
          })
          console.log({
              props: this.props, state: this.state
          })
      }

      componentWillUnmount() { // 4.
          console.log('6. componentWillUnmount()')
          clearTimeout(this.timeoutId)
      }
      

    render() { // 5.
        console.log('2/4. render()')
        return (
            <div>
                <div className="Badges">
                    <div className="Badges__hero">
                        <div className="Badges__container">
                            <img className="Badges_conf-logo" src={confLogo} alt="Conf Logo" />
                        </div>
                    </div>
                </div>

                <div className="Badge__container">
                    <div className="Badges__buttons">
                        <Link to="/badges/new" className="btn btn-primary" >New Badge</Link>
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

> 1. Generamos un constructor donde declaramos todos los datos que en este caso estarán vacíos, será lo primero que se ejecutara en la página, para poder observarlo por consola lo marcamos.
> 2. Agregamos el método `componentDidMount()` en el cual agregaremos un tiempo de espera de 3s que simulara una conexión a una API para obtener los datos, la idea es que esto actualice el estado del `render()`. 
>    * Si durante el proceso de espera nos dirigimos a otra página el componente será desmontado sin embargo el tiempo de espera y la actualización al componente se ejecutaran de todas formas lo que generara un error, para evitarlo obtenemos el valor de la función la cual procederemos a limpiar más adelante en el método de desmontaje del componente para evitar errores.
> 3. Al actualizarse el render se activara el método `componentDidUpdate()` adicionalmente mostraremos los datos previos y los datos actuales para poder compararlos.
>    * En este caso al comparar los datos veremos que en los datos previos no existen datos en data y en los datos actuales SI.
> 4. En el método de desmontar el componente colocamos un marcador para verlo por consola y adicionalmente limpiamos el tiempo de espera en caso de que hubiese uno en ejecución para evitar errores.
> 5. Cuando carga el componente y cuando es actualizado se muestra nuestro marcador de `render()`.
>
> 
>
> **El resultado al visitar la página por consola es el siguiente:**
>
> 1.**constructor()**
>
> 2/4. **render()**
>
> 3.**componentDidMount()**
>
> 2/4. **render()**
>
> 5.**componentDidUpdate()**
>
> {prevProps: {…}, prevState: {…}}
> {props: {…}, state: {…}}
>
> 
>
> **El resultado al visitar la página por consola y pasar a otra página antes de los 3s es el siguiente**:
>
> 1.**constructor()**
>
> 2/4. **render()**
>
> 3.**componentDidMount()**
>
> 6.**componentWillUnmount()**

