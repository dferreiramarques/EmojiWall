# 🎨 EmojiWall

> Cria wallpapers personalizados com emojis para Android — direto no browser, sem instalação, sem servidor.

![PWA](https://img.shields.io/badge/PWA-ready-7c6af7?style=flat-square&logo=pwa)
![License](https://img.shields.io/badge/license-MIT-f76ac8?style=flat-square)
![Zero dependencies](https://img.shields.io/badge/dependencies-zero-6af7d4?style=flat-square)

---

## ✨ Funcionalidades

### 🖼️ Modos de visualização
| Modo | Descrição |
|---|---|
| **Sistema** | Emojis nativos do dispositivo (Android / iOS / Windows) |
| **Glifo** | Emoji renderizado como glifo plano monocromático — cor personalizável |
| **Mono** | Escala de cinzas com contraste aumentado |
| **Sépia** | Tom vintage amarelado |
| **Neon** | Brilho colorido multi-camadas — cor e intensidade configuráveis |
| **Silhueta** | Emoji como forma sólida — cor personalizável |
| **Invertido** | Cores complementares invertidas |

> **Como funciona o modo Glifo:** renderiza o emoji num canvas offscreen com filtro `grayscale + contrast(999)`, depois faz uma passagem pixel a pixel com threshold para converter para glifo plano monocromático na cor escolhida — sem as cores originais do emoji.

### 🔲 Padrões de distribuição
| Padrão | Descrição |
|---|---|
| **Grelha** | Alinhamento perfeito em linhas e colunas |
| **Desfasado** | Grelha com offset alternado entre linhas (brick layout) |
| **Aleatório** | Distribuição pseudo-aleatória com seed reproduzível |
| **Diagonal** | Emojis em diagonais a 30° |
| **Hexagonal** | Distribuição honeycomb |
| **Espiral** | Emojis dispostos em espiral a partir do centro |

### 📐 Controles de layout
- Tamanho base (30–180px)
- Variação aleatória de tamanho (0–100%)
- Espaçamento entre emojis (1.0×–2.5×)
- Rotação aleatória (0–180°)
- Opacidade global (20–100%)
- Botão 🔀 para nova disposição mantendo todas as definições

### 🎨 Fundo
- 16 cores predefinidas
- Cor personalizada via color picker
- Gradiente de fundo com 2 cores

### 📖 Emojipedia integrada
- +500 emojis organizados em 10 categorias
- Paginação (40 emojis por página)
- Seleção direta — clica para adicionar/remover (fica marcado a roxo)
- Categorias: Populares, Caras, Gestos, Animais, Natureza, Comida, Desporto, Viagem, Objetos, Símbolos

### 📱 Resoluções de exportação
| Nome | Resolução |
|---|---|
| HD+ | 720 × 1600 |
| FHD | 1080 × 1920 |
| FHD+ | 1080 × 2400 |
| QHD+ | 1440 × 3200 |

### 💾 Exportação
- Botão principal **"Guardar Wallpaper"** — renderiza um canvas separado na resolução real e descarrega PNG de alta qualidade
- Nome de ficheiro automático com modo, resolução e timestamp
- A pré-visualização no ecrã é apenas um preview — o ficheiro exportado tem a resolução completa

---

## 🚀 Como usar

### Opção A — Direto no browser (local)
1. Faz download do `index.html`
2. Abre no Chrome para Android
3. Configura os emojis, padrão e estilo
4. Toca em **💾 Guardar Wallpaper**

### Opção B — GitHub Pages (recomendado, HTTPS grátis)
1. Cria um repositório público no GitHub
2. Faz upload de `index.html` e `README.md`
3. Vai a **Settings → Pages → Source → Deploy from branch → main**
4. Acede a `https://<username>.github.io/<repo>/`
5. O botão **📲 Instalar App** aparece automaticamente no Chrome para Android

### Opção C — Servidor local
```bash
# Python
python3 -m http.server 8080

# Node.js
npx serve .

# PHP
php -S localhost:8080
```

---

## 📲 Instalar como PWA no Android

O EmojiWall é uma Progressive Web App. Para instalar:

**Via botão automático:**
1. Abre o URL no Chrome para Android
2. O botão **📲 Instalar App** aparece no cabeçalho
3. Toca nele e confirma

**Via menu do browser:**
1. Abre o URL no Chrome
2. Toca nos ⋮ três pontos → **"Adicionar ao ecrã inicial"**
3. A app fica disponível como app nativa com ícone próprio

> **Nota:** A instalação PWA e o botão automático requerem HTTPS. Funciona nativamente via GitHub Pages. A partir de `file://` local, o botão não aparece mas a app funciona normalmente.

---

## 📂 Estrutura do projeto

```
emojiwall/
├── index.html   # App completa (single-file, zero dependências)
└── README.md
```

Projeto intencionalmente **single-file** — sem `node_modules`, sem bundler, sem build step. Pode ser partilhado como ficheiro único, alojado em qualquer servidor estático ou aberto diretamente no browser.

---

## 🛠️ Tecnologias

| Tecnologia | Uso |
|---|---|
| **Canvas API** | Renderização dos emojis e exportação PNG |
| **OffscreenCanvas (manual)** | Modo Glifo e Silhueta via composição de pixels |
| **CSS Filter API** | Modos mono, sépia, invertido, glifo |
| **ImageData pixel manipulation** | Threshold e colorização no modo Glifo |
| **Web App Manifest** | Instalação PWA com ícone gerado via Canvas |
| **beforeinstallprompt** | Botão de instalação nativo no Android |
| **Google Fonts** | Tipografia (Syne + DM Sans) |
| **Intl.Segmenter** | Parsing correto de emojis multi-codepoint |

Sem frameworks. Sem bundlers. Sem `node_modules`. JavaScript vanilla puro.

---

## 🔧 Personalização

### Adicionar categorias à Emojipedia
No objeto `EP_CATS` dentro do script:
```js
const EP_CATS = {
  '🦄 A minha categoria': ['🦄', '✨', '🌈'],
  // categorias existentes...
};
```

### Adicionar um modo de renderização
1. Adiciona uma entrada ao array `MODES`:
```js
{ id:'meuModo', name:'Meu Modo', bg:'#001122' }
```
2. Adiciona o case no `switch(S.mode)` dentro de `doRender()`
3. Opcionalmente cria uma secção de opções `#opts-meuModo` com a classe `mopt-sec`

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

- **Exportação vs Preview:** o preview redimensiona o canvas para caber no ecrã. O botão "Guardar" cria um canvas separado na resolução real (ex: 1080×1920) antes de exportar o PNG.
- **Modo Glifo:** usa `filter: grayscale(1) contrast(999) brightness(0.5)` num canvas offscreen para forçar o browser a achatar o emoji, seguido de threshold pixel-a-pixel com `ImageData` para colorização.
- **Seed RNG:** gerador linear congruente com seed — o botão 🔀 gera nova seed sem alterar definições, reproduzindo sempre o mesmo resultado para a mesma seed.
- **PWA sem Service Worker:** o SW requer um ficheiro `.js` servido via HTTPS com o scope correto — não é possível registar via blob URL. A app funciona offline após carga inicial do browser mas sem cache formal de SW.
- **Silhueta:** usa `globalCompositeOperation: 'source-in'` num canvas offscreen para colorir a forma do emoji preservando a transparência alpha.

---

## 📄 Licença

MIT — usa, modifica e distribui livremente.

---

*Feito com 🎨 e zero dependências.*
