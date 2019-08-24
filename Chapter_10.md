# Crear un nuevo mensaje

[capitulo anterior](Chapter_09.md) <----> [capitulo siguiente](Chapter_11.md) | [inicio](README.md)

Ahora que ya estamos cargando la lista de mensajes, es hora de hacer uso de las mutaciones de GraphQl para crear nuevos mensajes, para esto vamos a actualizar el componente `MessageInput`.

```javascript
const SEND_MESSAGE_MUTATION = gql`
  mutation SendMessage($from: ID!, $fromName: String!, $message: String!) {
    sendMessage(from: $from, fromName: $fromName, message: $message) {
      id
      from
      fromName
      message
    }
  }
`;
```

Ahora declaramos el **mutation** que nos permite crear un nuevo mensaje, la mutación espera ciertos parámetros necesarios para crear el mensaje, los cuales serán capturados por medio de la UI, vamos a actualizar el componente `MessageInput` de la siguiente manera:

```javascript
function MessageInput() {
  const { state } = useContext(Store);
  const [sendMessage] = useMutation(SEND_MESSAGE_MUTATION);
  const [message, setMessage] = React.useState("");

  return (
    <div className={styles["message-input"]}>
      <Input
        className={styles["text-input"]}
        value={message}
        onChange={e => setMessage(e.target.value)}
      />
      <Button
        onClick={() => {
          if (!message) return;

          sendMessage({
            variables: {
              message,
              from: state.user.id,
              fromName: state.user.name
            }
          }).then(() => {
            setMessage("");
          });
        }}
      >
        Enviar
      </Button>
    </div>
  );
}
```

Ahora estamos haciendo uso del hook `useMutation`, el cual nos devuelve un método con el nombre del **type mutation** que se declaro previamente, al ejecutar el método `sendMessage`, se envía como parámetro un objeto que contiene el _key_ variables, en donde vamos a agregar los datos necesarios para crear el mensaje.

[capitulo anterior](Chapter_09.md) <----> [capitulo siguiente](Chapter_11.md) | [inicio](README.md)
