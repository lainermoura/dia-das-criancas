# 🎉 Projeto Dia das Crianças 🎈

Bem-vindo ao projeto **Dia das Crianças**, uma aplicação divertida que apresenta cards de várias crianças e, ao serem clicados, revelam suas versões adultas! Este projeto é uma maneira especial de relembrar a infância e celebrar o Dia das Crianças.

## 📦 Tecnologias Utilizadas

- **Vue.js 3** 🌐
- **Bootstrap** 🎨

## 🚀 Como Começar

### Pré-requisitos

Antes de começar, você precisará ter o Node.js e o npm instalados em sua máquina.

### Instalação

1. Clone o repositório:
   ```bash
   git clone https://github.com/lainermoura/dia-das-criancas.git
   cd dia-das-criancas
   ```

2. Instale as dependências:
   ```bash
   npm install
   ```

3. Inicie o servidor de desenvolvimento:
   ```bash
   npm run serve
   ```

4. Acesse a aplicação em `http://localhost:8080`.

## 📸 Funcionalidades

- **Cards Interativos**: Clique em qualquer card para ver a foto da versão adulta da criança e o nome da pessoa.
- **Lógica dos Nomes**: O nome e sobrenome exibidos no card da versão adulta são extraídos a partir do nome do arquivo usando as convenções PascalCase ou camelCase. Por exemplo, se o arquivo da imagem se chama `JoaoSilva.jpg`, o card exibirá "João Silva" abaixo da imagem.

## 🔍 Lógica do Projeto

A lógica por trás do projeto é simples e eficiente:

1. **Estrutura de Arquivos**: As imagens são organizadas em uma pasta, onde o nome de cada arquivo segue as convenções PascalCase ou camelCase.
2. **Extração do Nome**: A aplicação analisa o nome do arquivo, separando as partes maiúsculas e minúsculas para formar o nome e sobrenome. Assim, `JoãoSilva.jpg` se torna "João Silva".
3. **Interatividade**: Ao clicar no card, a imagem adulta e o nome são mostrados, proporcionando uma experiência divertida e nostálgica.

---

✨ Divirta-se relembrando a infância! ✨

#DiaDasCrianças #Vue #Bootstrap
