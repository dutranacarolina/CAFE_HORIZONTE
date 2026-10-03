# Exercício avançado — site responsivo em 5 páginas

Validação do que você viu nas aulas 1 e 2: viewport, CSS separado, tokens, unidades e botão de dedo. Você **constrói do zero** um site de cinco páginas com um design system simples compartilhado.

Tema: **Café Horizonte** — cafeteria de bairro que quer um site que funcione no celular sem pinça e sem botão impossível de tocar.

Preset de teste: **iPhone SE (375)** e, no fim, uma janela larga (tipo 1280).

---

## O que entregar

Uma pasta `cafe-horizonte/` com esta árvore:

```text
cafe-horizonte/
  index.html
  cardapio.html
  horarios.html
  sobre.html
  contato.html
  css/
    tokens.css
    estilos.css
```

Zipa a pasta e anexa na atividade.

---

## O que conta (e o que ainda não)

**Usa:** `meta viewport`, `link` para CSS, custom properties no `:root`, `rem` / `%` / `vw` / `px`, `clamp`, alvo de toque ≥ 44 px, navegação entre as cinco páginas, **um** design system nos dois arquivos CSS.

**Ainda não usa** (fica para aulas seguintes): `flex`, `grid`, `media query`, framework (Tailwind, Bootstrap), JavaScript.

“Responsivo” neste exercício significa: o layout **flui** com unidades relativas e `clamp`. Em 375 não pinça; em 1280 o título não vira outdoor. Sem breakpoint.

---

## Situação

O dono do Café Horizonte mandou um brief curto:

> Quero um site com cinco páginas: início, cardápio, horários, sobre e contato. No celular o cliente tem que achar o telefone sem errar o toque. As cores e os espaços não podem estar espalhados no código — se eu pedir “mais escuro”, você muda num lugar só.

Você é a pessoa que entrega isso.

---

## As cinco páginas (conteúdo mínimo)

Cada HTML tem `<!DOCTYPE html>`, `lang="pt-BR"`, `meta charset`, **viewport**, `title` próprio e os dois `link` de CSS (nessa ordem: tokens, depois estilos).

| Arquivo | Título da aba | Conteúdo mínimo |
|---------|---------------|-----------------|
| `index.html` | Café Horizonte — Início | Nome do café, frase de boas-vindas, CTA `tel:` com classe `.cta`, nav para as outras páginas |
| `cardapio.html` | Café Horizonte — Cardápio | Pelo menos 3 itens (nome + preço em texto). Sem tabela obrigatória — `h2` + `p` bastam |
| `horarios.html` | Café Horizonte — Horários | Dias e horários em parágrafos ou lista. Endereço fictício de bairro |
| `sobre.html` | Café Horizonte — Sobre | 1–2 parágrafos sobre a história do café |
| `contato.html` | Café Horizonte — Contato | Telefone em `.cta` + `tel:`, e-mail em `mailto:` (pode ser texto + link), endereço |

Todas as páginas compartilham a **mesma navegação** (links relativos entre os cinco arquivos) e o **mesmo rodapé** com o telefone em `.cta`.

Telefone de exemplo (pode trocar o número, mantém o formato `tel:`):

```html
<a class="cta" href="tel:+555133334444">Ligar: (51) 3333-4444</a>
```

---

## Passo a passo

Faça na ordem. Não pule o design system: sem ele, as cinco páginas viram cinco estilos diferentes.

### Passo 1 — Pasta e esqueleto

1. Cria a pasta `cafe-horizonte/` e dentro dela a pasta `css/`.
2. Cria os cinco HTML vazios (só o esqueleto: doctype, `html`, `head`, `body`).
3. Em **cada** `head`, coloca:

```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>…</title>
<link rel="stylesheet" href="css/tokens.css">
<link rel="stylesheet" href="css/estilos.css">
```

O `title` muda por página. Os dois `link` são iguais nas cinco.

**Teste:** abre `index.html` em 375. Ainda sem CSS — a página não deve rolar pro lado (viewport ok).

---

