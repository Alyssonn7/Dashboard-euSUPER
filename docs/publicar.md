# Publicar o painel numa URL

O painel é um arquivo estático único (`painel/index.html`), sem build e sem
dependência. Qualquer hospedagem de site estático serve, e a raiz do repositório
redireciona para `/painel/`.

## Recomendado — Cloudflare Pages (grátis, com login na frente)

Funciona com repositório privado e permite proteger o acesso sem pagar.

1. Em [dash.cloudflare.com](https://dash.cloudflare.com) → **Workers & Pages** →
   **Create** → **Pages** → **Connect to Git**.
2. Autorize o GitHub e escolha `Alyssonn7/dashboard`.
3. Configuração de build:
   - **Framework preset:** None
   - **Build command:** deixe vazio
   - **Build output directory:** `/`
   - **Production branch:** a branch que você quiser servir
4. Salve. Sai uma URL tipo `https://<projeto>.pages.dev`.

### Colocar login na frente (Cloudflare Access, grátis até 50 pessoas)

**Zero Trust** → **Access** → **Applications** → **Add an application** →
**Self-hosted**, aponte para o domínio do Pages e crie uma política
**Allow** com os e-mails de quem pode entrar. Quem abrir o link recebe um
código por e-mail para acessar.

Sem isso, qualquer pessoa com a URL vê os números.

### Domínio próprio, sem custo extra

Você já é dono de `miaapp.com.br`. No projeto do Pages → **Custom domains** →
adicione `painel.miaapp.com.br`. Subdomínio de um domínio que já existe não
custa nada a mais, e fica melhor que qualquer domínio gratuito.

## Alternativa — Netlify ou Vercel

Também grátis, também aceitam repositório privado, e dão
`<nome>.netlify.app` / `<nome>.vercel.app`. Build vazio, diretório de saída `/`.
A diferença é que a proteção por senha nesses dois é recurso pago — por isso a
recomendação é o Cloudflare.

## Por que não GitHub Pages

Este repositório é **privado**, e GitHub Pages em repositório privado exige
plano pago. Na conta gratuita a única forma seria tornar o repositório público —
o que deixaria GMV, cadastros, contas ativas e desempenho de criativos abertos
para qualquer pessoa. Não vale.

## Sobre domínio gratuito de verdade

Os TLDs que eram gratuitos (`.tk`, `.ml`, `.ga`) não são mais uma opção
confiável. O que existe de graça hoje é **subdomínio de hospedagem**
(`.pages.dev`, `.netlify.app`, `.vercel.app`) — ou, melhor ainda, um subdomínio
do domínio que você já tem.
