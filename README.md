<h1 align="center">
  <img alt="FastFeet Logo" title="Fastfeet" src=".github/logo.png" width="300px" />
</h1>

<p>Este desafio consiste em criar uma aplicação completa (Back-end, Front-end e Mobile) do zero usando Node.js, ReactJS e React Native</p>

<p align="center">
  <img alt="License" src="https://img.shields.io/badge/license-MIT-%2304D361">
</p>

## :rocket: Sobre o desafio

A aplicação é um app para uma transportadora fictícia, o FastFeet.

## Executando o projeto:

- Clone o projeto.

- Instale o banco de dados PostgreSQL ou um container docker com o mesmo.

- crie um perfil no banco de dados com:

  - **USERNAME**: postgres
  - **PASSWORD**: fstfeet

- Crie um database com o nome: `fastfeetdb`

- Na pasta do projeto clonado execute o seguinte comando:
  <pre><code>npm install</code></pre>

  OU
  <pre><code>yarn</code></pre>

- Execute as migrations no banco de dados com o seguinte comando:
  <pre><code>npx sequelize db:migrate</code></pre>

  OU
  <pre><code>yarn sequelize db:migrate</code></pre>

- Popule o banco de dados com o seguinte comando:
  <pre><code>npx sequelize db:seed:all </code></pre>

  OU
  <pre><code>yarn sequelize db:seed:all </code></pre>

- Por último inicialize o servidor com o seguinte comando:
  <pre><code>npm run dev</code></pre>

  OU
  <pre><code>yarn dev</code></pre>

- Ele estará rodando no endereço:
  <pre><code>http://localhost:3333</code></pre>

### **Ferramentas e tecnologias usadas no backend :**

- <a href="https://nodejs.org/">Node.js</a> + <a href="https://github.com/expressjs/express/">Express</a>
- <a href="https://github.com/alangpierce/sucrase">Sucrase</a> + <a href="https://github.com/remy/nodemon">Nodemon</a>
- <a href="https://github.com/sequelize/sequelize">Sequelize</a>
- <a href="https://www.postgresql.org/">PostgresSQL</a>
- <a href="https://github.com/dcodeIO/bcrypt.js">Bcryptjs</a>
- <a href="https://github.com/auth0/node-jsonwebtoken">Jsonwebtoken</a>
- <a href="https://github.com/jquense/yup">Yup</a>

### **Funcionalidades**

Abaixo estão descritas as funcionalidades contidas na aplicação.

<a href="#1-autenticação">Autenticação</a><br>
<a href="#2-gestão-de-destinatários">Gestão de Destinatários</a><br>
<a href="#3-gestão-de-entregadores">Gestão de Entregadores</a><br>
<a href="#4-gestão-de-encomendas">Gestão de Encomendas</a><br>
<a href="#5-funcionalidades-do-entregador">Funcionalidades do entregador</a><br>
<a href="#6-funcionalidades-da-distribuidora">Funcionalidades da distribuidora</a>

### **1. Autenticação**

O usuário administrador pode se autenticar usando email e senha.

Ao se autenticar o usuário recebe um token de acesso gerado via JSON Web Token, que será necessario para poder acessar algumas funcionalidades da aplicação.

- Rota de autenticação: `POST http://localhost:3333/sessions`
  - Exemplo de campos a serem enviados:
  <pre><code>
    {
      "email": "admin@fastfeet.com",
      "password": "123456"
    }
  </code></pre>

### **2. Gestão de Destinatários**

Para realizar todas as operações abaixo o administrador deve enviar o seu **Bearer token** para confirmar que está autenticado no sistema.

Os destinatários são mantidos (cadastrados/atualizados) na aplicação, e esses destinatários contém **nome** e campos de endereço: **rua**, **número**, **complemento**, **estado**, **cidade** e **CEP**.

As informações do destinatário são guardadas em uma tabela do banco de dados chamada `recipients`.

O cadastro de destinatários só pode ser feito por administradores autenticados na aplicação.

O destinatário não pode se autenticar no sistema.

- Rota de cadastro de destinatários: `POST http://localhost:3333/recipients`

  - Exemplo de campos a serem enviados:
    <pre><code>
    {
      "name": "Destinatário",
      "street": "Rua do destinatário",
      "number": "00",
      "complement": "Complemento de endereço",
      "state": "Estado do destinatário",
      "city": "Cidade do destinatário",
      "cep": "CEP do destinatário"
    }
    </code></pre>