### Passo 2 — Design system em `tokens.css`

Este arquivo **só** declara decisões. Nada de `body`, `h1` ou `.cta` aqui.

Sugestão de tokens (pode ajustar valores; **não** pode deixar de ter cor, espaço, tipo e toque):

```css
:root {
  /* Cor */
  --cor-fundo: #f7f1e8;
  --cor-superficie: #fff8f0;
  --cor-texto: #2c2118;
  --cor-texto-suave: #5c4a3a;
  --cor-marca: #6b3e26;
  --cor-cta: #c45c26;
  --cor-cta-texto: #ffffff;
  --cor-borda: #e0d2c0;

  /* Tipo */
  --fonte-base: "Segoe UI", system-ui, sans-serif;
  --tamanho-corpo: 1rem;
  --tamanho-titulo: clamp(1.5rem, 5vw, 2.25rem);
  --tamanho-subtitulo: clamp(1.15rem, 3vw, 1.5rem);
  --altura-linha: 1.5;

  /* Espaço */
  --espaco-xs: 0.5rem;
  --espaco-sm: 0.75rem;
  --espaco-md: 1rem;
  --espaco-lg: 1.5rem;
  --espaco-xl: 2rem;
  --largura-conteudo: 40rem;
  --padding-pagina: 5%;

  /* Contorno e toque */
  --raio: 0.5rem;
  --toque: 44px;
}
```

Por que assim:

- **Cor** — decide a cara do café num lugar só.
- **Tipo** — o título já nasce com `clamp` no token; o CSS de componente só aplica `var(--tamanho-titulo)`.
- **Espaço** — `rem` e `%` (padding da página). `40rem` limita a largura do texto no desktop sem media query.
- **Toque** — `44px` absoluto de propósito: o dedo não escala com a fonte.

**Teste:** o arquivo carrega sem erro no Network. Ainda não muda a tela — falta o `estilos.css`.

---

### Passo 3 — Componentes em `estilos.css`

Aqui você **usa** os tokens. Cor hardcoded (`#c45c26` direto no seletor) = fora do critério.

Monte pelo menos estes blocos:

1. **`body`** — `font-family`, `font-size`, `line-height`, `background`, `color`, `margin: 0`. Tudo via `var(...)`.
2. **`.pagina`** (ou `main`) — `max-width: var(--largura-conteudo)`, `padding` com `var(--padding-pagina)` / `var(--espaco-lg)`, `margin-left` e `margin-right` em `auto` (centraliza no desktop sem flex).
3. **`.marca` ou `header`** — nome do café com `font-size: var(--tamanho-titulo)` (já traz o `clamp`).
4. **`.nav a`** — links da navegação. Padding generoso; em 375 o dedo precisa acertar. Pode ser lista de links um embaixo do outro (sem flex).
5. **`.cta`** — `display: inline-block`, `min-height: var(--toque)`, padding em `rem`, cores do token, `text-decoration: none`, `border-radius: var(--raio)`.
6. **`h2`** — `font-size: var(--tamanho-subtitulo)`.
7. **`.rodape`** — espaço com tokens + o `.cta` do telefone.

Exemplo do CTA (adapte ao seu arquivo):

```css
.cta {
  display: inline-block;
  min-height: var(--toque);
  padding: var(--espaco-sm) var(--espaco-md);
  background: var(--cor-cta);
  color: var(--cor-cta-texto);
  text-decoration: none;
  border-radius: var(--raio);
  font-size: var(--tamanho-corpo);
}
```

**Teste em 375:**

- fundo do `body` mudou
- Computed do `.cta` → `height` ≥ 44
- se você muda `--cor-cta` em `tokens.css`, o botão muda nas páginas sem editar o HTML

---

### Passo 4 — Montar o HTML da home (`index.html`)

Estrutura sugerida no `body`:

```text
header (nome do café)
nav (Início | Cardápio | Horários | Sobre | Contato)
main.pagina
  h1 ou p de boas-vindas
  parágrafo curto
  .cta de ligar
footer.rodape
  .cta de ligar de novo (ou só o telefone)
```

