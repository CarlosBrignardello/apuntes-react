### Ciclo de vida de un componente

Cuando se renderiza un componente se dice que este es **montado**, cuando su estado cambia o recibe props distintos se **actualiza** y cuando se cambia de página el componente se **desmonta**.



**Montaje**

Representa el momento en que se inserta el código del componente en el DOM. Son llamados tres métodos: **constructor**, **render** y **componentDidMount**.

* **constructor**: inicializa valores.
* **render**: introduce el elemento que representa un componente en el código.
* **componentDidMount**: es llamado cuando el componente ya es visible en el DOM.



**Actualización**

Ocurre cuando los props o el estado del componente cambian. Son llamados dos métodos: **render** y **componentDidUpdate**.

* **componentDidUpdate**: es un método que recibe dos argumentos(props_anteriores y estado_anterior), esto sirve para comparar con la versión anterior con la actual.



**Desmontaje**

Ocurre cuando un componente deja de estar presente y es limpiado. Se llama un método denominado **componentWillUnmount**.

* **componentWillUnmount**: 



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

