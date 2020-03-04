# Yarn workspaces

## O que é

É uma forma de organização de projeto que permite compartilha lógicas e componentes entre projetos apenas criando um `packages` que poderá ser acessado entre todos os projetos e podemos ter um único `node_modules` entre todos os projetos.

## Hands-on

```cmd
  mkdir yarn-workspaces
  cd yarn-workspace
  touch package.json
```

Agora iremos configurar nosso package.json, o principal é apenas definir nossos workspaces (aconselho criar uma pasta com o nome de packages).

```json
{
  "private": true,
  "workspaces": [
    "packages/*"
  ]
}
```

Está definido que tudo que estiver dentro da nossa pasta `packages` fará parte do nosso `workspace`.

Dentro de `workspaces` vamos criar duas pastas `common` e `server`. Pensando em modelo real todas as funções que serão compartilhadas entre os projetos estarão dentro de `common`, para podermos iniciar nossos testes vamos acessar as pastas de `common` e `server` e iniciar um projeto.

```cmd
cd common
yarn init -y
cd ..
cd server
yarn init -y
```

Agora com nossos `packages.json` dentro de cada projeto vamos renomear os names dos pacotes para poderem serem facilmente identificados (aconselho utilizar o nome do seu projeto/nome do pacote). Vamos alterar primeiro o pacote de `commom`.


```json
{
  "name": "@yarn-workspaces/common",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "dependencies": {
    "axios": "^0.19.2"
  }
}
```

E agora vamos fazer o mesmo processo para o pacote do `server`.

```json
{
  "name": "@yarn-workspace/server",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT"
}
```

Com nossos packages completos agora basta apenas criar uma função de teste dentro de `commom`. Primeiro vamos criar nosso `index.js` dentro de common.

```cmd
cd packages/common
touch index.js
```

Agora basta abrir o arquivo index.js dentro de common e criar sua função.

```js
module.exports = () => {
  console.log('Hello from common')
}
```

Agora criado nossa primeira função que será compartilhada entre os módulos podemos acessar nosso `packages server` e criar nosso `index.js`.

```cmd
cd packages/server
touch index.js
```

Agora com o arquivo criado basta importar a função do nosso `common`.

```js
const CommonFunction = require('@yarn-workspaces/common')

CommonFunction()
```

Com todo esse processo feito temos que adicionar a dependência do `common` no nosso `package.json` que está dentro de `server`.

```json
{
  "dependencies": {
    "@yarn-workspaces/common": "1.0.0"
  }
}
```

Com o setup finalizado basta voltar para a raiz do nosso projeto, executar o comando para instalar as depedências e testar.

```cmd
yarn install
```

Para testar basta executar o `index.js` que está dentro do nosso pacote do `server`.

```cmd
  node packages/server/index.js
  $ Hello from common
```

A parte mais interessante se instalarmos qualquer depedência do `npm` ela será compartilhada entre todos os projetos caso queira testar basta escolher alguma lib e instalar.

```cmd
yarn add axios
```
