# Cargar lista de mensajes

[capitulo anterior](Chapter_08.md) <----> [capitulo siguiente](Chapter_10.md) | [inicio](README.md)

Un vez tenemos conectado nuestro servidor de GraphQl, podemos empezar a ejecutar queries para obtener los datos que necesitamos, seguiremos haciendo uso de React Hooks, pero esta vez con los hooks propios de Apollo.

Lo primero que vamos a hacer, es declarar el **query** con el cual vamos a obtener la lista de chats, para esto vamos a ir al componente `ChatBody` y agregar lo siguiente:

```javascript
const GET_CHATS = gql`
  query ChatsQuery {
    chats {
      id
      from
      fromName
      message
    }
  }
`;
```

La librería `graphql-tag` nos permite "parsear" un query de tal forma que pueda ser usado por ApolloClient para obtener datos del servidor de GraphQl, en este caso la lista de chats, y por cada chat los atributos `id`, `from`, `fromName` y `message`.

Vamos a actualizar el componente `ChatBody` de la siguiente manera:

```javascript
function ChatBody() {
  const { state } = useContext(Store);
  const chatBodyClassNames = [styles["chat-body"], "elevation-box-inset-right"];
  const { data, loading } = useQuery(GET_CHATS);
  const chatList = data.chats;

  return (
    <section className={chatBodyClassNames.join(" ")}>
      {loading
        ? "Cargando mensajes..."
        : chatList.map(chat => (
            <Message
              key={chat.id}
              fromName={chat.fromName}
              message={chat.message}
              isOwn={chat.from === state.user.id}
            />
          ))}
    </section>
  );
}
```

El hook `useQuery` nos permite lanzar la petición a nuestro servidor de GraphQl para ejecutar el **query** y obtener los datos solicitados de vuelta, adicionalmente nos permite validar el estado de la petición por medio de la propiedad `loading` la cual podremos usar para renderizar los componentes de forma condicional.
