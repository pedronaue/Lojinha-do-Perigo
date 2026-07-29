# Lojinha do Perigo — App (PWA com dados compartilhados)

Este app funciona como um "aplicativo" instalável (PWA) e guarda os dados
(pedidos, financeiro e créditos) num banco de dados compartilhado na nuvem
(Firebase/Firestore, gratuito), para que qualquer dispositivo que abrir o
sistema com a senha veja os mesmos dados, em tempo real.

Sem esse passo de configuração abaixo, o site abre normalmente mas mostra um
aviso de "backend não configurado" e não deixa salvar pedidos, saldo ou
créditos — é rapidinho de resolver, leva uns 5 minutos.

## Passo 1 — Criar o projeto gratuito no Firebase

1. Acesse https://console.firebase.google.com e entre com uma conta Google.
2. Clique em **Criar projeto** (Add project), dê um nome (ex: `lojinha-do-perigo`) e siga os passos (pode desativar o Google Analytics, não é necessário).
3. Dentro do projeto, no menu lateral, clique em **Compilação (Build) → Firestore Database**.
4. Clique em **Criar banco de dados**. Escolha a localização (ex: `southamerica-east1`) e comece em **modo de teste** (test mode) — isso já libera leitura/escrita por 30 dias, dá pra trocar as regras depois (veja o Passo 3).
5. Ainda no console, clique no ícone de **engrenagem → Configurações do projeto**. Em "Seus aplicativos", clique no ícone `</>` (Web) para criar um app da Web, dê um nome e clique em registrar.
6. O Firebase vai mostrar um bloco `firebaseConfig = {...}` com `apiKey`, `authDomain`, `projectId` etc. **Copie esse bloco inteiro.**

## Passo 2 — Colar a configuração no arquivo

1. Abra o arquivo `index.html` num editor de texto.
2. Procure por este trecho, perto do início do `<script type="module">`:

   ```js
   const firebaseConfig = {
     apiKey: "SUA_API_KEY_AQUI",
     authDomain: "SEU_PROJETO.firebaseapp.com",
     projectId: "SEU_PROJETO",
     storageBucket: "SEU_PROJETO.appspot.com",
     messagingSenderId: "SEU_SENDER_ID",
     appId: "SEU_APP_ID"
   };
   ```

3. Substitua pelos valores reais que o Firebase te deu no Passo 1.6 e salve o arquivo.

## Passo 3 — Regras do Firestore (recomendado)

O "modo de teste" do Firebase libera tudo por 30 dias e depois bloqueia
automaticamente. Para deixar funcionando de forma permanente (as senhas do
próprio app continuam protegendo quem consegue *ver* as telas), vá em
**Firestore Database → Regras** e cole:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /pedidos/{doc} {
      allow read, write: if true;
    }
    match /movimentos/{doc} {
      allow read, write: if true;
    }
    match /config/{doc} {
      allow read, write: if true;
    }
  }
}
```

Isso mantém o banco público (qualquer um com o link do app consegue ler/escrever
diretamente na API do Firebase, não só pela tela) — para uma lojinha pequena
isso costuma ser aceitável, mas não é segurança de verdade. Se no futuro
quiser travar isso direito (com login de funcionário, por exemplo), é possível
adicionar o Firebase Authentication depois — é só pedir.

## Passo 4 — Publicar no GitHub Pages (para virar um link/app baixável)

1. Crie um repositório novo no GitHub (ex: `lojinha-do-perigo`).
2. Envie estes arquivos para o repositório, mantendo a mesma estrutura de pastas:
   - `index.html`
   - `manifest.json`
   - `service-worker.js`
   - `icons/icon-192.png`
   - `icons/icon-512.png`
3. No repositório, vá em **Settings → Pages**.
4. Em "Branch", selecione `main` (ou `master`) e a pasta `/ (root)`, depois clique em **Save**.
5. Espere 1–2 minutos. O GitHub vai te dar um link tipo:
   `https://SEU-USUARIO.github.io/lojinha-do-perigo/`
6. Abra esse link no celular ou computador. No navegador vai aparecer a opção
   **"Adicionar à tela inicial"** (celular) ou **"Instalar app"** (ícone na
   barra de endereço, no computador) — a partir daí funciona como um app de
   verdade, com ícone próprio, mesmo offline para abrir a tela (os dados de
   pedidos/financeiro precisam de internet para sincronizar).

## Como os dados ficam compartilhados

- **Pedidos**: toda vez que alguém preenche "Encomendar" e clica em Concluir, o pedido é salvo no Firestore. Qualquer dispositivo que abrir "Sistema" (senha `Naue2311`) vê a lista atualizada em tempo real.
- **Financeiro**: ao registrar "Adicionar saldo" ou "Retirar saldo" na aba Financeiro (senha `Naue2311`), o valor entra no histórico e o saldo total é recalculado — visível para todos os dispositivos.
- **Créditos** (Contribuidores/Deckers): ao editar pelo lápis (senha `Pjnf2311`), a alteração é salva e aparece automaticamente para quem já estiver com o app aberto em outro aparelho.

## Senhas

- Editar créditos: `Pjnf2311`
- Financeiro: `Naue2311`
- Sistema (pedidos): `Naue2311`

Essas senhas ficam gravadas no próprio código do `index.html` — funcionam
como uma trava simples de tela, não como autenticação real. Qualquer pessoa
com acesso ao código-fonte do site consegue vê-las.