- Rota de atualização dos dados de destinatários: `PUT http://localhost:3333/recipients`

  - Exemplo de campos a serem enviados:
    <pre><code>
    {
      "currentName": "Nome atual do destinatário",
      "street": "Rua nova"
    }
    </code></pre>
    Obs.1: o campo `currentName` é obrigatório, todos os outros campos são opcionais e seguem a mesma estrutura do exemplo de cadastro de destinatários.

  Obs.2: Para atualizar o nome do destinatário adicione um campo chamado `newName`.

- Rota para listagem de todos os destinatários: `GET http://localhost:3333/recipients`

  - Retorna um array de objetos contendo os usuários.

- Rota para listagem de um destinatário específico: `GET http://localhost:3333/recipients?name=nomeDoDestinatario`

  - Retorna um array com um objeto contendo o usuário caso ele seja encontrado no banco de dados.

### **3. Gestão de Entregadores**

Para realizar todas as operações abaixo o administrador deve enviar o seu **Bearer token** para confirmar que está autenticado no sistema.

**Listagem de entregadores:**

Administradores podem listar todos os entregadores usando a seguinte rota:

- `GET http://localhost:3333/deliverymen`

Administradores podem listar um entregador específico passando o nome do mesmo via `query parameter` usando a seguinte rota:

- `GET http://localhost:3333/deliverymen?name=nomeDoEntregador`

**Cadastro de entregadores:**

Administradores podem cadastrar novos entregadores usando a seguinte rota, a foto do entregador pode ser enviado na requisição:

- Rota: `POST http://localhost:3333/deliverymen`

- O corpo da requisição dever ser do tipo `Multipart Form`.

- A foto do entregador deve estar contida no campo `file`.

- O nome do entregador deve estar contido no campo `name`.

- O email do entregador deve estar contido no campo `email`.

**Atualização de entregadores:**

Administradores podem atualizar os dados entregadores informando o **ID** do entregador usando a seguinte rota:

- Rota: `PUT http://localhost:3333/deliverymen/id`
  <br>

Exemplo usando id = 1:

- Rota: `PUT http://localhost:3333/deliverymen/1`

* O corpo da requisição dever ser do tipo `Multipart Form`.

* A foto do entregador deve estar contida no campo `file`.

* O nome do entregador deve estar contido no campo `name`.

* O email do entregador deve estar contido no campo `email`.

**Deletar entregadores:**

Administradores podem deletar entregadores da base de dados usando a seguinte rota:

- `DELETE http://localhost:3333/deliverymen/id`

Exemplo usando o id = 1

- `DELETE http://localhost:3333/deliverymen/1`

### **4. Gestão de Encomendas**

Para realizar todas as operações abaixo o administrador deve enviar o seu **Bearer token** para confirmar que está autenticado no sistema.

**Listagem todas as encomendas:**

Administradores podem listar todas as encomendas usando a seguinte rota:

- Rota: `GET http://localhost:3333/orders`

- Ela retornará um array de encomendas.

Administradores podem listar encomendas especificas apenas informando o nome da mesma via `query parameter` usando a seguinte rota:

- Rota: `GET http://localhost:3333/orders?order=nomeDaEncomenda`

- Ela retornará um array com a encomenda.

**Cadastro de encomendas:**

Administradores podem cadastrar novas encomendas usando a seguinte rota:

- Rota: `POST http://localhost:3333/orders`

- É obrigatório o envio do id do destinatário.

- É obrigatório o envio do id do entregador.

- É obrigatório o envio do nome do produto a ser entregue.

- Exemplo de corpo da requisição a ser enviado:
<pre><code>
   {
     "recipient_id": "2",
     "deliveryman_id": "1",
     "product": "Caixa de som"
   }
</code></pre>

Quando a encomenda é cadastrada para um entregador, o entregador recebe um e-mail com detalhes da encomenda, com nome do produto e uma mensagem informando-o que o produto já está disponível para a retirada

**Atualização de encomendas:**

Administradores podem atualizar as encomendas usando a seguinte rota:

- Rota: `PUT http://localhost:3333/orders/id`

- É obrigatório enviar o id da encomenda a ser atualizada.

- Todos os campos da requisição são opcionais.

