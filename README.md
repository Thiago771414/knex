# knex API Rest full
O Knex é um poderoso construtor de consultas SQL para Node.js, que oferece uma maneira elegante e fluente de interagir com o banco de dados. Ele suporta vários bancos de dados, incluindo PostgreSQL, MySQL, SQLite e outros, tornando-o uma escolha popular para aplicativos Node.js que exigem acesso ao banco de dados.

## Criando uma aplicação Express com Knex
Aqui está um guia passo a passo para criar uma aplicação Express utilizando o Knex para acessar um banco de dados. Certifique-se de que o Node.js já está instalado em seu sistema.

Crie a estrutura inicial da aplicação Express até o passo 9 do repositório do GitHub (https://github.com/Thiago771414/Express).

Na pasta "routes" da aplicação Express, crie uma cópia do arquivo "index.js" e renomeie essa cópia para "apiRouterV1.js". Em seguida, adicione o código a seguir no arquivo "apiRouterV1.js":
````javascript
const express = require('express');
const apiRouterV1 = express.Router();

const produtos = [
  {"id": 1, "descricao": "camiseta", "marca": "Nike", "preco": 49.99},
  {"id": 2, "descricao": "calça jeans", "marca": "Levi's", "preco": 89.95},
  {"id": 3, "descricao": "tênis esportivo", "marca": "Adidas", "preco": 79.50},
  {"id": 4, "descricao": "vestido floral", "marca": "Zara", "preco": 59.99},
  {"id": 5, "descricao": "moletom com capuz", "marca": "Puma", "preco": 69.75},
  {"id": 6, "descricao": "boné", "marca": "New Era", "preco": 29.99},
  {"id": 7, "descricao": "bolsa de couro", "marca": "Michael Kors", "preco": 149.00},
  {"id": 8, "descricao": "óculos de sol", "marca": "Ray-Ban", "preco": 119.50},
  {"id": 9, "descricao": "shorts jeans", "marca": "Guess", "preco": 54.95},
  {"id": 10, "descricao": "jaqueta de couro", "marca": "Harley Davidson", "preco": 199.99}
];

apiRouterV1.get('/produtos', function(req, res, next) {
    res.json(produtos);
});

module.exports = apiRouterV1;
````
Execute o comando npm install para instalar os módulos necessários.

Inicie o servidor com o comando npm start ou node app.js e verifique se a aplicação está sendo executada em localhost:3000.

Se ocorrer algum erro relacionado ao pacote "cookie-parser", navegue até a pasta da aplicação e instale o pacote com o comando npm install cookie-parser.

No arquivo "app.js", configure a nova rota para produtos, adicionando as seguintes linhas de código:

````javascript
var createError = require('http-errors');
var express = require('express');
var path = require('path');
var cookieParser = require('cookie-parser');
var logger = require('morgan');

var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');
var apiRouterV1 = require('./routes/apiRouterV1');

var app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'jade');

app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', indexRouter);
app.use('/users', usersRouter);
app.use('/api/v1', apiRouterV1)

// catch 404 and forward to error handler
app.use(function(req, res, next) {
  next(createError(404));
});

// error handler
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;
````
Adicione o seguinte trecho de código no arquivo "apiRouterV1.js" para implementar um novo endpoint que permite buscar um produto pelo seu ID:
````javascript
var express = require('express');
var apiRouterV1 = express.Router();

var produtos = [
  {"id": 1, "descricao": "camiseta", "marca": "Nike", "preco": 49.99},
  {"id": 2, "descricao": "calça jeans", "marca": "Levi's", "preco": 89.95},
  {"id": 3, "descricao": "tênis esportivo", "marca": "Adidas", "preco": 79.50},
  {"id": 4, "descricao": "vestido floral", "marca": "Zara", "preco": 59.99},
  {"id": 5, "descricao": "moletom com capuz", "marca": "Puma", "preco": 69.75},
  {"id": 6, "descricao": "boné", "marca": "New Era", "preco": 29.99},
  {"id": 7, "descricao": "bolsa de couro", "marca": "Michael Kors", "preco": 149.00},
  {"id": 8, "descricao": "óculos de sol", "marca": "Ray-Ban", "preco": 119.50},
  {"id": 9, "descricao": "shorts jeans", "marca": "Guess", "preco": 54.95},
  {"id": 10, "descricao": "jaqueta de couro", "marca": "Harley Davidson", "preco": 199.99}
]

apiRouterV1.get('/produtos', function(req, res, next) {
    res.json(produtos)
});

