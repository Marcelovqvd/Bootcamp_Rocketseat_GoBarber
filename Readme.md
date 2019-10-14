# Bootcamp modulo 02 - Projeto GoBarber

## Configurando estrutura

```
$ yarn init -y

$ yarn add express

```

criar pasta src p manter todo código manipulável da aplicação.

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

Agora a parte onde está criada a estrutura da aplicação (app.js) está separada do servidor (server.js). Esta separação é importante para os testes automatizados que serão feitos na aplicação.

## Nodemon e Sucrase

O NodeJs ainda não suporta algumas sintaxes. Para ser possível a utilização da sintaxe import/export em vez de require, vamos utilizar o Sucrase.

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

Criar arquivo de configuração p o Nodemon. Estabelece que quando executar nodemon em arquivos com extensão js, utilizar o Sucrase em vez do nodemon.

Criar "scripts" no package.json p o comando yarn dev

```
$ yarn dev

```

Para rodar o sucrase-node.

Com o Sucrase o processo de debug vai ser modificado. Então é necessário criar novo script em package.json.

    "debug": "node --inspect -r sucrase/register src/server.js"

Para instalar as novas dependências:

    $ yarn

Para rodar o debug:

    $ yarn debug

Modificações no launch.json:

    "request": "attach"
    "protocol": "inspector"

## Docker

- Cria ambientes isolados(containers), que não interferem no funcionamento de outras ferramentas do servidor, da aplicação e mesmo do sistema operacional.

- Expõe portas para comunicação com banco de dados. Torna possível utilizar vários banco de dados, cada um em seu container com sua porta específica, sem nenhuma conexão entre eles.

- Imagem - São as ferramentas colocadas dentro dos containeres.

- Container - instância de imagem.

- Docker Registry(Docker Hub) - É o registro de todas as imagens do Docker.

- Dockerfile - É a receita para montar uma imagem. Define como a imagem de nossa aplicação inteira possa rodar em um servidor. Assim, é possível trocar de servidor com facilidade.

ex de receita:

- Partimos de uma imagem existente;
- Definimos a pasta e copiamos os arquivos;
- Instalamos as dependências (Run yarn);
- Definimos a porta que queremos expor (Expose 3333);
- Executamos nossa aplicação (CMD yarn start).

## Configurando Docker

Buscar 'Docker CE' no google para instalar

[Docker](https://docs.docker.com/)

Se a máquina não suportar os requirements de sistema, pode-se usar o Docker Tool Box que é uma versão mais antiga e utiliza o virtual box.

[Docker Tool Box](https://docs.docker.com/docker-for-windows/docker-toolbox/)

Para testar:
  
 Docker -v

    Docker help

#### Criar serviço de banco de dados Postgres

Buscar docker postgres

Acessar

[hub.docker](https://hub.docker.com/_/postgres)

Em start a postgres instance utilizar o comando abaixo com o redirecionamento de porta:

    $ docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres

Com nome (database), senha(docker), nome da imagem (postgres) e redirecionamento de porta.

Qdo acessar localhost na porta 5432 na própria máquina, acessar porta 5432 que é a que o postgres está rodando.

Pode mudar as portas se uma já estiver sendo utilizada.

    $ docker run --name database -e POSTGRES_PASSWORD=docker -p 5432:5432 -d postgres

Com o comando acima, baixa a útlima versão do Postgres e inicializa o container.

Mostra o id do container

    $ docker ps

Lista todos os conatineres que estão em execução na máquina.

Para testar, fazer download da interface PostBird

[Postbird](https://electronjs.org/apps/postbird)

Para fazer a conexão usar username e senha setados acima.

Save and connect mostra uma database postgres pré-criada.

Criar a database. As tabelas vão ser feitas pela própria aplicação.

Para que a database continue sendo executada independentemente se a máquina for reiniciada:

    $ docker stop nomedocontainer

Para parar o container como se estivesse reiniciando a máquina

    $ docker ps -a

Mostra todos os containeres (mesmo os que não estão rodando)

    $ docker start nomedocontainer

Para verificar:

    $ docker ps

Para visualisar algum erro:

    $ docker log

O container criado pode ser utilizado por outras aplicações ou ser configurado para ser usado somente em uma aplicação específica, o que é melhor.
