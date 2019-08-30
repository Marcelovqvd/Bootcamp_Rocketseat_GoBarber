# Bootcamp modulo 02 - Projeto GoBarber

## Configurando estrutura

```
$ yarn init -y

$ yarn add express

```

criar pasta src p manter todo código manipulável da aplicação.

Dentro da src, criar app.js, router.js, server.js

#### app.js

Vai ser a estrutura da aplicação, onde se registra os middlewares e as rotas.

Importar o express e definir a classe com o método constructor.
Quando a classe App for chamada, o método constructor vai ser executado automaticamente. Outros métodos dentro da classe vão ser middlewares() e routes().

- definir variável server que recebe express();
- usar use p receber requisições em json;
- importar as rotas dentro do use. Neste caso, funcionam como middlewares;
- chamar os métodos dentro do constructor
- exportar uma nova instância de App (new App) e exportar diretamente o server.

#### server.js

Onde vai ser criado o servidor

- importar o App()
  \*app.listen(3030);

#### routes.js

- importar apenas o router do express { router } para separar a parte de roteamento do express;
- exportar routes;
- construir as rotas.

## Nodemon e Sucrase

O node ainda não suporta algumas sintaxes. Para nãoi utilizar require por ex para importar, é possível usar formas como Babel. Aqui vamos utilizar o sucrase.

#### Sucrase

Entende a nova sintaxe para import/export

```
$ yarn add sucrase nodemon -D

```

Agora é possível alterar os requires para import e usar export default.

```
$ yarn sucrase-node src/server.js
```

#### Nodemon

Inicializa o servidor automaticamente.

Configurando:

Criar arquivo de configuração p o Nodemon. Estabelece que quando executar nodemon em arquivos com extensão js, utilizar o sucrase em vez do nodemon.

Criar "scripts" no package.json p o comando yarn dev

```
$ yarn dev

```

Para rodar o sucrase