apiRouterV1.get('/produtos/:id', function(req, res, next) {
  let id = req.params.id;
  if (id) {
    let idInt = Number.parseInt(id)
    let idx = produtos.findIndex(o => o.id === idInt)
    if (idx > -1) {
      res.json(produtos[idx])
    }
    else{
      res.status(404).json({ message: `Produto não encontrado`})
    }
  }
  else{
    res.status(404).json({ message: `Produto não encontrado`})
  }
});

module.exports = apiRouterV1;
````
Utilize uma extensão cliente HTTP, como o "Thunder Client" (https://github.com/Thiago771414/Express), para testar o endpoint criado. Por exemplo, acesse a URL http://localhost:3000/api/v1/produtos/1 para buscar o produto com o ID igual a 1.

Adicione o seguinte trecho de código no arquivo "apiRouterV1.js" para implementar um novo endpoint que permite criar um produto.
````javascript
apiRouterV1.post('/produtos', function(req, res, next) {
   let produto = req.body
   let newId = Math.max(...produtos.map(o => o.id)) + 1
   produto.id = newId
   produtos.push (produto)
   res.status(201).json({message: `Produto inserido com sucesso`,
                         data: {id: newId}})
});
````
Adicione o seguinte trecho de código no arquivo "apiRouterV1.js" para implementar um novo endpoint que permite deletar um produto.
````javascript
apiRouterV1.delete('/produtos/:id', function(req, res, next) {
  let id = req.params.id;
  if (id) {
    let idInt = Number.parseInt(id)
    let idx = produtos.findIndex(o => o.id === idInt)
    if (idx > -1) {
      produtos.splice (idx, 1)
      res.status(200).json({message: `Produto excluído com sucesso!`})
    }
    else{
      res.status(404).json({ message: `Produto não encontrado`})
    }
  }
  else{
    res.status(404).json({ message: `Produto não encontrado`})
  }
});
````
Adicione o seguinte trecho de código no arquivo "apiRouterV1.js" para implementar um novo endpoint que permite atualizar um produto.
````javascript
apiRouterV1.put('/produtos/:id', function(req, res, next) {
  let id = req.params.id;
  let produto = req.body
  if (id) {
    let idInt = Number.parseInt(id)
    let idx = produtos.findIndex(o => o.id === idInt)
    if (idx > -1) {
      produtos[idx].descricao = produto.descricao
      produtos[idx].marca = produto.marca
      produtos[idx].preco = produto.preco

      res.status(200).json({message: `Produto atualizado com sucesso!`, data: {produto: produtos[idx]}})
    }
    else{
      res.status(404).json({ message: `Produto não encontrado`})
    }
  }
  else{
    res.status(404).json({ message: `Produto não encontrado`})
  }
});
````
## Instalação e Configuração do Knex
Instale o Knex e o banco de dados SQLite:
````javascript
npm install knex sqlite3
````
Inicialize o Knex para criar o arquivo knexfile.js:
````javascript
npx knex init
````
Crie uma pasta chamada db no projeto para guardar tudo relacionado ao banco de dados.
No arquivo knexfile.js, atualize o caminho do arquivo do banco de dados (filename) para a pasta db. Dentro da configuração development, o trecho ficará assim:
````javascript
development: {
  client: 'sqlite3',
  connection: {
    filename: './db/dev.sqlite3'
  },
},
````
Vamos criar um banco de dados a partir de migrações do Knex. Crie uma nova pasta chamada migrations dentro da pasta db.
Execute o comando para criar o primeiro arquivo de migração, responsável por criar a tabela "produtos":
````javascript
npx knex migrate:make tabela-produto
````
Abra o arquivo criado na pasta migrations (ele terá um nome com a data e o nome que você definiu na etapa anterior, como 20230731123456_tabela-produto.js) e adicione o código para a tabela de produtos:
````javascript
exports.up = function(knex) {
  return knex.schema.createTable("produtos", tbl => {
    tbl.increments('id');
    tbl.text("descricao", 255).unique().notNullable();
    tbl.text("marca", 128).notNullable();
    tbl.decimal("preco").notNullable();
  });
};

exports.down = function(knex) {
  return knex.schema.dropTableIfExists('produtos');
};
````
Execute a migração para criar a tabela de produtos no banco de dados:
````javascript
npx knex migrate:latest
````
## Carga Inicial com Seeds
Agora, vamos configurar as seeds (dados iniciais) na pasta seeds. Crie a pasta seeds dentro da pasta db.
No arquivo knexfile.js, abaixo da configuração das migrações, adicione a configuração para as seeds:
````javascript
seeds: {
  directory: './db/seeds'
},
````
Execute o comando para criar o arquivo de seed:
````javascript
npx knex seed:make carga-produto
````
Abra o arquivo criado na pasta seeds (ele terá um nome com a data e o nome que você definiu na etapa anterior, como 20230731123643_carga-produto.js) e substitua o código pelo seguinte:
````javascript
exports.seed = async function(knex) {
  // Deletes ALL existing entries
  await knex('produtos').del();
  // Inserts seed entries
  await knex('produtos').insert([
    {"descricao": "camiseta", "marca": "Nike", "preco": 49.99},
    {"descricao": "calça jeans", "marca": "Levi's", "preco": 89.95},
    {"descricao": "tênis esportivo", "marca": "Adidas", "preco": 79.50},
    {"descricao": "vestido floral", "marca": "Zara", "preco": 59.99},
    {"descricao": "moletom com capuz", "marca": "Puma", "preco": 69.75},
    {"descricao": "boné", "marca": "New Era", "preco": 29.99},
    {"descricao": "bolsa de couro", "marca": "Michael Kors", "preco": 149.00},
    {"descricao": "óculos de sol", "marca": "Ray-Ban", "preco": 119.50},
    {"descricao": "shorts jeans", "marca": "Guess", "preco": 54.95},
    {"descricao": "jaqueta de couro", "marca": "Harley Davidson", "preco": 199.99}
  ]);
};
````
Execute a seed para inserir os dados na tabela:
````javascript
npx knex seed:run
````
## Atualização da API com Knex
Crie uma nova versão da API, duplicando o arquivo apiRouterV1.js e renomeando-o para apiRouterV2.js.
No novo arquivo apiRouterV2.js, atualize a importação do Knex e substitua a lógica dos endpoints para utilizar consultas ao banco de dados com o Knex:
````javascript
var express = require('express');
var apiRouterV2 = express.Router();

const knex = require('knex')(require('../../knexfile').development)

apiRouterV2.get('/produtos', function (req, res, next) {
  knex('produtos')
    .select('*')
    .then(produtos => {
      res.status(200).json(produtos);
    })
    .catch(err => res.status(500).json({ message: `Erro ao obter produtos: ${err.message}` }))
});

apiRouterV2.get('/produtos/:id', function (req, res, next) {
  let id = req.params.id;
  if (id) {
    let idInt = Number.parseInt(id)
    knex('produtos')
      .select('*')
      .where({ id: idInt })
      .then(produtos => {
        if (!produtos.length) {
          res.status(404).json({ message: `Produto não encontrado` })
          return
        }
        let produto = produtos[0]
        res.status(200).json(produto)
      })
      .catch(err => res.status(500).json({ message: `Erro ao obter produtos: ${err.message}` }))
  }
  else res.status(404).json({ message: `Produto não encontrado` })
});

apiRouterV2.post('/produtos', function (req, res, next) {
  let produto = req.body
  knex('produtos')
    .insert(produto, ['id'])
    .then(produtos => {
      if (!produtos.length) {
        res.status(400).json({ message: `Erro ao inserir produto` })
        return
      }
      else {
        let id = produtos[0].id
        res.status(201).json({ message: `Produto inserido com sucesso`, data: { id } })
      }
    })
    .catch(err => res.status(500).json({ message: `Erro ao inserir produto: ${err.message}` }))
});

apiRouterV2.delete('/produtos/:id', function (req, res, next) {
  let id = req.params.id;
  if (id) {
    let idInt = Number.parseInt(id)
    knex('produtos')
      .where({ id: idInt })
      .del()
      .then(result =>
        res.status(200).json({ message: `Produto excluído com sucesso` })
      )
      .catch(err => res.status(500).json({ message: `Erro ao excluir produto: ${err.message}` }))
  }
  else {
    res.status(404).json({ message: `Produto não encontrado` })
  }
});

apiRouterV2.put('/produtos/:id', function (req, res, next) {
  let id = req.params.id;
  let produto = req.body
  if (id) {
    let idInt = Number.parseInt(id)
    knex('produtos')
      .where({ id: idInt })
      .update(produto)
      .then(result => {
        res.status(200).json({ message: `produto alterado com sucesso`, data: { produto: produtos[idx] } })
      })
      .catch(err => res.status(500).json({ message: `Erro ao excluir produto: ${err.message}` }))
  }
  else {
    res.status(404).json({ message: `Produto não encontrado` })
  }
});

module.exports = apiRouterV2;
````
No arquivo app.js, atualize a importação da nova versão da API:
````javascript
// ...

var apiRouterV2 = require('./routes/apiRouterV2');

// ...

app.use('/api/v2', apiRouterV2);

// ...
````
Agora sua API deve estar atualizada para usar o Knex como ORM e o banco de dados SQLite. Utilizando o Knex, você pode facilmente trabalhar com migrações, seeds e consultas ao banco de dados de maneira mais organizada e eficiente. Lembre-se de sempre realizar testes e validar os dados de entrada e saída para garantir o bom funcionamento da API.
