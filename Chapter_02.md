# API de GraphQl que usaremos en nuestra aplicación

[capítulo anterior](Chapter_01.md) <----> [capítulo siguiente](Chapter_03.md) | [inicio](README.md)

La intención de este curso es cubrir conceptos básicos de GraphQl de lado del cliente, a continuación daremos un breve introducción a el API de GraphQl que estaremos usando para poder conectar nuestro front-end, así como también veremos los fundamentos principales que componen una aplicación con GraphQl.

## Que es GraphQl?

GraphQL es un lenguaje de consultas, el cual tiene como objetivo ofrecer a los clientes una forma más directa, sencilla y eficiente para obtener exactamente los datos que requieren.

- GraphQl no es una librería o framework: es una especificación open source creada por Facebook, existen implementaciones de la misma en lenguajes como JavaScript, Python, Scala, Java etc...
- GraphQl le da el poder al cliente de describir que data se quiere de forma exacta.
- GraphQl es fuertemente tipada.
- GraphQl puede con existir con un API existente, ya que es solo un lenguaje de consultas, que define como es la interacción con el API.

## GraphQl vs REST

- GraphQl tiene un único endpoint y las peticiones siempre son de tipo POST (algunas veces GET).
- En REST, la forma y el tamaño de los datos esta determinada por el servidor, con GraphQl esta determinada por la consulta (query).
- En REST es necesario hacer múltiples peticiones al servidor para obtener datos relacionados, con GraphQl se puede tener un recurso de entrada y obtener todas sus relaciones en una sola petición.
- En REST, una petición ejecuta un único controlador dentro del servidor, con GraphQl una petición puede ejecutar múltiples resolvers.

## Conceptos básicos de GraphQl

A continuación veremos de que forma podemos interactuar con nuestro API de GraphQl.

### Scalar y Object types

En GraphQl los types envuelven muchas cosas, cuando hablamos de "tipos" de datos tenemos dos tipos básicos, **scalar types** y **object types**.

- Scalar types
  - String
  - Int
  - Float
  - Boolean
  - ID
- Object types.

```graphql
type Chat {
  id: Int!
  from: ID!
  fromName: String!
  message: String!
}
```

Un **object type** puede estar compuesto por **scalar types**.

### Query

Un **query** es básicamente una consulta que hacemos a nuestro servidor, especificando que datos queremos que nos envíe.

```graphql
query Query {
  chats {
    id
    from
    fromName
    message
  }
}
```

Este es el **query** que estaremos utilizando para pedir la lista completa de chats, como podemos ver, es el **query** quien define cual es la estructura que se quiere obtener.

### Mutation

Las **mutaciones** básicamente nos permiten efectuar acciones tales como crear, modificar o eliminar datos. Una mutación es similar a una función: recibe ciertos parámetros, realiza un cambio y devuelve una respuesta.

```graphql
mutation SendMessage($message: String!, $from: ID!, $fromName: String!) {
  sendMessage(from: $from, fromName: $fromName, message: $message) {
    id
  }
}
```

Como podemos ver en el ejemplo, esta mutación nos permite crear un nuevo mensaje, enviando como parámetros los datos necesarios para crearlo, las mutaciones siempre devuelven algo, en este caso un `Chat`, por lo cual tenemos que definir que datos queremos obtener del chat al efectuar la mutación.

### Subscription

Las **suscripciones** como su nombre lo dice, nos permiten suscribirnos a cambios que ocurran en el servidor, en el caso de nuestro ejemplo, podemos suscribirnos a nuevos mensajes dentro de nuestro chat y efectuar acciones en consecuencia de eso.

```graphql
subscription OnMessage {
  messageSent {
    id
    fromName
  }
}
```

Con esta suscripción podemos enterarnos, por ejemplo, de cuando algún usuario crea un nuevo mensaje dentro del chat, adicionalmente estamos especificando que queremos obtener el `id` y `fromName` del mensaje que disparo la acción.

### Schema

Al momento de crear un API con GraphQl, es necesario definir la forma en la que el cliente puede interactuar con los datos, esto se hace por medio de **schemas**, la definición de un **schema** puede contener **types** de tipo **Object type**, **query**, **mutation** y **subscription**.

```graphql
type Chat {
  id: Int!
  from: ID!
  fromName: String!
  message: String!
}

type Query {
  chats: [Chat]
}

type Mutation {
  sendMessage(from: ID!, fromName: String!, message: String!): Chat
}

type Subscription {
  messageSent: Chat
}
```

En el siguiente [link](https://github.com/sanserna/graphql-chat-server) se encuentra el repositorio con la implementación del API de GraphQl con el que vamos a conectar nuestra aplicación.

[capítulo anterior](Chapter_01.md) <----> [capítulo siguiente](Chapter_03.md) | [inicio](README.md)
