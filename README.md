# TecDicas

App web único (`index.html` + `dicas.json`), pronto para hospedar no GitHub Pages. Funciona como site normal e também pode ser **instalado como app** no Android, iOS e desktop (PWA).

## 1. Publicar

1. Crie um repositório no GitHub (pode ser público).
2. Suba `index.html`, `dicas.json`, `manifest.json` e a pasta `assets/` (com os ícones) na raiz do repositório — mantenha essa estrutura de pastas.
3. Vá em **Settings → Pages** e ative o GitHub Pages apontando pra branch `main`, pasta `/ (root)`.
4. Seu app fica disponível em `https://SEU-USUARIO.github.io/SEU-REPOSITORIO/`.

## 2. Instalar como app (ícone na tela inicial)

- **Android (Chrome)**: abrir o site → menu (⋮) → "Adicionar à tela inicial" / "Instalar app". O ícone do TecDicas aparece igual a um app nativo.
- **iOS (Safari)**: abrir o site → botão de compartilhar → "Adicionar à Tela de Início". O ícone e o nome "TecDicas" aparecem automaticamente.
- **Desktop (Chrome/Edge)**: ícone de instalação na barra de endereço.

O ícone já vem embutido dentro do `index.html` e do `manifest.json` (não depende mais da pasta `assets/` estar no ar) — se mesmo assim aparecer um ícone genérico com a letra "T", é sinal de que o navegador ainda está com a versão antiga em cache: feche a aba, limpe o cache do site (ou aguarde alguns minutos) e tente adicionar de novo.

## 3. Configurar

Abra `index.html` e edite o bloco `CONFIG` no início do `<script>`:

```js
const CONFIG = {
  GITHUB_OWNER: "SEU-USUARIO",
  GITHUB_REPO: "SEU-REPOSITORIO",
  GITHUB_BRANCH: "main",
  GITHUB_FILE_PATH: "dicas.json",
  ADMIN_PASSWORD_HASH: "..."
};
```

## 4. Trocar a senha do admin

A senha do admin já vem configurada como hash SHA-256 em `ADMIN_PASSWORD_HASH` (o texto puro da senha não fica em nenhum arquivo). Pra trocar, gere o hash da nova senha e cole no lugar do atual.

No terminal (com Python instalado):
```
python3 -c "import hashlib; print(hashlib.sha256('SUA-NOVA-SENHA'.encode()).hexdigest())"
```

Cole o resultado no lugar do hash atual, dentro do bloco `CONFIG` do `index.html`.

> Atenção: como o site é estático, qualquer pessoa pode ver o código-fonte, inclusive esse hash. Isso é proteção básica, suficiente pra uso pessoal — não é criptografia forte. Não reutilize uma senha importante aqui.

## 5. Gerar o Token do GitHub (para salvar as dicas)

O modo admin grava as dicas direto no repositório usando a API do GitHub (também usado pra criar categorias novas e enviar imagens de capa). Pra isso você precisa de um **Personal Access Token**:

1. GitHub → foto de perfil → **Settings**
2. **Developer settings** → **Personal access tokens** → **Fine-grained tokens**
3. Crie um token com acesso apenas ao repositório do app, permissão **Contents: Read and write**
4. Cole esse token quando o app pedir (após o login de admin)

O token não fica salvo em nenhum arquivo do projeto — só na sessão do navegador, a menos que você marque "lembrar neste navegador".

## 6. Usar o modo admin

- Clique em **Admin** no canto superior direito
- Digite a senha
- Cole o token do GitHub (só na primeira vez, ou sempre que não marcar "lembrar")
- Use **+ Nova dica** pra cadastrar, ou os botões **editar/remover** em cada card
- Cada dica tem um **resumo curto** (aparece no card da lista) e um campo de **texto completo** opcional (aparece só na página da dica) — pode deixar em branco, curto ou bem detalhado

Cada alteração gera um commit automático no repositório, e o GitHub Pages atualiza o site publicado em poucos segundos.

## 7. Página da dica

Clicar no título ou na descrição de qualquer card abre a página completa daquela dica (com URL própria, tipo `#dica-abc123`, que pode ser compartilhada). Ali aparecem: o texto completo (ou o resumo, se o texto completo não foi preenchido), o vídeo e as tags. Clicar na miniatura do vídeo ou em "Assistir" no card continua abrindo o vídeo direto, sem passar pela página da dica.

**Plataformas de vídeo aceitas no campo "Link do vídeo":**
- **YouTube** — gera miniatura automática no card e player incorporado na página da dica.
- **TikTok** — precisa ser o link completo do vídeo (`tiktok.com/@usuario/video/123...`), não o link curto (`vm.tiktok.com/...`). Incorpora o vídeo direto na página.
- **Instagram** — link de post, reel ou IGTV público (`instagram.com/p/...`, `/reel/...`). Incorpora o vídeo direto na página (não funciona com contas privadas).
- **Facebook** — link de vídeo público (`facebook.com/.../videos/...` ou `fb.watch/...`). Incorpora o player direto na página.
- **Qualquer outro link** — funciona como um botão "Assistir vídeo" que abre em outra aba.

Nos cards da lista, quando não é possível gerar miniatura automática (todas as plataformas exceto YouTube, a menos que você preencha uma "Capa"), aparece um card colorido identificando a plataforma (TikTok, Instagram ou Facebook).

## 8. Categorias

As categorias padrão ficam definidas no `index.html`, mas você pode criar novas direto pelo formulário de cadastro: no campo "Categoria", escolha **"+ Nova categoria..."**, dê um nome e escolha uma cor. Isso cria (ou atualiza) automaticamente o arquivo `categorias.json` no repositório — não precisa mexer em nada manualmente. Esse arquivo só é criado no primeiro uso; até lá, o app usa as categorias padrão internas.

## 9. Capa por upload

No cadastro de uma dica, além de colar um link de imagem, dá pra **enviar um arquivo direto do computador ou celular** no campo "Ou envie um arquivo". A imagem é enviada pro repositório (pasta `capas/`) via GitHub e o link gerado é salvo automaticamente na dica. Isso só é necessário quando o vídeo não é do YouTube (o YouTube já gera a miniatura sozinho).
