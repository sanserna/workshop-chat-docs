# Simulando un Store por medio del React Context API

[capítulo anterior](Chapter_05.md) <----> [capítulo siguiente](Chapter_07.md) | [inicio](README.md)

El concepto de **Store** es muy popular cuando hablamos de "Single Page Applications" (SPA), el objetivo de este patrón, es centralizar el "estado" de nuestra aplicación en un solo lugar, así como también el modelo de datos. En ReactJS existe una librería muy famosa llamada [Redux](http://redux.js.org), la cual nos permite conectar nuestros componentes con un "estado" centralizado, es una librería muy poderosa, la desventaja que tiene es que puede llegar a ser un poco difícil de aprender, especialmente para desarrolladores principiantes.

ReactJS en una de sus mas recientes versiones introdujo el [Context API](https://reactjs.org/docs/context.html), el cual esta diseñado para compartir datos que pueden considerarse "globales" para un árbol de componentes en React como su propia documentación lo dice, una misma aplicación puede tener múltiples "Contextos" cada uno con una responsabilidad especifica, puede existir un contexto que se encargue de controlar los datos y el estado de autenticación del usuario, puede existir otro contexto encargado de controlar la apariencia de la aplicación, puede haber un contexto que se encargue de controlar el estado interno de un componente y la forma en la que interactúan sus componentes descendientes entre ellos, las posibilidades son muchas.

## Los principios de Redux

De acuerdo a la [documentación](https://redux.js.org/introduction/three-principles) Redux puede ser descrito en tres principios básicos, **stores**, **actions** y **reducers**.

1. Las **actiones** son las únicas que deberían desencadenar un cambio en el estado.
2. Un **reducer** especifica que parte de nuestro estado se vera afectada por la acción.
3. El **store** contiene el estado y modelo de datos de nuestra aplicación en un árbol de objetos.

## Como crear el Store

Lo primero que tenemos que hacer, es crear un nuevo archivo llamado `Store.js` en la raíz de nuestra carpeta `src`, acá vamos a hacer uso del React Context API para crear un componente padre que le dará acceso a sus componentes descendientes a la data que contiene.

```javascript
import React from "react";

export const Store = React.createContext();

const initialState = {
  user: {}
};

function reducer() {}

export function StoreProvider({ children }) {}
```

Y esto es todo lo que necesitamos, el React Context API es bastante sencillo de implementar y entender.

En nuestra aplicación haremos uso de este **StoreContext** para almacenar los datos del usuario que ingresa a la sala de chat y así poder tomar acciones en nuestro componentes en base a esto.

A continuación vamos a ver como podemos implementar un flujo que nos permita actualizar los datos del usuario dentro del **store**.

Vamos a actualizar el método `reducer` de la siguiente forma:

```javascript
function reducer(state, action) {
  switch (action.type) {
    case "SET_USER":
      return { ...state, user: action.payload };
    default:
      return state;
  }
}
```

El objetivo de un **reducer** es modificar el estado sin mutar el objeto original, por el contrario, crea una copia del mismo con la modificación efectuada, para la implementación de nuestro flujo, en caso de que la acción disparada sea de tipo `SET_USER`, vamos a devolver una copia del estado reemplazando la propiedad `user` con los datos obtenidos en el **payload** de la acción.

Vamos a modificar el método `StoreProvider` de la siguiente forma:

```javascript
export function StoreProvider({ children }) {
  const [state, dispatch] = React.useReducer(reducer, initialState);
  const value = { state, dispatch };

  return <Store.Provider value={value}>{children}</Store.Provider>;
}
```

Hicimos uso de nuestro primer React Hook, `useReducer`, el cual nos permite hacer uso de un `reducer` para realizar modificaciones sobre el estado, adicionalmente nos entrega un método dispatch, el cual podemos usar para disparar las acciones que desencadenan las modificaciones sobre el estado.

## Hacer uso del StoreProvider

Finalmente, lo único que nos queda hacer es actualizar el archivo `src/index.js` para que nuestra aplicación y todos los componentes descendientes puedan acceder al contexto de nuestro store.

```javascript
import StoreProvider from "./Store";

ReactDOM.render(
  <StoreProvider>
    <App />
  </StoreProvider>,
  document.getElementById("root")
);
```

[capítulo anterior](Chapter_05.md) <----> [capítulo siguiente](Chapter_07.md) | [inicio](README.md)
