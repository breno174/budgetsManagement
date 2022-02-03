<h1 align="center">
  <img alt="Budgets Management" title="Budgets Management" src="https://fv2-3.failiem.lv/thumb_show.php?i=k4msj66ac&view" width="100px" />
</h1>

<h1 align="center">
  Budgets Management - API
</h1>

<p align = "center">
Este é o backend da aplicação Budgets Management - Uma database fake do Json-Server onde usuários podem se cadastrar, cadastrar despesas e gerenciá-las, separando-as por categorias e listas. O objetivo dessa aplicação é trazer praticidade para que usuários consigam ter uma boa visualização de duas despesas e gastos, fazendo uma boa gestão de seu orçamento.
</p>

<p align="center">
  <a href="#endpoints">Endpoints</a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</p>

## **Endpoints**

A API tem um total de 5 endpoints, sendo relacionados ao usuário - podendo cadastrar-se e efetuar login e relacionados a cadastrar despesas, listas, podendo categorizar as mesmas.<br/>

O url base da API é https://budgets-management.herokuapp.com

## Rotas que não precisam de autenticação

<h2 align ='center'> Listando usuários </h2>

Nesse endpoint podemos ver os usuários já cadastrados na plataforma. Aqui conseguimos ver os usuários e suas informações básicas.
Na API podemos acessar a lista dessa forma:

`GET /users - FORMATO DA RESPOSTA - STATUS 200`

```json
[
  {
    "email": "kenzinho@mail.com",
    "password": "$2a$10$YQiiz0ANVwIgpOjYXPxc0O9H2XeX3m8OoY1xk7OGgxTnOJnsZU7FO",
    "name": "Kenzinho",
    "id": 1
  },
  {
    "email": "bruno@teste.com",
    "password": "$2a$10$N1ykUKN/qvniGLyqoCf4ge7IM9yo9K82bTAN5.h7HpFbhkkvOtUe.",
    "name": "Bruno",
    "id": 2
  },
  {
    "email": "miguel@teste.com",
    "password": "$2a$10$8U3PesnU7KxKiTYjQCf/N.3f/Xaz23PBpj/KYCw3Ueoy3mRNXu4g2",
    "name": "Miguel",
    "id": 3
  }
]
```

<h2 align ='center'> Criação de usuário </h2>

`POST /register - FORMATO DA REQUISIÇÃO`

```json
{
  "email": "marcos@teste.com",
  "password": "123456",
  "name": "Marcos"
}
```

Caso dê tudo certo, a resposta será assim:

`POST /register - FORMATO DA RESPOSTA - STATUS 201`

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6Im1hcmNvc0B0ZXN0ZS5jb20iLCJpYXQiOjE2NDIxMzg5OTcsImV4cCI6MTY0MjE0MjU5Nywic3ViIjoiNCJ9.3ZN-3egG6H6A8MfGyhKS1xDIOEHLMIyze_aWYq8aox4",
  "user": {
    "email": "marcos@teste.com",
    "name": "Marcos",
    "id": 4
  }
}
```

<h3 align ='center'> Possíveis erros no Cadastro </h3>

Caso você acabe errando e mandando algum campo errado, a resposta de erro será assim:
No exemplo a requisição foi feita faltando o campo "email" e/ou "password".

`POST /register - `
` FORMATO DA RESPOSTA - STATUS 400`

```json
"Email and password are required"
```

Email já cadastrado:

`POST /register - `
` FORMATO DA RESPOSTA - STATUS 400`

```json
"Email already exists"
```

A senha necessita de no mínimo 4 caracteres.

`POST /register - `
` FORMATO DA RESPOSTA - STATUS 400`

```json
"Password is too short"
```

<h2 align = "center"> Login </h2>

`POST /login - FORMATO DA REQUISIÇÃO`

```json
{
  "email": "silva@teste.com",
  "password": "1234"
}
```

Caso dê tudo certo, a resposta será assim:

`POST /login - FORMATO DA RESPOSTA - STATUS 200`

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InNpbHZhQHRlc3RlLmNvbSIsImlhdCI6MTY0MjE4MTkyNywiZXhwIjoxNjQyMTg1NTI3LCJzdWIiOiI1In0.2RbjndDPCSQ9uitum28yh3JJDvtkPdcg9HYTIMIfJ1Q",
  "user": {
    "email": "silva@teste.com",
    "name": "Silva",
    "id": 5
  }
}
```

Com essa resposta, vemos que temos duas informações, o user e o token respectivo, dessa forma você pode guardar o token e o usuário logado no localStorage para fazer a gestão do usuário no seu frontend.

<h3 align ='center'> Possíveis erros no Login </h3>

Email / usuário não cadastrado/encontrado:

`POST /login - `
` FORMATO DA RESPOSTA - STATUS 400`

```json
"Cannot find user"
```

Senha incorreta:

`POST /login - `
` FORMATO DA RESPOSTA - STATUS 400`

```json
"Incorrect password"
```

## Rotas que necessitam de autorização

Rotas que necessitam de autorização deve ser informado no cabeçalho da requisição o campo "Authorization", dessa forma:

> Authorization: Bearer {token}
> Após o usuário estar logado, ele deve conseguir visualizar as despesas, listas e categorias.

<h2 align ='center'>Budgets ( Grupos de Despesas )</h2>

<h3 align ='center'>Listar Grupos de Despesas</h3>

São uma listagem de todos os grupos de despesas criadas pelo user logado.

`GET /budgets - FORMATO DA RESPOSTA - STATUS 200`

```json
{
  "name": "Janeiro2021",
  "max_value": 1500,
  "categories": ["food", "entertainment", "transport", "home", "health", "others"],
  "month" : "01",
  "userId": 1,
  "id": 1
},
{
  "name": "Fevereiro2021",
  "max_value": 1300,
  "categories": ["food", "entertainment", "transport", "home", "health", "others"],
  "month" : "02",
  "userId": 1,
  "id": 2
}
```

<h3 align ='center'> Criar/Registrar budget ( Grupo de Despesas )</h3>

`POST /budgets - FORMATO DA REQUISIÇÃO - STATUS 201`

```json
{
  "name": "Fevereiro2021",
  "max_value": 1300,
  "month": "02",
  "categories": [
    "food",
    "entertainment",
    "transport",
    "home",
    "health",
    "others"
  ],
  "userId": 1
}
```

1. O campo - "userId" deve receber respectivamente o id do user que possui a lista de despesas:

```json
{
  "name": "Fevereiro2021",
  "max_value": 1300,
  "categories": ["food", "entertainment", "transport", "home", "health", "others"],
  "month" : "02",
  "userId": 4,
  "id": 2
}
"user": {
  "email": "marcos@teste.com",
  "name": "Marcos",
  "id": 4
}
```

<h3 align ='center'> Atualizar budget ( Grupo de Despesas ) </h3>

`PATCH /budgets/:id - FORMATO DA REQUISIÇÃO - STATUS 200`

1. A url da requisição deve receber o id da lista que deseja atualizar;
2. O corpo da requisição deve receber somente os dados a serem atualizados.

```json
{
  "name": "Março2021",
  "max_value": 1000,
  "categories": ["food", "transport", "home", "others"],
  "month": "02"
}
```

<h3 align ='center'> Deletar/Excluir budget ( Grupo de Despesas )</h3>

`DELETE /budgets/:id - FORMATO DA REQUISIÇÃO - STATUS 200`

1. A url da requisição deve receber o id da lista que deseja excluir;
2. Não é necessário nenhuma informação no corpo da requisição.

```json
{}
```

<h2 align ='center'>Expenses</h2>

<h3 align ='center'> Listar Todas as Despesas </h3>

`GET /expenses - FORMATO DA RESPOSTA - STATUS 200`

```json
[
  {
    "name": "Conta de água",
    "description": "fatura relativa ao mês de novembro",
    "amount": 130.45,
    "type": "home",
    "month": "02",
    "userId": 1,
    "budgetId": 2,
    "id": 1
  },
  {
    "name": "Cardiologista",
    "description": "Consulta particular em janeiro 2022",
    "amount": 300,
    "type": "health",
    "month": "01",
    "userId": 1,
    "budgetId": 1,
    "id": 2
  }
]
```

<h3 align ='center'> Criar(Registrar) Despesa </h3>

`POST /expenses - FORMATO DA REQUISIÇÃO - STATUS 201`

```json
{
  "name": "Cardiologista",
  "description": "Consulta particular em janeiro 2022",
  "amount": 300,
  "type": "health",
  "month": "02",
  "userId": 1,
  "budgetId": 1
}
```

1. O campo - "type" deve receber uma das categorias definidas no grupo da despesa.
2. O campo - "budgetId" deve receber respectivamente o id do grupo de despesa ao qual pertence.
3. O campo - "userId" deve receber respectivamente o id do usuário ao qual a despesa pertence.

<h3 align ='center'> Atualizar despesa</h3>

`PATCH /expenses/:id - FORMATO DA REQUISIÇÃO - STATUS 200`

1. A url da requisição deve receber o id da despesa que deseja atualizar;
2. O corpo da requisição deve receber somente os dados a serem atualizados.

```json
{
  "name": "Cardiologista",
  "description": "Consulta particular em fevereiro 2022",
  "amount": 250,
  "type": "health",
  "month": "02"
}
```

<h3 align ='center'> Deletar/Excluir despesa</h3>

`DELETE /expenses/:id - FORMATO DA REQUISIÇÃO - STATUS 200`

1. A url da requisição deve receber o id da despesa que deseja excluir;
2. Não é necessário nenhuma informação no corpo da requisição.

```json
{}
```

---
