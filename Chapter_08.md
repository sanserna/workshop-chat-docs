# Conectar ApolloClient

[capítulo anterior](Chapter_07.md) <----> [capítulo siguiente](Chapter_09.md) | [inicio](README.md)

Como se menciono en un principio, haremos uso de la librería [ApolloClient](https://www.apollographql.com/docs/react/) para poder conectarnos con el API de GraphQl, existen algunas otras librerías que hacen el trabajo mucho mas fácil al momento de usar Apollo, como por ejemplo [apollo-boost](https://github.com/apollographql/apollo-client/tree/master/packages/apollo-boost), pero el objetivo de este taller es poder entender un poco mas a fondo lo necesario para conectar un servidor de GraphQl.

## Instalar las dependencias necesarias

```bash
npm install --save @apollo/react-hooks apollo-cache-inmemory apollo-client apollo-link apollo-link-http apollo-link-ws apollo-utilities graphql graphql-tag subscriptions-transport-ws
```

## Configurar ApolloClient

Vamos a actualizar el archivo `src/index.js` de la siguiente manera:

```javascript
import { InMemoryCache } from "apollo-cache-inmemory";
import { ApolloClient } from "apollo-client";
import { split } from "apollo-link";
import { HttpLink } from "apollo-link-http";
import { WebSocketLink } from "apollo-link-ws";
import { getMainDefinition } from "apollo-utilities";
import { ApolloProvider } from "@apollo/react-hooks";

// Endpoint donde se encuentra corriendo el servidor de GraphQl
const SERVER_ENDPOINT = "localhost:4000";

// Nos permite crear un link para obtener resultados de un servidor de GraphQl
// por medio de una conexion http
// https://www.apollographql.com/docs/link/links/http/
const httpLink = new HttpLink({ uri: `http://${SERVER_ENDPOINT}/graphql` });

// Nos permite crear un link por el cual nos podremos suscribir a las
// actualizaciones del servidor de GraphQl por medio del protocolo
// ws (WebSockets)
// https://www.apollographql.com/docs/link/links/ws/
const wsLink = new WebSocketLink({
  uri: `ws://${SERVER_ENDPOINT}/subscriptions`,
  options: {
    reconnect: true
  }
});

// La capacidad de dividir enlaces nos permite enviar datos a cada enlace
// dependiendo del tipo de operación que se recibe
const link = split(
  ({ query }) => {
    const { kind, operation } = getMainDefinition(query);
    return kind === "OperationDefinition" && operation === "subscription";
  },
  wsLink,
  httpLink
);

// Cliente con el cual podremos conectarnos al servidor de GraphQl
const apolloClient = new ApolloClient({
  link,
  cache: new InMemoryCache(),
  connectToDevTools: true
});

ReactDOM.render(
  <ApolloProvider client={apolloClient}>
    <StoreProvider>
      <App />
    </StoreProvider>
  </ApolloProvider>,
  document.getElementById("root")
);
```

[capítulo anterior](Chapter_07.md) <----> [capítulo siguiente](Chapter_09.md) | [inicio](README.md)
