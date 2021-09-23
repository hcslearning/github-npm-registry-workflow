# github-npm-registry-workflow

Para crear un paquete automáticamente en el Registro NPM de Github se debe:

1. Haber creado el siguiente archivo en el repositorio: 
*.github/workflows/release-package.yml*

```
name: Node.js Package

on:
  release:
    types: [created]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: 12
      - run: npm ci
      - run: npm test

  publish-gpr:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: 12
          registry-url: https://npm.pkg.github.com/
      - run: npm ci
      - run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{secrets.GITHUB_TOKEN}}
```

1. Actualizar *package.json* con el nuevo número de versión.

1. Asegurarse que en *package.json* el nombre del proyecto vaya con el prefijo del nombre de usuario de Github (ej. @hcslearning/mi-proyecto).

1. Asegurarse que en *package.json* NO esté el campo:

``` 
private: "true"
```

1. Crear un nuevo release dentro del repositorio de Github.

1. En el proyecto donde se utilice la librería recuerde agregar un archivo *.npmrc* con un contenido similar a:

```
@hcslearning:registry=https://npm.pkg.github.com
//npm.pkg.github.com/:_authToken=ghp_AxZdfasdrASDFASDF1312fasdfaasdgasdfaUC
registry=https://registry.npmjs.org
```



