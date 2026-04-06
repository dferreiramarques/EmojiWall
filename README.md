# 🎨 EmojiWall

> Cria wallpapers personalizados com emojis para Android — direto no browser, sem instalação, sem servidor.

![EmojiWall](https://img.shields.io/badge/PWA-ready-7c6af7?style=flat-square&logo=pwa)
![License](https://img.shields.io/badge/license-MIT-f76ac8?style=flat-square)
![Zero dependencies](https://img.shields.io/badge/dependencies-zero-6af7d4?style=flat-square)

---

## ✨ Funcionalidades

### 🖼️ Modos de visualização
| Modo | Descrição |
|---|---|
| **Sistema** | Emojis nativos do dispositivo (Android / iOS / Windows) |
| **Twemoji** | Estilo Twitter/X — carregado via CDN, consistente entre dispositivos |
| **Mono** | Escala de cinzas com contraste aumentado |
| **Sépia** | Tom vintage amarelado |
| **Neon** | Brilho colorido multi-camadas — cor e intensidade configuráveis |
| **Silhueta** | Emoji como forma sólida — cor personalizável |
| **Contorno** | Apenas a borda do emoji — cor e espessura configuráveis |
| **Invertido** | Cores complementares |

### 🔲 Padrões de distribuição
- **Grelha** — alinhamento perfeito em linhas e colunas
- **Desfasado** — grelha com offset alternado entre linhas
- **Aleatório** — distribuição pseudo-aleatória com seed reproduzível
- **Diagonal** — emojis em diagonais a 30°
- **Hexagonal** — distribuição honeycomb
- **Espiral** — emojis dispostos em espiral a partir do centro

### 📐 Controles de layout
- Tamanho base (30–180px)
- Variação aleatória de tamanho (0–100%)
- Espaçamento entre emojis (1.0×–2.5×)
- Rotação aleatória (0–180°)
- Opacidade global (20–100%)

### 🎨 Fundo
- 16 cores predefinidas
- Cor personalizada via color picker
- Gradiente de fundo com 2 cores

### 📖 Emojipedia integrada
- +500 emojis organizados em 10 categorias
- Paginação (40 emojis por página)
- Seleção direta — clica para adicionar/remover
- Categorias: Populares, Caras, Gestos, Animais, Natureza, Comida, Desporto, Viagem, Objetos, Símbolos

### 📱 Resoluções suportadas
| Nome | Resolução |
|---|---|
| HD+ | 720 × 1600 |
| FHD | 1080 × 1920 |
| FHD+ | 1080 × 2400 |
| QHD+ | 1440 × 3200 |

### 💾 Exportação
- PNG de alta resolução (resolução real selecionada, não o preview)
- Nome de ficheiro automático com modo, resolução e timestamp

---

## 🚀 Como usar

### Opção A — Direto no browser
1. Faz download do ficheiro `emoji-wallpaper.html`
2. Abre-o no Chrome para Android
3. Configura os emojis, padrão e estilo
4. Toca em **💾 Guardar Wallpaper** para exportar

### Opção B — Instalar como PWA no Android
1. Abre o ficheiro num servidor HTTPS (ver abaixo) ou via GitHub Pages
2. No Chrome, toca nos ⋮ três pontos
3. Seleciona **"Adicionar ao ecrã inicial"**
4. A app fica disponível como app nativa

> **Nota:** O botão **"📲 Instalar App"** aparece automaticamente no cabeçalho quando o Chrome deteta que a PWA é instalável (requer HTTPS).

### Opção C — Servidor local rápido
```bash
# Python
python3 -m http.server 8080

# Node.js (npx)
npx serve .

# PHP
php -S localhost:8080
```
Depois abre `http://localhost:8080/emoji-wallpaper.html` no browser.

---

## 📂 Estrutura do projeto

```
emojiwall/
├── emoji-wallpaper.html   # App completa (single-file)
└── README.md
```

O projeto é intencionalmente um **único ficheiro HTML** sem dependências externas (exceto a fonte Google Fonts e a CDN do Twemoji para o modo Twemoji). Pode ser partilhado, alojado ou aberto diretamente.

---

## 🌐 Deploy no GitHub Pages

1. Cria um repositório no GitHub
2. Faz upload do `emoji-wallpaper.html` e do `README.md`
3. Vai a **Settings → Pages → Source → Deploy from branch → main**
4. Acede a `https://<username>.github.io/<repo>/emoji-wallpaper.html`
5. O botão de instalação PWA ficará ativo automaticamente

---

## 🛠️ Tecnologias

| Tecnologia | Uso |
|---|---|
| **Canvas API** | Renderização dos emojis e exportação PNG |
| **CSS Filter API** | Modos mono, sépia, invertido |
| **Web App Manifest** | Instalação PWA |
| **Twemoji CDN** | Emojis estilo Twitter/X (`cdn.jsdelivr.net`) |
| **Google Fonts** | Tipografia (Syne + DM Sans) |
| **Intl.Segmenter** | Parsing correto de emojis multi-codepoint |

Sem frameworks. Sem bundlers. Sem `node_modules`. JavaScript vanilla puro.

---

## 🔧 Personalização

### Adicionar categorias à Emojipedia
No objeto `EP_CATS` dentro do script:
```js
const EP_CATS = {
  '🦄 A minha categoria': ['🦄','✨','🌈', /* ... */],
  // categorias existentes...
};
```

### Adicionar um modo de renderização
1. Adiciona uma entrada ao array `MODES`
2. Adiciona o case correspondente no `switch(S.mode)` dentro de `doRender()`
3. Opcionalmente cria uma secção de opções em `#opts-<id>`

### Alterar resoluções disponíveis
No HTML, edita os botões `.rbtn`:
```html
<div class="rbtn" data-w="1290" data-h="2796" onclick="selRes(this)">
  <div class="rn">iPhone 15 Pro Max</div>
  <div class="rd">1290 × 2796</div>
</div>
```

---

## 📋 Notas técnicas

- **Service Worker:** Requer HTTPS para registar um SW e funcionar offline. Num ficheiro local (`file://`) ou via GitHub Pages sem SW configurado, a app funciona normalmente mas sem cache offline.
- **Twemoji:** O modo Twemoji faz fetch de imagens PNG de 72×72 da CDN do jsDelivr. As imagens são cacheadas em memória durante a sessão.
- **Seed RNG:** O padrão aleatório usa um gerador linear congruente com seed reproduzível — o botão 🔀 gera uma nova seed para variar a disposição sem alterar as definições.
- **Silhueta:** Implementada via canvas offscreen com `globalCompositeOperation: 'source-in'` para colorir a forma do emoji preservando a transparência.

---

## 📄 Licença

MIT — faz o que quiseres, com ou sem atribuição.

---

*Feito com 🎨 e zero dependências.*
