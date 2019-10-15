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

## Sequelize

Sequelize - é um ORM p NodeJS e Bancos de dados relacionais (MySql, Postgres).

ORM é uma forma de abstrair banco de dados, mudar a forma com que a aplicação se comunica com o banco de dados.

Na arquitetura MVC as tabelas do banco de dados viram models, arquivos que simbolizam estas tabelas.

Para manipular os dados não utiliza a linguagem SQL. Será usado apenas código Javascript. O sequelize vai fazer a tradução da sintaxe Javascript para código SQL.

Esta sintaxe funciona com vários banco de dados

#### Migrations

Funcionalidade de controle de versão para base de dados. Mantém a base de dados atualizada entre todos os devs do time e entre ambientes de desenvolvimento e de produção.

Cada arquivo de migration contém instruções para criação, alteração ou remoção de tabelas ou colunas da base de dados.

Cada arquivo é uma migration e sua ordenação ocorre por data.

A partir do momento em que a migration saiu do ambiente de desenvolvimento e foi repassada para outros usuários, desenvolvedores ou se o ambiente de produção já a executou, não é mais possível editar a migration. Se precisar mudar algo, tem que criar nova migration.

Cada migration deve realizar alterações em apenas uma tabela. É específica para cada tabela.

#### Seeds

Usado para configurar ambiente de testes. São arquivos que populam a base de dados para desenvolvimento (criam dados fakes); São usados em testes automatizados. São executáveis apenas por código/comando e nunca são utilizados em produção

## Arquitetura MVC

É forma de estruturar pastas e arquivos da aplicação para separar as responsabilidades de cada um.

#### Model

Armazena a abstração do banco e manipula os dados. Não possui responsabilidade sobre a regra de negócio da aplicação.

#### Controller

É o ponto de entrada das requisições. Uma rota geralmente está associada a um método de controller. O controller é uma classe e nele pode incluir grande parte das regras de negócio da aplicação.

#### View

É o retorno ao cliente. No modelo API Rest é o JSON (não é o HTML), que será retornado ao front end e manipulado pelo React.

Características:

- classe
- retorna JSON
- Não chama outro controller

#### Quando criar um novo controller:

Cria-se quando houver uma nova entidade na aplicação. Pode haver controller que não tenha models. Ex. na autenticação não se cria um usuário, mas sim uma sessão, pois se está autenticando. Então neste caso seria o 'session controller'.

#### 5 métodos:

- index() - listar
- show() - exibir só um
- store() - cadastrar
- update() - alterar
- delete() - deletar

## ESLint, Prettier & EditorConfig

Ferramentas que ajudam a padronizar a escrita do código. A rocketseat gosta de utilizar o padrão de código da AirBnB

#### EsLint

Verifica se o código está seguindo o padrão.

    $ yarn add eslint -D

Para inicializar um arquivo de configurção:

\$ yarn eslint --init

Faz a instalação das dependências via NPM. Então deve-se remover o package-lock.json criado.

Rodar yarn para fazer mapeamento das novas dependências no yarn.lock

\$ yarn

#### Configurar .eslintrc.js

Instalar a extensão ESLint do VsCode.

Para o ESLint arrumar alguns erros automaticamente, deve-se configurar no settings.json do VsCode e manter o "eslint.autoFixOnSave": true e fazer a configuração de "eslint.validate".

Ver as configurações das rules em .eslintrc.js.

#### Prettier

    $ yarn add prettier eslint-config-prettier eslint-plugin-prettier -D

Configurações em .eslintrc.js

Criar .prettierrc para sobrescrever algumas regras.

Para arrumar tudo de forma automatica:

    $ yarn eslint --fix src --ext .js

#### EditorConfig

Usado para times que usam editores diferentes

instalar extensão EditorConfig no VsCode e gerar e configurar o arquivo .editorconfig.

## Configurando Sequelize

    $ yarn add sequelize

e

    $ yarn add sequelize-cli -D

Criar

    .sequelizerc

vai exportar os caminhos dos arquivos e pastas criados na aplicação. Este arquivo não consegue utilizar o formato import/export.

Criar

    src/config/database.js

e

    $ yarn add pg pg-hstore

v. configurações em:

    src/config/database

## Migration de usuário

Utilizar o sequelize cli

    $ yarn sequelize migration:create --name=create-users

Com isso foi criado o arquivo de migration dentro de database/migrations. Neste arquivo há o método up (para quando a migration for executada) e o método down (Para fazer o rollback)

Para executar

    $ yarn sequelize db:migrate

Com isso, a tabela vai ser criada dentro da base de dados.

É possível ver a tabela no software postbird. Neste, a tabela SequelizeMeta armazena todas a Migrations que o banco de dados já recebeu. Então a migration sabe quais foram e não foram executadas.

Se precisar de alguma alteração na útlima migration

    $ yarn sequelize db:migrate:undo

Se precisar alterar todas as migrations

    $ yarn sequelize db:migrate:undo:all

Para recomeçar:

    $ yarn sequelize db:migrate

## Model de usuário

Serve para manipular os dados de usuários

src/app/models/User.js

No método super.init(), o primeiro parâmetro é um objeto contendo as colunas e valores que o usuário pode receber para edição , criação, etc e o segundo parâmetro é um objeto com sequelize.
