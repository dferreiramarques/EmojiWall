# 🎨 EmojiWall

> Cria wallpapers personalizados com emojis para Android — PWA, zero dependências, single-file.

![PWA](https://img.shields.io/badge/PWA-ready-7c6af7?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-f76ac8?style=flat-square)
![Zero dependencies](https://img.shields.io/badge/dependencies-zero-6af7d4?style=flat-square)

---

## 🚀 Deploy

### Opção 1 — GitHub Pages (recomendado, grátis)

É a opção mais simples. Não precisas de servidor, não pagas nada, HTTPS incluído.

```bash
# 1. Cria o repositório
git init
git add .
git commit -m "feat: initial commit"
git remote add origin https://github.com/<username>/emojiwall.git
git push -u origin main
```

Depois no GitHub:
1. **Settings → Pages → Source → Deploy from branch → main → / (root)**
2. Aguarda ~1 minuto
3. A app fica disponível em `https://<username>.github.io/emojiwall/`

O botão **📲 Instalar** aparece automaticamente no Chrome para Android.

---

### Opção 2 — Railway

O Railway é para apps com servidor (Node, Python, etc.). Para um site estático como este, **não é a melhor escolha** — vais pagar por um servidor que não precisas.

Se mesmo assim quiseres usar o Railway, precisas de adicionar dois ficheiros ao repositório:

**`package.json`**
```json
{
  "name": "emojiwall",
  "scripts": {
    "start": "npx serve . -l $PORT"
  }
}
```

**`railway.toml`**
```toml
[deploy]
startCommand = "npm start"
```

Depois:
1. Cria conta em [railway.app](https://railway.app)
2. **New Project → Deploy from GitHub repo**
3. Liga o repositório
4. O Railway deteta o `package.json` e faz deploy automático

> ⚠️ O Railway tem um free tier limitado a 500 horas/mês e suspende o serviço quando inativo. Para um site público, o GitHub Pages é definitivamente melhor.

---

## 📂 Ficheiros do projeto

```
emojiwall/
├── index.html     # App completa (single-file, todo o CSS e JS inline)
├── sw.js          # Service Worker para cache offline e instalação PWA
└── README.md
```

> **Importante:** o `sw.js` tem de estar na mesma pasta que o `index.html`. Sem ele, a PWA não instala nem funciona offline.

---

## ✨ Funcionalidades

### 🖼️ Modos de visualização
| Modo | Descrição |
|---|---|
| **Sistema** | Emojis nativos do dispositivo |
| **Glifo** | Forma plana monocromática — cor personalizável |
| **Mono** | Escala de cinzas |
| **Sépia** | Tom vintage |
| **Neon** | Brilho multi-camadas — cor e intensidade configuráveis |
| **Silhueta** | Forma sólida colorida |
| **Invertido** | Cores complementares |

### 🔲 Padrões
Grelha · Desfasado · Aleatório · Diagonal · Hexagonal · Espiral

### 🎛️ Controlos
- Tamanho base + variação aleatória
- Espaçamento e rotação
- Opacidade global
- Cor de fundo sólida ou gradiente
- Botão 🔀 para nova disposição com a mesma seed

### 📖 Emojipedia
+500 emojis em 10 categorias com paginação. Toca para adicionar/remover.

### 📱 Resoluções de exportação
HD+ (720×1600) · FHD (1080×1920) · FHD+ (1080×2400) · QHD+ (1440×3200)

### 💾 Exportação
O botão **Guardar Wallpaper** renderiza um canvas separado na resolução real e descarrega PNG de alta qualidade. A pré-visualização no ecrã é apenas um thumbnail.

---

## 📲 Instalar como PWA no Android

Com o site no ar via GitHub Pages ou Railway:

1. Abre o URL no **Chrome para Android**
2. O botão **📲 Instalar** aparece automaticamente no cabeçalho
3. Toca → confirma → fica no ecrã inicial como app nativa

Se o botão não aparecer:
- Menu ⋮ → **"Adicionar ao ecrã inicial"**

O botão esconde-se automaticamente se a app já estiver instalada.

---

## 🛠️ Tecnologias

| Tecnologia | Uso |
|---|---|
| **Canvas API** | Renderização e exportação PNG |
| **ImageData pixel manipulation** | Modo Glifo (threshold + colorização) |
| **CSS Filter API** | Modos mono, sépia, invertido |
| **Web App Manifest** | Ícone e metadata da PWA |
| **Service Worker** | Cache offline (`sw.js`) |
| **beforeinstallprompt** | Botão de instalação nativo |
| **Figtree** (Google Fonts) | Tipografia |
| **Intl.Segmenter** | Parsing correto de emojis multi-codepoint |

Zero frameworks. Zero `node_modules`. JavaScript vanilla puro.

---

## 🔧 Personalização

### Adicionar emojis à Emojipedia
```js
// Em index.html, no objeto EP_CATS:
const EP_CATS = {
  '🦄 A minha categoria': ['🦄', '✨', '🌈'],
  // ...
};
```

### Adicionar um modo de renderização
1. Adiciona ao array `MODES`: `{ id:'meuModo', name:'Nome', bg:'#000' }`
2. Adiciona o `case 'meuModo':` no `switch(S.mode)` dentro de `doRender()`
3. Opcionalmente cria `<div class="mopt-sec" id="opts-meuModo">` com opções

### Versionar o cache do Service Worker
Incrementa o número em `sw.js` a cada deploy para garantir que os utilizadores recebem a versão mais recente:
```js
const CACHE = 'emojiwall-v2'; // incrementar a cada deploy
```

---

## 📋 Notas técnicas

- **Exportação vs Preview:** o preview é um canvas redimensionado. O botão "Guardar" cria um canvas separado na resolução real antes de exportar.
- **Service Worker:** funciona apenas com HTTPS. Localmente via `file://` falha silenciosamente — usa `python3 -m http.server` para testar offline.
- **Modo Glifo:** `grayscale + contrast(999)` no canvas offscreen, seguido de threshold pixel-a-pixel com `ImageData` para colorização monocromática.
- **Seed RNG:** gerador linear congruente — mesma seed = mesmo layout, sempre reproduzível.

---

## 📄 Licença

MIT

---

*vibe coded by [David Marques](https://www.linkedin.com/in/dferreiramarques/) · [☕ ko-fi](https://ko-fi.com/dferreiramarques)*
