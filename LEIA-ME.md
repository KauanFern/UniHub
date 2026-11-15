# UniHub · portal acadêmico (protótipo)

Site estático (HTML, CSS e JavaScript puros, sem instalação). Os dados são de demonstração.

## Estrutura

```
index.html                 página única (rotas por #, ex.: /#notas)
favicon.ico
robots.txt                 pede aos buscadores que NÃO indexem (ambiente de testes)
_headers                   cabeçalhos de segurança (Netlify e Cloudflare Pages)
.htaccess                  cabeçalhos de segurança (Apache / cPanel)
assets/
  css/estilos.css          estilos, temas claro e escuro
  js/tema-inicial.js       tema salvo, antes da tela aparecer
  js/config.js             configuração do ambiente
  js/app.js                telas, rotas, login e camada de dados ("api")
  js/interface.js          botão de tema, busca no celular, menu no tablet
  img/                     logos, ícones e fotos:
                           hero-hub.jpg, hero-biblioteca.jpg, hero-anfiteatro.jpg  (carrossel do Início)
                           biblioteca.jpg                                          (Biblioteca Virtual)
                           disc-<disciplina>.jpg                                   (miniaturas das disciplinas)
```

## Testar no seu computador

Na pasta do site: `python3 -m http.server 8080` e abra http://localhost:8080
(Teste por um servidor, como será no domínio. Abrir o index.html direto também funciona, mas sem os cabeçalhos de segurança.)

## Publicar no domínio

- **Hospedagem comum (cPanel, FTP):** envie o CONTEÚDO desta pasta para `public_html` (ou para uma subpasta). O `.htaccess` aplica os cabeçalhos. Ative o HTTPS (AutoSSL / Let's Encrypt).
- **Netlify ou Cloudflare Pages:** arraste a pasta (ou o zip). O `_headers` é lido automaticamente. Conecte o domínio e deixe o HTTPS automático ligado.
- **GitHub Pages:** funciona, mas não aplica `_headers` nem `.htaccess` (sem a política de segurança).
- Funciona na raiz do domínio e em subpastas (ex.: seudominio.com/teste/), pois os caminhos são relativos.

## Conferir depois de publicar

1. Abra o site e o console (F12): não deve haver erros em vermelho.
2. Logos e foto aparecem (tela de login, cabeçalho, Início).
3. Login de demonstração (veja abaixo), "Esqueceu sua senha?" e "Sair da conta" (em Configurações).
4. Tema claro/escuro e a escolha continua após recarregar.
5. Larguras de 360 px, 768 px e 1366 px, sem rolagem lateral.
6. Adicione `?erro=1` ao endereço para ver a tela de erro de carregamento.
7. Cabeçalhos: https://securityheaders.com

Conta de demonstração: RA 2026001 ou CPF 529.982.247-25, senha demo1234.
Agenda (teste): a 1ª atividade conclui com sucesso e permite "Cancelar envio" (com confirmação); a 2ª simula um erro no envio. Para mudar isso, edite `simularErro` em `DEMO_ACTS` no `app.js`.

## Configuração (assets/js/config.js)

- `mostrarContaDemo`: `false` esconde a dica da conta de demonstração no login.
- `latenciaMs`: atraso simulado dos dados de exemplo (use 0 ao ligar o back-end).
- `envioMs`: duração simulada do envio de atividade na Agenda (a tela de carregamento fica visível por esse tempo).
- `localizacaoUrl`: link do item "Encontre sua sala!" no Início (abre em nova aba).

## Ao atualizar o site

Os arquivos têm `?v=0.3.0` no `index.html` para evitar cache antigo. Ao publicar uma mudança, troque o número nos cinco lugares (CSS, tema-inicial, config, app, interface).

## Antes de abrir para alunos reais

- Remova `noindex` do `index.html` e o `Disallow` do `robots.txt` só quando quiser aparecer em buscas.
- Remova a conta de demonstração e os dados de exemplo (`DEMO_*`, `DEMO_ACCOUNT`) do `app.js`.
- A política de segurança permite estilos inline (`style-src 'unsafe-inline'`) porque as telas usam atributos `style`. Dá para endurecer depois.
- Ative o HSTS (linha comentada em `_headers` e `.htaccess`) somente com HTTPS funcionando.

## Ligando o back-end

Todos os dados passam pelo objeto `api` em `assets/js/app.js` (`entrar`, `recuperar`, `usuario`, `disciplinas`, `atividades`, `carregar`). Basta trocar o conteúdo dessas funções por chamadas à API; as telas não mudam.

- Sessão por cookie `httpOnly` + `Secure` + `SameSite`, não em `localStorage`.
- O e-mail mascarado deve ser montado no servidor (hoje a função `mascararEmail` simula isso).
- Limite de tentativas de login e de recuperação de senha no servidor. Notas, frequência e dados financeiros são dados pessoais (LGPD): a regra de acesso fica no servidor.
- Adicione o domínio da API em `connect-src` (política de segurança) e configure o CORS.

## Trocar as imagens

Substitua o arquivo mantendo o mesmo nome. Para o carrossel, use imagens de pelo menos 1600 px de largura, sem texto desenhado e com o assunto principal à direita (o texto de boas-vindas cobre a esquerda). Para as miniaturas das disciplinas, pelo menos 560×400 px. Depois, aumente o número da versão (`?v=`) no `index.html`.
