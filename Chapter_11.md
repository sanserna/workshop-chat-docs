# Actualizar mensajes en tiempo real

[capítulo anterior](Chapter_10.md) | [inicio](README.md)

Finalmente lo único que esta pendiente es actualizar la lista de mensajes sin necesidad de refrescar el navegador, para esto haremos uso de las **suscripciones**, las cuales permiten mantener un canal abierto con el servidor de GraphQl para estar atento a mensajes nuevos y de esa forma efectuar actualizaciones sobre el UI.

Vamos a actualizar el componente `ChatBody` y agregar lo siguiente:

```javascript
const CHATS_SUBSCRIPTION = gql`
  subscription OnNewMessage {
    messageSent {
      id
      from
      fromName
      message
    }
  }
`;
```

El **type subscription** nos permite establecer el "evento" al que nos queremos suscribir, en este caso el evento `OnNewMessage`, el cual se va a disparar cada vez que un usuario agregue un mensaje nuevo. Adicionalmente, debemos actualizar el componente `ChatBody` para hacer uso de la **suscripción** agregando lo siguiente:

```javascript
const { data: subscriptionData } = useSubscription(CHATS_SUBSCRIPTION);
```

Hacemos uso del hook `useSubscription`, el cual actualiza nuestro componente en tiempo real cada vez que se realiza una actualización desde el servidor de GraphQl, en caso de que se cree un mensaje nuevo, lo agregamos a la lista de mensajes existente.

```javascript
if (subscriptionData) {
  chatList.push(subscriptionData.messageSent);
}
```

Para finalizar vamos a realizar unos ajustes en el componente `ChatBody` para que el scroll se ajuste cada vez que se agrega un nuevo mensaje, la versión final del componente es la siguiente:

```javascript
function ChatBody() {
  const { state } = useContext(Store);
  const chatBodyClassNames = [styles["chat-body"], "elevation-box-inset-right"];
  const { data, loading } = useQuery(GET_CHATS);
  const { data: subscriptionData } = useSubscription(CHATS_SUBSCRIPTION);
  const chatList = data.chats;
  const elementRef = useRef(null);

  if (subscriptionData) {
    chatList.push(subscriptionData.messageSent);
  }

  useEffect(() => {
    elementRef.current.scrollTop = elementRef.current.scrollHeight;
  }, [data, subscriptionData]);

  return (
    <section className={chatBodyClassNames.join(" ")} ref={elementRef}>
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

[capítulo anterior](Chapter_10.md) | [inicio](README.md)