Links da nav (caminhos relativos, arquivos na mesma pasta):

```html
<nav class="nav" aria-label="Principal">
  <a href="index.html">Início</a>
  <a href="cardapio.html">Cardápio</a>
  <a href="horarios.html">Horários</a>
  <a href="sobre.html">Sobre</a>
  <a href="contato.html">Contato</a>
</nav>
```

**Teste:** clica em Cardápio — tem que abrir o arquivo (mesmo que ainda esteja vazio).

---

### Passo 5 — Repetir o casco nas outras quatro páginas

Copia header + nav + footer da home. Troca só o `title` e o `main`.

| Página | Ideia de conteúdo |
|--------|-------------------|
| Cardápio | Três produtos: ex. Café coado R$ 8, Cappuccino R$ 12, Pão na chapa R$ 10 |
| Horários | Seg–Sex 8h–19h, Sáb 9h–14h, Dom fechado + endereço |
| Sobre | Por que “Horizonte”, quando abriu, o que o bairro encontra lá |
| Contato | `.cta` com `tel:`, link `mailto:contato@cafehorizonte.exemplo`, endereço |

Mantém as **mesmas classes** (`.nav`, `.cta`, `.pagina`, `.rodape`). O design system é o que garante que as cinco páginas parecem o mesmo site.

---

### Passo 6 — Unidades na prática (checklist consciente)

Antes de entregar, abre o `estilos.css` e confere:

| Decisão | Unidade / token | Por quê |
|---------|-----------------|---------|
| Texto do corpo | `rem` (`--tamanho-corpo`) | acompanha a raiz |
| Título | `clamp` via `--tamanho-titulo` | chão no SE, teto no monitor |
| Padding da página | `%` (`--padding-pagina`) | acompanha a largura da janela |
| Espaço entre blocos | `rem` (`--espaco-*`) | escala com o texto |
| Largura do texto | `rem` (`--largura-conteudo`) | não estica frase em 1280 |
| Altura mínima do botão | `px` (`--toque`) | alvo de dedo fixo (44) |
| Cor | só `var(--cor-…)` | muda o tema num arquivo |

Se ainda tiver `font-size: 12vw` sem `clamp`, ou `height` do CTA abaixo de 44 no Computed, volta e corrige.

---

### Passo 7 — Bateria de testes (entrega)

Faz na ordem, em **375** e depois largo:

1. As cinco páginas abrem e a nav leva de uma à outra.
2. `tokens.css` e `estilos.css` carregam (Network / Elements).
3. Mudar `--cor-fundo` em `tokens.css` muda o fundo nas **cinco** páginas.
4. Mudar `--cor-cta` muda o botão em Início e Contato (e no rodapé).
5. `h1` / título da marca: em 375 não é formiga; em 1280 não é outdoor.
6. Computed do `.cta` → `height` ≥ 44.
7. O `tel:` discaria se fosse um aparelho (href correto).
8. Nenhum `flex`, `grid` ou `@media` no seu CSS.

---

## Critério mínimo para validar

- [ ] Cinco HTML + `css/tokens.css` + `css/estilos.css`
- [ ] Viewport em todas as páginas
- [ ] Design system: decisões de cor, tipo, espaço e toque no `:root` de `tokens.css`
- [ ] Componentes só com `var(...)` para essas decisões
- [ ] Título com `clamp` (no token ou no seletor — desde que exista)
- [ ] `.cta` ≥ 44 px de caixa em 375
- [ ] Site usável em 375 sem pinça e legível em janela larga
- [ ] Sem flex, grid, media query ou framework

---

## Dica se travar

1. CSS “não pega” → caminho do `link` (os HTML estão na raiz da pasta; o href é `css/...`).
2. Botão ainda fino → `inline-block` + `min-height: var(--toque)` + padding; mede a **caixa**, não a fonte.
3. Título explode no PC → falta o máximo do `clamp`.
4. Páginas com cara diferente → você copiou cor no HTML ou criou classe nova sem token.

Bom café — e bom Computed.
