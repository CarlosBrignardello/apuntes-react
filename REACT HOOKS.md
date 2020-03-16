# Hooks



Permiten actualizar el estado sin la necesidad de crear un Class Component.

**Class Component:** Utilizado para cambiar el state.

**Funcional Component:** Utilizado para mostrar el state.

*La cantidad de líneas se reduce con Hooks*



**Sintaxis**:

```js
import React, {userState} from 'react'

const [state, changeState] = useState([])
```



**Beneficio de Hooks:**

* Reducen la cantidad de código.
* Mayor facilidad para implementar reducers, administrar el state y el context.



**Los Hooks se dividen en dos grupos:**

* Básicos
  * **useState**: Permite ver y cambiar el estado.
  * **useEffect**: Permite manejar el ciclo de vida de los componentes.
* Avanzados
  * **useContext**: 
  * **useRef**
  * **useReducer**
  * **useCallback**
  * **useMemo**



