# Publicar systemize.tec.br no GitHub Pages

Os nameservers continuam no Registro.br. Você só adiciona registros A e CNAME na zona DNS que já existe; os registros de e-mail (MX, SPF, DKIM) não são tocados.

## 1. Repositório

1. Crie um repositório **público** no GitHub, por exemplo `systemize-site`. No plano gratuito, o GitHub Pages só publica repositórios públicos.
2. Envie todo o conteúdo desta pasta para a raiz da branch `main`, incluindo os arquivos `CNAME` e `.nojekyll`. Eles começam com ponto ou não têm extensão e alguns sistemas os escondem.

```bash
cd systemize-site
git init -b main
git add .
git commit -m "Site institucional Systemize"
git remote add origin https://github.com/<SEU-USUARIO>/systemize-site.git
git push -u origin main
```

3. Em **Settings > Pages**, escolha **Source: Deploy from a branch**, branch `main`, pasta `/ (root)` e salve.
4. Aguarde 1 a 2 minutos e confira se o site abre em `https://<SEU-USUARIO>.github.io/systemize-site/`.
   Nesse endereço provisório, imagens e favicon podem não carregar (os caminhos são da raiz do domínio). Isso se resolve no passo 3.

## 2. Verificar o domínio no GitHub (recomendado)

Protege contra outra conta do GitHub usar o seu domínio.

1. Na sua conta (não no repositório): **Settings > Pages > Add a domain**, informe `systemize.tec.br`.
2. O GitHub mostra um registro TXT, no formato `_github-pages-challenge-<SEU-USUARIO>` com um valor.
3. Crie esse TXT no Registro.br (passo 3) e volte para clicar em **Verify**.

## 3. DNS no Registro.br

Registro.br > seu domínio > **DNS > Editar zona**. Adicione:

| Tipo | Nome | Valor |
| --- | --- | --- |
| A | (vazio, raiz) | 185.199.108.153 |
| A | (vazio, raiz) | 185.199.109.153 |
| A | (vazio, raiz) | 185.199.110.153 |
| A | (vazio, raiz) | 185.199.111.153 |
| AAAA (opcional) | (vazio, raiz) | 2606:50c0:8000::153 |
| AAAA (opcional) | (vazio, raiz) | 2606:50c0:8001::153 |
| AAAA (opcional) | (vazio, raiz) | 2606:50c0:8002::153 |
| AAAA (opcional) | (vazio, raiz) | 2606:50c0:8003::153 |
| CNAME | www | `<SEU-USUARIO>.github.io` |
| TXT | `_github-pages-challenge-<SEU-USUARIO>` | valor mostrado pelo GitHub |

Antes de salvar, confira:

- **Não apague nem altere** os registros MX, o TXT do SPF (`v=spf1 include:_spf.google.com ~all`), o DKIM (`google._domainkey`) nem o TXT de verificação do Google. São eles que mantêm o seu e-mail funcionando.
- Se já existir um registro A ou CNAME na raiz ou no `www` apontando para outro lugar, remova só esse.
- O CNAME do `www` aponta para `<SEU-USUARIO>.github.io`, **sem** o nome do repositório.

## 4. Domínio no repositório e HTTPS

1. No repositório: **Settings > Pages > Custom domain**: `systemize.tec.br` e salve. O arquivo `CNAME` já traz esse valor.
2. Aguarde a verificação de DNS ficar verde (de minutos a algumas horas, conforme a propagação).
3. Quando a opção liberar, marque **Enforce HTTPS**. O certificado pode levar até 1 hora para ser emitido.
4. O `www.systemize.tec.br` passa a redirecionar automaticamente para `systemize.tec.br`.

Para conferir pelo terminal:

```bash
dig systemize.tec.br +noall +answer -t A
dig www.systemize.tec.br +noall +answer
dig systemize.tec.br +noall +answer -t MX   # e-mail continua igual
```

## 5. Logo da assinatura de e-mail

Com o site no ar, o logo da assinatura passa a ter endereço público:

`https://systemize.tec.br/assets/logo-email.png`

No editor de assinatura do Gmail: apague a imagem atual, clique em **Inserir imagem > URL da Web**, cole esse endereço e defina a largura como 150px.

## Alterações futuras

Edite o `index.html`, faça commit e push na `main`. O GitHub publica em cerca de 1 minuto.

## Pendências no conteúdo

- **Seção de projetos:** está comentada no `index.html` até os cases serem definidos.
- **LinkedIn:** comentado no rodapé. Descomente e preencha quando quiser exibir.
- **Dados da empresa:** o rodapé e a página `/politica-de-privacidade/` mostram razão social, CNPJ e endereço iguais ao Cartão CNPJ. A verificação da empresa na Meta compara o site com o documento: se algum desses dados mudar na Receita, atualize aqui também.
