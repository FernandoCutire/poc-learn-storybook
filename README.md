

## 馃摪 En este art铆culo aprender谩s

1. C贸mo empaquetar una aplicaci贸n en un contenedor docker
2. El desarrollo de un contenedor para una aplicaci贸n JS
3. C贸mo ver los puertos del contenedor
4. Soluci贸n a errores comunes al empaquetar SPA (Single Page Application)
5. Repositorio con c贸digo completo para que puedas [probar la aplicaci贸n](https://github.com/FernandoCutire/storybook-docker) 

## Tabla de Contenidos

## 驴Por qu茅 docker?

Respuesta corta, te lo han pedido subir谩n el storybook a la nube y quieren tener tu sistema de dise帽o en un pipeline siempre atento.

Puedes leer m谩s sobre docker en mi art铆culo de Docker para desarrolladores

## Comenzando

Para este ejemplo decid铆 usar create-react-app para la aplicaci贸n `npx create-react-app storybook-docker`. Recuerda que storybook en su documentaci贸n dice que su comando `sb init` no funciona sin tener una aplicaci贸n antes corriendo, as铆 que es mejor que sigas los pasos.

### Paso 1: Crea la aplicaci贸n

`npx create-react-app storybook-docker`

Si ya tienes tu app, usa tu aplicaci贸n y ve al siguiente paso

### Paso 2: Storybook

`sb init`

Nota: No funciona en proyectos vac铆os por eso usar primero react app

Si ya tienes tu storybook ahora s铆 vayamos a dockerizar.

### Paso 3: Docker

Para este paso, es recomendable que entiendas como funciona un Dockerfile, te lo explico mejor [aqu铆](https://github.com/FernandoCutire/storybook-docker)

Este es el c贸digo que uso para mi Dockerfile

```docker
# Usar una imagen  
FROM node:current-alpine3.14

# Establecer el directorio de trabajo de nuestro contenedor
WORKDIR /usr/src/app

# Copiar el package.json a la carpeta /app de nuestro contenedor
COPY package.json /app

# Copiar谩 otros archivos de la aplicaci贸n
COPY . .

# Ejecutar el comando npm set progress=false && npm install
RUN npm set progress=false && npm install

# Exponer el puerto 8086 de el contenedor docker, fin de documentaci贸n
EXPOSE 8086

# Correr谩 este comando al final cuando se est茅 corriendo el contenedor
CMD ["npm", "start"]
```

Puedes realizar este y a帽adirle seg煤n tus necesidades, yo solo necesito esos comandos as铆 que lo dejo as铆.

### Paso 4: docker-compose

Yo para este proyecto uso docker-compose.yml

Puede que no sea necesario tomando en cuenta que solo es una aplicaci贸n pero a la hora de manejar m谩s en tu aplicaci贸n puede serte 煤til, as铆 que dej贸 el c贸digo.

```docker
version: "3"
services:
  storybook:
    ports:
      - "8086:8086"
    build: .
```

Aqu铆 se expone el puerto 8086.

Corre tu aplicaci贸n con un `docker-compose up`

### Adicional

Algo que me di贸 problema fue en el package.json, ya que corr铆a mi aplicaci贸n dentro del docker pero no pod铆a observarla en el navegador.

As铆 que revisando mi package.json, expuse el puerto 8086 tambi茅n ya que por defecto viene otro, te recomiendo que si te da problemas tambi茅n lo hagas.

```json
{
  "name": "storybook-docker",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@testing-library/jest-dom": "^5.14.1",
    "@testing-library/react": "^11.2.7",
    "@testing-library/user-event": "^12.8.3",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "react-scripts": "4.0.3",
    "web-vitals": "^1.1.2"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "storybook": "start-storybook -p 8086",
    "build-storybook": "build-storybook -s public"
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ],
    "overrides": [
      {
        "files": [
          "**/*.stories.*"
        ],
        "rules": {
          "import/no-anonymous-default-export": "off"
        }
      }
    ]
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  },
  "devDependencies": {
    "@storybook/addon-actions": "^6.3.7",
    "@storybook/addon-essentials": "^6.3.7",
    "@storybook/addon-links": "^6.3.7",
    "@storybook/node-logger": "^6.3.7",
    "@storybook/preset-create-react-app": "^3.2.0",
    "@storybook/react": "^6.3.7"
  }
}
```

Fijate en el comando que dice `"storybook": "start-storybook -p 8086"`, ese ser铆a el que deber铆as de cambiar.

## 馃敟 Recapitulando

Repasemos lo que aprendiste

- Tener una aplicaci贸n corriendo antes de iniciar storybook, una app como la que te genera create-react-app
- Entender como funciona un Dockerfile, para a帽adir capas seg煤n lo necesario
- Verificar los puertos despu茅s de montar tu contenedor
- Revisar el package.json con el comando que corras para inicializar storybook por si te da problemas al momento de visualizar tu contenedor en el servidor local.

## 馃敋 Fin

Sabes como dockerizar una SPA n un entorno de desarrollo, recuerda el repositorio de github, para que tengas un acceso a todo el c贸digo,

[GitHub - FernandoCutire/storybook-docker: This is how you can dockerize a storybook project](https://github.com/FernandoCutire/storybook-docker)
