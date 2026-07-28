# Lojinha do Perigo — App

Este site foi transformado em um **PWA** (Progressive Web App): depois de publicado, qualquer pessoa pode "instalar" o app pelo navegador — ganha ícone, abre em janela própria (sem barra do navegador) e continua funcionando offline.

## 1. Publicar no GitHub (GitHub Pages)

1. Crie um repositório novo no GitHub (pode ser público ou privado).
2. Envie todos os arquivos desta pasta para o repositório (pela interface do GitHub em **Add file → Upload files**, ou por `git push` se preferir usar o terminal).
3. No repositório, vá em **Settings → Pages**.
4. Em "Build and deployment", escolha **Deploy from a branch**.
5. Em "Branch", selecione `main` e a pasta `/ (root)`. Clique em **Save**.
6. Espere 1–2 minutos. O GitHub vai mostrar o link do site, algo como:
   `https://seu-usuario.github.io/nome-do-repositorio/`

Esse link é o que você compartilha com todo mundo (no grupo do WhatsApp, por exemplo).

## 2. Como as pessoas "baixam" o app

- **Android / Chrome / Edge (celular ou PC):** ao abrir o link, aparece um botão **"Instalar"** dentro do próprio site (perto do botão "Sistema"). Também dá pra instalar pelo menu do navegador (⋮ → "Instalar app" / "Adicionar à tela inicial").
- **iPhone / iPad (Safari):** o iOS não mostra o botão automático. É preciso abrir o link no Safari, tocar em **Compartilhar** (o ícone do quadrado com seta) e depois em **"Adicionar à Tela de Início"**.
- **Computador (Chrome/Edge):** clique no ícone de instalar que aparece na barra de endereço, ou use o botão "Instalar" do site.

Depois de instalado, o app abre com ícone próprio, sem a barra do navegador — igual um aplicativo de verdade.

## 3. Importante sobre os dados

Os pedidos, o financeiro e os créditos ficam salvos no `localStorage` do navegador — ou seja, **cada aparelho guarda os próprios dados, sem sincronizar entre eles**. Se a ideia é que, por exemplo, um cliente faça o pedido no celular dele e a loja veja esse pedido em outro aparelho, isso hoje não acontece automaticamente — cada instalação do app é isolada. Se isso for importante pra vocês, dá pra evoluir depois com um banco de dados compartilhado (posso ajudar quando quiser).

## 4. Atualizando o site depois

Sempre que editar `index.html` (ou qualquer outro arquivo) e reenviar para o GitHub:

1. Abra `service-worker.js` e troque o valor de `CACHE_NAME` (ex: de `v1` para `v2`).
2. Suba os arquivos atualizados de novo.

Isso garante que quem já instalou o app receba a versão nova na próxima vez que abrir com internet.

## Estrutura dos arquivos

```
index.html              → o site/app em si
manifest.json           → configurações do app (nome, ícone, cores)
service-worker.js       → permite instalar e funcionar offline
icons/                  → ícones do app em vários tamanhos
```
