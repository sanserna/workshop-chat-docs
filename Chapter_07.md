# Actualizar la aplicación para uso del StoreContext

[capitulo anterior](Chapter_06.md) <----> [capitulo siguiente](Chapter_08.md) | [inicio](README.md)

Ahora vamos a realizar las modificaciones necesarias en nuestra aplicación para poder validar el estado de nuestro usuario, en caso de que no exista ningún usuario activo, mostraremos el componente `UserNameInput`, en caso contrario mostraremos el componente `Chat`.

Vamos a actualizar el componente `App` de la siguiente manera:

```javascript
function App() {
  const { state } = React.useContext(Store);
  const userExists = !!Object.keys(state.user).length;

  return (
    <div className={styles["app-container"]}>
      {userExists ? <Chat /> : <UserNameInput />}
    </div>
  );
}
```

El React Hook `useContext` nos permite suscribir un componente a el estado de cierto contexto, en este caso, a el StoreContext, el hook retorna la propiedad **state**, la cual habíamos declarado en el `value` del StoreProvider y contiene los datos del store, en caso de que se efectúe una actualización sobre el estado del contexto, el componente se actualizara.

Vamos a actualizar el componente `UserNameInput` de la siguiente manera:

```javascript
const ID = () => {
  return `_${Math.random()
    .toString(36)
    .substr(2, 9)}`;
};

function UserNameInput() {
  const { dispatch } = React.useContext(Store);
  const [userName, setUserName] = React.useState("");

  return (
    <div className={styles["user-name-input"]}>
      <strong>Nombre:</strong>
      <Input
        className={styles["text-input"]}
        value={userName}
        onChange={e => setUserName(e.target.value)}
      />
      <Button
        disabled={!userName}
        onClick={() => {
          dispatch({
            type: "SET_USER",
            payload: { id: ID(), name: userName }
          });
        }}
      >
        Go
      </Button>
    </div>
  );
}
```

El React Hook `useContext` adicionalmente nos entrega una propiedad llamada **dispatch**, la segunda propiedad declarada dentro del `value` del StoreProvider, la cual nos permite disparar acciones que desencadenan los cambios en el estado, en este caso la acción que permite actualizar los datos del usuario dentro del **state** con los que capturamos por medio de la interfaz.

Finalmente vamos a actualizar el componente `Chat/Header` para que muestre el nombre ingresado por el usuario.

```javascript
function Header() {
  const { state } = React.useContext(Store);

  return (
    <header className={styles.header}>
      <h2 className={styles.header__title}>Hola {state.user.name}</h2>
      <img src="assets/zemoga-logo.svg" alt="Zemoga" />
    </header>
  );
}
```

[capitulo anterior](Chapter_06.md) <----> [capitulo siguiente](Chapter_08.md) | [inicio](README.md)
