# Painel de Dívidas da Viagem

Site simples onde cada pessoa marca suas próprias parcelas como pagas — e todo mundo vê a atualização na hora, de qualquer celular.

Faltam **2 passos** antes de funcionar: criar o banco de dados gratuito (Firebase) e publicar o site (GitHub Pages).

---

## Passo 1 — Criar o banco de dados gratuito (Firebase)

1. Acesse **https://console.firebase.google.com** e entre com uma conta Google.
2. Clique em **"Adicionar projeto"**, dê um nome (ex: `dividas-viagem`) e finalize a criação (pode desmarcar o Google Analytics, não é necessário).
3. No menu à esquerda, clique em **"Compilação" → "Firestore Database"**.
4. Clique em **"Criar banco de dados"**.
   - Escolha o modo **"Iniciar no modo de teste"** (assim ele já libera leitura/escrita — importante, senão o site não vai conseguir salvar nada).
   - Escolha uma localização (qualquer uma serve, ex: `southamerica-east1`).
5. Ainda no Firebase, clique no ícone de engrenagem (⚙️) → **"Configurações do projeto"**.
6. Role até **"Seus apps"** e clique no ícone **`</>`** (Web) para criar um app.
7. Dê um apelido (ex: `site`) e clique em **"Registrar app"**.
8. Ele vai mostrar um bloco de código com um objeto `firebaseConfig = { ... }`. **Copie esse bloco inteiro.**

### Colando a configuração no site

Abra o arquivo `index.html`, procure por este trecho perto do final:

```js
const firebaseConfig = {
  apiKey: "COLE_AQUI",
  authDomain: "COLE_AQUI.firebaseapp.com",
  projectId: "COLE_AQUI",
  storageBucket: "COLE_AQUI.appspot.com",
  messagingSenderId: "COLE_AQUI",
  appId: "COLE_AQUI"
};
```

Substitua esse bloco inteiro pelo que você copiou do Firebase no passo 8.

> **Atenção sobre o "modo de teste"**: por padrão, ele libera acesso de leitura/escrita por 30 dias e depois bloqueia sozinho. Se o site parar de salvar depois de um tempo, volte em Firestore Database → aba **"Regras"** e troque a data de expiração, ou use estas regras permanentes (simples, sem login):
>
> ```
> rules_version = '2';
> service cloud.firestore {
>   match /databases/{database}/documents {
>     match /{document=**} {
>       allow read, write: if true;
>     }
>   }
> }
> ```
>
> Isso deixa o banco aberto para qualquer pessoa que tenha o link do site — a proteção é só "ninguém de fora tem o link", não é uma segurança à prova de tudo. Para uma dívida de viagem entre a família, costuma ser suficiente.

---

## Passo 2 — Publicar no GitHub Pages

1. Crie uma conta em **https://github.com** (se ainda não tiver).
2. Clique em **"New repository"**, dê um nome (ex: `dividas-viagem`), marque como **Public** e clique em **"Create repository"**.
3. Na página do repositório, clique em **"Add file" → "Upload files"**.
4. Arraste os dois arquivos (`index.html` e este `README.md`) para lá e clique em **"Commit changes"**.
5. Vá em **"Settings" → "Pages"** (menu à esquerda).
6. Em **"Branch"**, selecione `main` e a pasta `/ (root)`, depois clique em **"Save"**.
7. Espere 1–2 minutos e recarregue a página — vai aparecer um link tipo:
   `https://seu-usuario.github.io/dividas-viagem/`

Esse é o link que você compartilha com a família — abre direto, sem senha.

---

## Como usar

- Cada quadradinho numerado é uma parcela. Toque para marcar como paga (fica verde) ou desmarcar.
- A mudança aparece para todo mundo que abrir o site, na hora.
- Os valores, pix e datas de vencimento já vêm preenchidos com o que você me passou — se algo mudar de novo (reajuste, saída de alguém), me chama que eu atualizo o `index.html`.
