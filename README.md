# -Validador-de-Curriculos-com-Node.js
Mostra domínio com Node.js + Express, manipulação de arquivos JSON e organização de rotas.

// Projeto: Validador de Currículos com Node.js + Express

// Estrutura de Diretórios Sugerida:
// validador-curriculo/
// ├── server.js
// ├── routes/
// │   └── cvRoutes.js
// ├── data/
// │   └── curriculos.json
// ├── public/
// │   └── index.html
// ├── package.json
// └── README.md

// -------------------------------------------
// 1. server.js (ponto de entrada)
// -------------------------------------------
const express = require('express');
const bodyParser = require('body-parser');
const path = require('path');
const cvRoutes = require('./routes/cvRoutes');

const app = express();
const PORT = 3000;

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
app.use(express.static(path.join(__dirname, 'public')));

app.use('/api/curriculos', cvRoutes);

app.listen(PORT, () => {
  console.log(`Servidor rodando em http://localhost:${PORT}`);
});

// -------------------------------------------
// 2. routes/cvRoutes.js (rotas da API)
// -------------------------------------------
const express = require('express');
const fs = require('fs');
const path = require('path');
const router = express.Router();

const filePath = path.join(__dirname, '../data/curriculos.json');

// Rota POST para validar e salvar currículo
router.post('/', (req, res) => {
  const { nome, email, formacao, experiencia } = req.body;

  if (!nome || !email || !formacao || !experiencia) {
    return res.status(400).json({ mensagem: 'Preencha todos os campos obrigatórios.' });
  }

  const novoCV = { nome, email, formacao, experiencia, dataEnvio: new Date().toISOString() };

  // Salvar no arquivo
  const dados = fs.existsSync(filePath) ? JSON.parse(fs.readFileSync(filePath)) : [];
  dados.push(novoCV);
  fs.writeFileSync(filePath, JSON.stringify(dados, null, 2));

  res.status(201).json({ mensagem: 'Currículo válido e salvo com sucesso!' });
});

module.exports = router;

// -------------------------------------------
// 3. public/index.html (formulário HTML)
// -------------------------------------------
<!-- index.html -->
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Validador de Currículo</title>
</head>
<body>
  <h2>Envie seu Currículo</h2>
  <form action="/api/curriculos" method="post">
    <input type="text" name="nome" placeholder="Nome completo" required><br>
    <input type="email" name="email" placeholder="E-mail" required><br>
    <input type="text" name="formacao" placeholder="Formação" required><br>
    <textarea name="experiencia" placeholder="Descreva sua experiência" required></textarea><br>
    <button type="submit">Enviar</button>
  </form>
</body>
</html>

// -------------------------------------------
// 4. README.md (documentação resumida)
// -------------------------------------------
# Validador de Currículos com Node.js

Este projeto é uma aplicação simples em Node.js com Express que valida e armazena currículos submetidos por formulário HTML. Os dados são salvos localmente em um arquivo JSON.

## Como usar

1. Instale as dependências:
```bash
npm install
```

2. Inicie o servidor:
```bash
node server.js
```

3. Acesse `http://localhost:3000` no navegador.

## Tecnologias utilizadas
- Node.js
- Express
- HTML + CSS (formulário básico)
- Armazenamento local com JSON

## Objetivo
Simular uma validação de dados e prática com rotas, middlewares e manipulação de arquivos usando Node.js para fins de estudo e portfólio.

// -------------------------------------------
// 5. package.json (exemplo mínimo)
// -------------------------------------------
{
  "name": "validador-curriculo",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "body-parser": "^1.20.2",
    "express": "^4.18.2"
  }
}
