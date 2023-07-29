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