- Exemplo da requisição e dos campos a serem enviados:
  <br>
  <pre><code>PUT http://localhost:3333/orders/1</code></pre>
  <pre><code>
    {
      "recipient_id": "1",
      "deliveryman_id": "2",
      "product": "Caixa vazia"
    }
  </code></pre>

**Deletar encomendas:**

Administradores podem deletar encomendas usando a seguinte rota:

- Rota: `DELETE http://localhost:3333/orders/id`

- É obrigatório enviar o id da encomenda a ser deletada.

- Exemplo da requisição a ser enviada:
  <br>
  `DELETE http://localhost:3333/orders/1`

### **5. Funcionalidades do entregador**

**Visualizar encomendas:**

O entregador pode visualizar as encomendas disponíveis para ele entregar informando apenas o seu id.

- Exemplo de requisição:
  <br>
  `GET http://localhost:3333/deliveries/id`
  <br>

- Exemplo de requisição usando id = 1:
  <br>
  `GET http://localhost:3333/deliveries/1`

**Visualizar encomendas já entregues:**

O entregador pode visualizar as encomendas que já foram entregues por ele usando a seguinte rota:

- Exemplo de requisição:
  <br>
  `GET http://localhost:3333/deliveries/id/completed`

- Exemplo de requisição usando id = 1:
  <br>
  `GET http://localhost:3333/deliveries/1/completed`

**Retirar uma encomenda para entregar**

O entregador pode iniciar a entrega de uma encomenda usando a seguinte rota:

- Exemplo de requisição:
  <br>
  `POST http://localhost:3333/deliveries/take`

- É **obrigatório** enviar os campos informando o id do entregador e o id da encomenda.

- Exemplo de campos a serem enviados no corpo da requisição:
<pre><code>
{
	"deliveryman_id": "1",
	"order_id": "2"
}
</code></pre>

- O entregador só pode fazer **5 retiradas por dia** e as retiradas só podem ser feitas entre as **08:00 e 18:00h**.

**Finalizar uma entrega:**

O entregador pode finalizar uma entrega usando a seguinte rota:

- Exemplo de requisição:
  <br>
  `PUT http://localhost:3333/deliveries/id/conclude`

- Exemplo de requisição usando id do entregador = 1:
  <br>
  `PUT http://localhost:3333/deliveries/1/conclude`

- O entregador deve enviar uma foto da assinatura do destinatário para comprovar que recebeu a encomenda.

- O corpo da requisição dever ser do tipo `Multipart Form`.

- A foto da assinatura deve estar contida no campo `file`.

- O id da encomenda deve estar contido no campo `order_id`.

**Cadastrar problemas na entrega:**

O entregador pode cadastrar um problema na entrega usando a seguinte rota:

- Exemplo de requisição:
  <br>
  `POST http://localhost:3333/delivery/id/problems`

- Exemplo de requisição usando id da encomenda = 1:
  <br>
  `POST http://localhost:3333/delivery/1/problems`

- A requisição também deve ter o seguinte campo em seu corpo:
<pre><code>
  {
  	"description": "Roubo de encomenda"
  }
</code></pre>

**Listar todos os problemas de uma encomenda:**

O entregador pode ver todos os problemas já cadastrados para uma encomenda apenas informando o id dela usando a seguinte rota:

- Exemplo de requisição;
  <br>
  `POST http://localhost:3333/delivery/id/problems`

- Exemplo de requisição usando id da encomenda = 1:
  <br>
  `POST http://localhost:3333/delivery/1/problems`

### **6. Funcionalidades da distribuidora:**

Para realizar todas as operações abaixo o administrador deve enviar o seu **Bearer token** para confirmar que está autenticado no sistema.

**Listar todas as entregas com algum problema**

A distribuidora pode ver todas as entregas com algum problema usando a seguinte rota:

- Exemplo de requisição:
  <br>
  `GET http://localhost:3333/delivery`

**Cancelar entrega**

A distribuidora pode cancelar uma entrega com apenas o ID do problema usando a seguinte rota:

- Exemplo de requisição:
  <br>
  `DELETE http://localhost:3333/problem/id/cancel-delivery`

- Exemplo de requisição usando ID do problema = 1:
  <br>
  `DELETE http://localhost:3333/problem/1/cancel-delivery`

- Quando uma encomenda é cancelada o entregador recebe um email informando-o sobre o cancelamento.

---
