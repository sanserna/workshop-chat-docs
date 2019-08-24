# Crear el UI de la aplicación

[capítulo anterior](Chapter_04.md) <----> [capítulo siguiente](Chapter_06.md) | [inicio](README.md)

Ahora vamos a empezar a crear la interfaz de nuestra aplicación, considero que es buena practica dejar listo todo lo relacionado a la parte visual antes de de conectarnos con el API de GraphQl.

Se debe tener un buen entendimiento de cuales van a ser aquellos componentes que va a permitir a el usuario interactuar con nuestra aplicación, ya que entre más modulares y genéricos sean nuestros componentes, más fácil será escalar y mantener nuestra aplicación.

> **Importante**: Para agregar estilos a los componentes, es necesario hacer uso de las variables de SASS que se encuentran en nuestro sistema de diseño.

## Componente Chat

Empezaremos por crear el componente principal de nuestra aplicación, este componente será el encargado de mostrar la lista de mensajes y adicionalmente nos permitirá agregar mensajes nuevos.

```
└── Chat
    ├── ChatBody
    │   ├── Message
    │   │   ├── Message.jsx
    │   │   ├── Message.module.scss
    │   │   └── index.js
    │   ├── ChatBody.jsx
    │   ├── ChatBody.module.scss
    │   └── index.js
    ├── Header
    │   ├── Header.jsx
    │   ├── Header.module.scss
    │   └── index.js
    ├── MessageInput
    │   ├── MessageInput.jsx
    │   ├── MessageInput.module.scss
    │   └── index.js
    ├── Chat.jsx
    ├── Chat.module.scss
    └── index.js
```

## Componente UserNameInput

Este componente nos va a permitir ingresar un nombre de usuario o nickname con el que vamos a poder enviar mensajes desde el chat.

```
└── UserNameInput
    ├── UserNameInput.jsx
    ├── UserNameInput.module.scss
    └── index.js
```

Una vez finalicemos la creación de nuestra interfaz, estamos listos para empezar a agregar interactividad a nuestra aplicación.

[capítulo anterior](Chapter_04.md) <----> [capítulo siguiente](Chapter_06.md) | [inicio](README.md)
