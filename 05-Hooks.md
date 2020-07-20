# Hooks







### Custom Hook

Los custom hooks permiten ampliar el uso de los hooks pre definidos para volver los componentes más poderosos y re utilizables. En un Custom Hook se utilizan todos los hooks convenciales utilizados en un componente y sus funciones como pueden ser los handlers.



**Hook para manipular valores**

```jsx
import { useState } from 'react'

export const useCounter = () => {

  const [state, setstate] = useState(0)
  const increment = ( factor = 1) => {
    setstate( state + factor )
  }
  const decrement = (factor = 1) => {
    setstate( state - factor )
  }
  const reset = () => {
    setstate( 0 )
  }

  return {
    state,
    increment,
    decrement,
    reset
  }
}

// Uso

  const {state: counter, increment, decrement, reset } = useCounter()

  return (
    <>
      <h1>Counter with Hook: { counter }</h1>

      <button 
        className="btn btn-primary"
        onClick={ () => increment(5) }
      >+5</button>
      <button 
        className="btn btn-secondary"
        onClick={ reset }
      >Reset</button>
      <button 
        className="btn btn-danger"
        onClick={ () => decrement(5) }
      >-5</button>
    </>
  )
```



**Hook para manejar estado de formulario**

```js
import { useState } from "react"

export const useForm = ( initialState = {} ) => {

  const [values, setValues] = useState(initialState)

  const handleInputChange = ({ target }) => {
    setValues({
      ...values, [target.name] : target.value
    })
  }

  return [ values, handleInputChange ]
}


// uso

  const [ formValues, handleInputChange ] = useForm({
    name: '',
    email: ''
  })

  const { name, email } = formValues


  return (
      ...
      <div className="form-group">
        <input 
          type="text"
          name="name"
          className="form-control"
          placeholder="Tu nombre"
          value = { name }
          onChange={ handleInputChange }
        />
      
      </div>
      <div className="form-group">
        <input 
          type="text"
          name="email"
          className="form-control"
          placeholder="carlos.alb.brig@gmail.com"
          value = { email }
          onChange={ handleInputChange }
      />
	  ...
```

> Este Hook devuelve el valor del estado y una función para cambiarlo automáticamente cada vez que exista un cambio.



```js
import { useState, useEffect } from "react"

export const useFetch = ( url ) => {

  const [state, setstate] = useState({ data: null, loading: true, error: null})

  useEffect( () => {
    fetch( url )
    .then( resp => resp.json() )
    .then( data => {
      setstate({
        loading: false,
        error: null,
        data
      })
    })
  }, [ url ])

  return state

}

```



### Use Effect

Permite realizar un efecto secundario cuando suceda algo especifico en el componente. Y también permite desmontar ciertos comportamientos o funciones del componente.



**Accionar solo al renderizar**

```js
  useEffect(() => {
    console.log('Efecto al renderizar')
  }, [])
```

> Este es el uso más recurrente de todos.



**Accionar cuando cambie un estado**

```js
// Efecto al renderizar
  useEffect(() => {
    console.log('Efecto al renderizar')
  }, [])
// Efecto al cambiar un estado
  useEffect(() => {
    console.log('Form State cambio')
  }, [formState])
```

> En este caso cada vez que se modifique el estado "**formState**" se accionara el Efecto.



**Renderizado condicional**

Los Hooks no pueden utiizarse en un componente de forma condicional, es decir que este dentro de un `if` y que se dispare cuando algo ocurra. Siempre deben estar declarados lo más arriba posible.

```jsx
// Componente condicional
  useEffect(() => {
    console.log('El componente se monta')
    return () => {
      console.log('Componente desmontado')
    }
  }, [])

  return (
    <>
     <h3>Hola!</h3> 
    </>
  )
}

// En otro componente
        { (name === '123') && <Message /> }
```

> En el momento en que se cumple la condición de que **name** sea `123` el componente se renderiza y ejecuta el primer mensaje de consola. Y en el momento en que la condición se deje de cumplir el componente se desmonta y ejecuta el otro mensaje de consola.
>
> El desmontar componentes ayuda a limpiar la memoria de la aplicación.



**Desempeño y consumo de memoria**

Cuando se cumple una condición y se carga un efecto, si no es limpiado ese proceso puede ocurrir que al desmontar y montar un efecto se realice múltiples veces una cadena de peticiones, lo que volvería extremadamente lenta la página.

```js

  useEffect(() => {

    const mouseMove = () => {
      console.log('PROCESO')
    }

    window.addEventListener('mousemove', mouseMove)

    return () => {
      window.removeEventListener('mousemove', mouseMove)
    }
  }, [])

```

> Lo que hacemos al retornar es ejecutar una orden al desmontar el componente. En este caso eliminar el listener.



### useRef

Permite manejar una referencia de un elemento HTML dentro de una página para darle seguimiento.

https://www.udemy.com/course/react-cero-experto/learn/lecture/19828012#content

Se puede utilizar para asegurar que un componente este montado.



### memo

Se utiliza para memorizar algo, por ejemplo si se llama una función que ya se cargo y no ha cambiado su contenido o sus propiedades, no la vuelve a llamar a no ser que exista un cambio.



### useMemo

Si una variable y una función cambian se ejecuta un proceso, es lo mismo que antes pero con variables o funciones.



### useCallback