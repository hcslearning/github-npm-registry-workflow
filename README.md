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

2. Actualizar *package.json* con el nuevo número de versión.

3. Crear un nuevo release dentro del repositorio de Github.

**Nota Importante:** El nombre del proyecto debe estar prefijado por el nombre de usuario del repositorio Github (ej. @hcslearning/mi-proyecto).
