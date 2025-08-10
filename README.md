# 🤖 Projeto: Reconhecimento Facial, Emoções e Chatbot com Node.js e Dialogflow

---

## 💡 Sobre o Projeto

Esse projeto é uma solução backend robusta feita em **Node.js** que junta duas paradas poderosas:

1. **Reconhecimento facial e análise de emoções** usando a biblioteca `face-api.js` (via `@vladmandic/face-api`) rodando no servidor com NodeCanvas.
2. **Chatbot inteligente** integrado com o **Dialogflow** para criar uma experiência interativa com o usuário.

Ou seja, o sistema recebe imagens, detecta rostos e identifica emoções com alta precisão. Além disso, pode responder às interações do usuário via chatbot, permitindo construir aplicações que entendem tanto o que o usuário fala, quanto como ele está se sentindo pela expressão facial.

---

## ⚙️ O que o projeto faz?

- Recebe uma **imagem via upload** (form-data) na rota `/processar-foto`
- Usa modelos de IA pré-treinados para:
  - Detectar rostos na imagem
  - Reconhecer a expressão facial dominante (feliz, triste, neutro, surpreso, etc)
- Responde com um JSON contendo:
  - Quantidade de rostos detectados
  - Emoção principal da primeira face detectada
  - Grau de confiança dessa emoção (em %)
- Remove o arquivo da imagem enviada para não ocupar espaço no servidor
- Serve um **chatbot integrado com Dialogflow**, que permite interação por texto (via outra rota ou frontend)

---

## 🧩 Tecnologias Utilizadas

| Tecnologia            | Descrição                                                                                   |
|----------------------|---------------------------------------------------------------------------------------------|
| **Node.js**          | Ambiente para rodar JavaScript no backend                                                   |
| **Express**          | Framework web para criar servidor HTTP                                                      |
| **face-api.js**      | Biblioteca para detecção facial e análise de emoções (fork `@vladmandic/face-api` para Node)|
| **canvas (NodeCanvas)** | Manipulação de imagens em Node.js (essencial para face-api rodar fora do browser)          |
| **multer**           | Middleware para lidar com uploads de arquivos                                               |
| **cors**             | Habilita requisições externas para a API                                                   |
| **Dialogflow**       | Plataforma de NLP do Google para criação de chatbots inteligentes                           |

---

## 🧠 Como funciona a parte de IA (Reconhecimento Facial)?

- **Modelos carregados**: TinyFaceDetector (detecção de rostos), FaceLandmark68Net (pontos-chave do rosto), FaceRecognitionNet (reconhecimento), FaceExpressionNet (detecção de emoções)
- Esses modelos são pré-treinados e carregados do disco (`/models`)
- Quando uma imagem chega no servidor, ela é carregada pelo módulo `canvas`
- O `face-api` analisa a imagem e detecta todas as faces e suas expressões
- A emoção com maior probabilidade é extraída e retornada junto com a confiança

---

## 📝 Explicação do `server.js` (ou seu arquivo principal)

- Importa libs essenciais (`express`, `multer`, `cors`, `canvas`, `face-api`, etc)
- Define paths com `path` e `fileURLToPath` para organização das pastas
- Configura o Express para servir arquivos estáticos (`public`, `models`) e habilita CORS
- Configura o multer para receber upload de imagem e salvar temporariamente em `uploads/`
- Faz o monkey patch do `face-api` pra funcionar com NodeCanvas (ambiente Node)
- Função `carregarModelos()` que carrega os modelos do face-api do disco no início do servidor (await na inicialização)
- Endpoint POST `/processar-foto`:
  - Recebe a imagem
  - Processa com face-api para detectar faces e expressões
  - Remove a imagem do disco após o processamento
  - Retorna JSON com número de faces, emoção dominante e confiança
- Inicializa o servidor na porta 5000 (ou variável de ambiente)
- Loga a URL do servidor no console para facilitar

---

## 🤖 Chatbot com Dialogflow

- O projeto integra a API do Dialogflow para responder perguntas e interagir com o usuário
- O Dialogflow é uma ferramenta poderosa do Google que entende linguagem natural e mapeia intenções para respostas programadas ou até acionamento de funções
- No backend, você pode criar uma rota para receber mensagens do frontend e encaminhar para o Dialogflow
- O Dialogflow processa e retorna a resposta inteligente, que é então entregue ao usuário
- Essa integração pode ser expandida para criar assistentes virtuais que entendem tanto texto quanto emoção (combinando a análise facial)

---

## 📦 Como rodar o projeto localmente

1. Clone o repositório:
   ```bash
   git clone https://github.com/seu-usuario/seu-repo.git
   cd seu-repo

2. Instale as dependências:

   ```bash
   npm install
   ```

3. Baixe os modelos do `face-api.js` e coloque dentro da pasta `/models`.

   * Você pode baixar aqui: [https://github.com/justadudewhohacks/face-api.js/tree/master/weights](https://github.com/justadudewhohacks/face-api.js/tree/master/weights)

4. Configure suas credenciais do Dialogflow (se for usar o chatbot):

   * Crie um projeto no Google Cloud
   * Ative Dialogflow API
   * Baixe o arquivo JSON da chave de serviço e configure as variáveis de ambiente para acesso

5. Inicie o servidor:

   ```bash
   node server.js
   ```

   Ou com `nodemon`:

   ```bash
   nodemon server.js
   ```

6. Acesse:

   * Endpoint `/processar-foto` para enviar imagens e receber análise facial/emocional
   * Rota do chatbot para enviar mensagens e receber respostas (depende do seu front ou setup)

---

## 🗂 Estrutura do Projeto

```
/models          # Modelos de IA (deve subir no repo)
/public          # Frontend estático (opcional)
/uploads         # Uploads temporários (não subir no Git)
/server.js       # Código principal do backend
.gitignore      # Arquivos ignorados pelo Git
package.json    # Dependências e scripts npm
```

---

## 🚀 Subindo no GitHub

1. Crie um repositório novo no GitHub, sem README (pra evitar conflitos)
2. No terminal, inicialize git e adicione tudo:

   ```bash
   git init
   git add .
   git commit -m "Primeiro commit: backend reconhecimento facial + chatbot"
   ```
3. Adicione o remoto:

   ```bash
   git remote add origin https://github.com/seu-usuario/seu-repo.git
   ```
4. Envie para o GitHub:

   ```bash
   git push -u origin master
   ```

---

## 💬 Contato

Se quiser trocar ideia, tirar dúvidas ou pedir ajuda para customizar o projeto, só chamar!
