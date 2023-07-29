# knex
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
const createError = require('http-errors');
const express = require('express');
const path = require('path');
const cookieParser = require('cookie-parser');
const logger = require('morgan');

const indexRouter = require('./routes/index');
const usersRouter = require('./routes/users');
const apiRouterV1 = require('./routes/apiRouterV1');

const app = express();

// ... Código existente ...

app.use('/', indexRouter);
app.use('/users', usersRouter);
app.use('/api/v1', apiRouterV1);

// ... Código existente ...
````
Adicione o seguinte trecho de código no arquivo "apiRouterV1.js" para implementar um novo endpoint que permite buscar um produto pelo seu ID:
````javascript
// ... Código existente ...

apiRouterV1.get('/produtos/:id', function(req, res, next) {
  const id = req.params.id;
  if (id) {
    const idInt = Number.parseInt(id);
    const idx = produtos.findIndex(o => o.id === idInt);
    if (idx > -1) {
      res.json(produtos[idx]);
    } else {
      res.status(404).json({ message: `Produto não encontrado` });
    }
  } else {
    res.status(404).json({ message: `Produto não encontrado` });
  }
});

// ... Código existente ...
````
Utilize uma extensão cliente HTTP, como o "Thunder Client" (https://github.com/Thiago771414/Express), para testar o endpoint criado. Por exemplo, acesse a URL http://localhost:3000/api/v1/produtos/1 para buscar o produto com o ID igual a 1.
