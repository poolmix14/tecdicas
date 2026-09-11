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

Isso funciona porque o `manifest.json` e as tags de ícone já estão configurados no `index.html` — não precisa mexer em nada além de publicar os arquivos.

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

O modo admin grava as dicas direto no `dicas.json` do repositório usando a API do GitHub. Pra isso você precisa de um **Personal Access Token**:

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

Clicar no título ou na descrição de qualquer card abre a página completa daquela dica (com URL própria, tipo `#dica-abc123`, que pode ser compartilhada). Ali aparecem: o texto completo (ou o resumo, se o texto completo não foi preenchido), o vídeo (incorporado quando é do YouTube, ou um link quando for de outro serviço) e as tags. Clicar na miniatura do vídeo ou em "Assistir" no card continua abrindo o vídeo direto, sem passar pela página da dica.

## 8. Categorias

As categorias ficam definidas no array `CATEGORIES` dentro do `index.html`. Pra adicionar uma nova categoria, adicione um item nesse array com `id`, `label` e uma `color` (pode usar uma nova variável CSS em `:root`).
