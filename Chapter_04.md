# Crear y configurar el proyecto

[capítulo anterior](Chapter_03.md) <----> [capítulo siguiente](Chapter_05.md) | [inicio](README.md)

Vamos a empezar por crear la estructura básica de nuestro proyecto, en este capítulo, veremos como podemos organizar nuestra aplicación con las mejores practicas.

## Create React App

```bash
npx create-react-app workshop-chat
cd workshop-chat
```

## Configurar nuestra aplicación para poder hacer uso de SASS y SASS modules

```bash
npm install node-sass --save
```

Ahora podemos cambiar el nombre del archivo `src/App.css` a `src/App.scss/`, adicionalmente es necesario actualizar el archivo `src/App.js` para que importe el archivo `src/App.scss/`.

## Configurar esLint

EsLint es una herramienta que nos permite identificar errores en nuestro código, se puede configurar dependiendo del tipo de aplicación en la que se este trabajando, en nuestro caso, vamos a configurarlo para poder trabajar con ReactJS en optimas condiciones.

## Instalar las dependencias necesarias

```bash
npm install --save babel-eslint eslint eslint-config-airbnb eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react eslint-plugin-react-hooks
```

Para que nuestro linter funcione correctamente, vamos a crear un archivo llamado `.eslintrc.json` en la raíz de nuestro proyecto, este es el archivo de configuración de esLint, en donde vamos a agregar lo necesario para que funcione de forma correcta en una aplicación con ReactJS.

```json
{
  "parser": "babel-eslint",
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "plugins": ["jsx-a11y", "react", "react-hooks"],
  "extends": ["airbnb", "eslint:recommended"],
  "env": {
    "browser": true,
    "es6": true
  },
  "settings": {
    "import/resolver": {
      "node": {
        "paths": ["src"]
      }
    }
  },
  "rules": {
    "react/forbid-prop-types": 0
  }
}
```

## Configurando nuestro entorno para trabajar con JavaScript

En la raíz de nuestro proyecto, vamos a crear un archivo llamado `jsconfig.json`, la presencia de este archivo en un directorio, indica que el directorio es la raíz de un proyecto que trabaja con JavaScrip.

```json
{
  "compilerOptions": {
    "baseUrl": "src"
  },
  "include": ["src"]
}
```

La configuración básica que hemos agregado, define que la carpeta `src` es el directorio base, de esta forma podemos hacer uso de rutas absolutas al momento de importar módulos.

## Estructura básica de nuestra aplicación

Como se había mencionado anteriormente, la idea es crear un proyecto con las mejores prácticas, esto incluye la forma en la que organizamos y estructuramos los archivos dentro de nuestro proyecto, a continuación se muestra la configuración básica para poder empezar a crear nuestra aplicación con ReactJS.

```
├── .eslintrc.json
├── .gitignore
├── jsconfig.json
├── package.json
├── public
│   ├── assets
│   └── index.html
└── src
    ├── components
    │   ├── base
    │   └── MyComponent
    │       ├── MyComponent.jsx
    │       ├── MyComponent.module.scss
    │       └── index.js
    ├── design
    │   ├── _colors.scss
    │   ├── _durations.scss
    │   ├── _fonts.scss
    │   ├── _fonts.scss
    │   ├── _sizes.scss
    │   └── index.scss
    ├── pages
    ├── styles
    │   ├── base
    │   └── global.scss
    ├── App.jsx
    ├── App.module.scss
    └── index.js
```

[capítulo anterior](Chapter_03.md) <----> [capítulo siguiente](Chapter_05.md) | [inicio](README.md)
