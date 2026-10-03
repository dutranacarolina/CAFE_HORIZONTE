# Dicas de CSS — Café Horizonte

O enunciado das cinco páginas já está com você. Este arquivo ajuda a **planejar** o site antes de sair estilizando, e a usar o CSS das Aulas 1 e 2 para as páginas parecerem o mesmo café — legível no iPhone SE (375) e numa janela larga.

Continua valendo o combinado da atividade: viewport, `tokens.css` + `estilos.css`, `rem` / `%` / `clamp` e botão com caixa de pelo menos 44 px. Flex, grid e media query entram nas próximas aulas, na vitrine. Aqui o site melhora com decisão e com o CSS que você já pode escrever.

Preset de teste: **iPhone SE (375)**. No fim, uma janela larga (tipo 1280).

---

## Planejar numa folha, antes do `estilos.css`

O dono pediu cinco páginas que pareçam o mesmo lugar. Se a home ficar pronta no olho e o resto for “cópia do que der”, cardápio e contato nascem com outra cara.

Fecha estas quatro coisas no papel (ou num comentário no topo do `tokens.css`). Só depois abre o HTML.

**1. Casco.** O que se repete em toda página: nome do café, navegação, miolo, rodapé com telefone. Você escreve isso uma vez na home e copia. O que muda é o `title` e o conteúdo do `main`.

**2. Texto de verdade.** Três itens com preço, os horários, duas frases do sobre, telefone e e-mail. Inventar isso no meio do CSS atrasa e o espaçamento sai torto.

**3. Tokens.** Fundo, texto, marca, botão, tamanhos, espaços, 44 px. O `:root` sugerido no enunciado já é um café quente. Ajuste o tom se quiser; uma cor nova por página quebra o design system.

**4. Ordem no celular.** De cima para baixo, em 375: quem é o café, os cinco links (um embaixo do outro), o título da página, o texto, o botão de ligar, o rodapé.

| Página | `h1` | O que a pessoa acha rápido | Telefone |
|--------|------|----------------------------|----------|
| Início | Café Horizonte | nome, uma frase, botão de ligar | `.cta` no miolo e no rodapé |
| Cardápio | Cardápio | 3 itens com preço | no rodapé |
| Horários | Horários | dias, horas, endereço | no rodapé |
| Sobre | Sobre | 1–2 parágrafos | no rodapé |
| Contato | Contato | `tel:`, `mailto:`, endereço | `.cta` no miolo e no rodapé |

O CSS pinta esse plano. Ele não inventa o plano.

---

## Dois arquivos, um site

`tokens.css` guarda a decisão. `estilos.css` aplica com `var(...)`.

No `<head>` de **cada** página, nesta ordem: charset, viewport, `title` daquela página, depois os dois `link`:

```html
<link rel="stylesheet" href="css/tokens.css">
<link rel="stylesheet" href="css/estilos.css">
```

Se você muda `--cor-fundo` e só a home muda, ou o `link` faltou numa página, ou a cor foi escrita direto no seletor (`background: #f7f1e8`). A cor do café mora no token. O componente só aponta para ela.

---

## Hierarquia: o cliente lê em segundos

Na calçada a pessoa procura o nome do café e o telefone. O resto espera.

Um `h1` por página. Na home, o nome do café. Nas outras, o assunto (Cardápio, Horários, Sobre, Contato). O nome da marca no topo pode ser um parágrafo com a classe `.marca`, para a página não ter dois `h1`.

`h2` marca um bloco de verdade: um item do cardápio, o endereço, um pedaço da história. Título de seção usa `font-size: var(--tamanho-subtitulo)`. O `clamp` desse tamanho já está no token.

Preço, horário secundário e endereço usam `--cor-texto-suave`. O texto que a pessoa precisa ler usa `--cor-texto`.

O título da marca usa `font-size: var(--tamanho-titulo)`. Esse token já traz o `clamp` (chão no celular, teto no monitor). Um `font-size: 48px` no mesmo seletor apaga essa decisão.

---

## O navegador já coloca margem. Você devolve o ritmo.

`body`, `h1`, `p` e `ul` chegam com margem do Chrome. Por isso o cardápio respira de um jeito e os horários de outro, mesmo com tokens no `:root`.

Zera essa margem e separa os blocos com **um** token de espaço. `--espaco-md` entre parágrafos. `--espaco-xl` antes do rodapé. O mesmo vão no cardápio e nos horários.

```css
body {
  margin: 0;
  font-family: var(--fonte-base);
  font-size: var(--tamanho-corpo);
  line-height: var(--altura-linha);
  background: var(--cor-fundo);
  color: var(--cor-texto);
}

h1,
h2,
p {
  margin-top: 0;
  margin-bottom: var(--espaco-md);
}
```

`line-height: var(--altura-linha)` (1.5 no token sugerido) deixa o parágrafo respirar. Texto colado na linha de cima cansa no celular.

---

## A caixa acompanha a tela

Com `box-sizing: border-box`, o padding entra na largura. Sem isso, `width: 100%` mais padding fica maior que o celular e a página rola para o lado.

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}

.pagina {
  max-width: var(--largura-conteudo);
  margin-right: auto;
  margin-left: auto;
  padding: var(--espaco-lg) var(--padding-pagina);
  background: var(--cor-superficie);
}
```

`--largura-conteudo` (40rem no enunciado) segura a linha de texto no monitor. `--padding-pagina` em `%` acompanha a janela. `margin` automático centraliza o miolo. Largura fixa em pixel no header, no miolo ou no rodapé estoura os 375 px.

O fundo da página pode ser `--cor-fundo` e o miolo `--cor-superficie`. As duas cores já estão no token. O miolo fica uma faixa clara em cima do bege.

---

## Faixa da marca

O nome do café pode ocupar uma faixa com a cor da marca. O texto dessa faixa usa a cor clara do botão, senão o marrom do `body` soma em cima do marrom do header e some.

```css
header {
  padding: var(--espaco-lg) var(--padding-pagina);
  background: var(--cor-marca);
  color: var(--cor-cta-texto);
}

.marca {
  margin: 0;
  font-size: var(--tamanho-titulo);
  font-weight: 700;
}
```

---

## Navegação: um link por linha, alvo de dedo

Cinco links numa linha só, letra pequena, não cabem em 375 e o dedo erra o alvo. Cada link vira um bloco, com a mesma altura mínima do botão.

```css
a {
  color: var(--cor-marca);
}

.nav a {
  display: block;
  min-height: var(--toque);
  padding: var(--espaco-sm) var(--espaco-md);
  background: var(--cor-superficie);
  color: var(--cor-marca);
  text-decoration: none;
  border-bottom: 1px solid var(--cor-borda);
}
```

A regra `a` tira o azul padrão do navegador, que briga com a paleta do café. A classe `.cta` continua com a cor dela: classe ganha de elemento.

No Computed, a **caixa** do link (altura) chega a 44 ou mais. Medir só o `font-size` engana.

Para a pessoa saber em que página está, no HTML da página atual:

```html
<a href="cardapio.html" aria-current="page">Cardápio</a>
```

```css
.nav a[aria-current="page"] {
  background: var(--cor-marca);
  color: var(--cor-cta-texto);
}
```

`aria-current="page"` diz “esta é a página aberta”. O CSS só pinta. Nas outras páginas, esse atributo fica no link correspondente.

No monitor largo o menu continua empilhado. Isso é o combinado desta entrega. Enfileirar os links é assunto da aula de flex, na vitrine.

---

## Cardápio e horários em lista

Lista alinha melhor do que parágrafos soltos, cada um com uma margem diferente.

```html
<ul class="lista">
  <li>Café coado — R$ 8</li>
  <li>Cappuccino — R$ 12</li>
  <li>Pão na chapa — R$ 10</li>
</ul>
```

```css
.lista {
  list-style: none;
  margin: 0;
  padding: 0;
}

.lista li {
  padding-top: var(--espaco-sm);
  padding-bottom: var(--espaco-sm);
  border-bottom: 1px solid var(--cor-borda);
}
```

Nome e preço na mesma linha, com travessão. Horários no mesmo padrão: “Seg–sex — 8h–19h”, “Sábado — 9h–14h”, “Domingo — fechado”. O endereço pode ser um `p` depois da lista, com `--cor-texto-suave`.

---

## Botão de ligar

O `.cta` do enunciado continua. Duas propriedades ajudam o polegar no SE: o texto no centro da caixa e, no miolo da home e do contato, o botão ocupando a largura da coluna.

```css
.cta {
  display: inline-block;
  min-height: var(--toque);
  padding: var(--espaco-sm) var(--espaco-md);
  background: var(--cor-cta);
  color: var(--cor-cta-texto);
  text-decoration: none;
  border-radius: var(--raio);
  text-align: center;
  line-height: 1.4;
}

.pagina .cta {
  display: block;
}
```

`.pagina .cta` é o botão do miolo, em bloco. O do rodapé pode continuar `inline-block`. Os dois usam `min-height: var(--toque)`.

O contorno de foco fica. É o que mostra o botão para quem navega por teclado. `outline: none` apaga isso.

Texto claro em `--cor-cta`. Se escurecer demais o laranja, o branco continua tendo que se ler.

---

## Rodapé

O rodapé fecha as cinco páginas com o mesmo telefone. Uma borda de token separa ele do miolo, no lugar de uma margem inventada.

```css
.rodape {
  margin-top: var(--espaco-xl);
  padding: var(--espaco-lg) var(--padding-pagina);
  border-top: 1px solid var(--cor-borda);
  color: var(--cor-texto-suave);
}
```

---

## Foto, se você colocar alguma

A atividade não pede imagem. Se entrar uma, ela acompanha a coluna:

```css
img {
  display: block;
  max-width: 100%;
  height: auto;
}
```

Foto de milhares de pixels, sem `max-width`, cria a rolagem horizontal no celular. Uma imagem basta.

---

## Mapa rápido

| Situação no café | Onde resolver |
|------------------|---------------|
| “Mais escuro” / outra cor de botão | um token em `tokens.css` |
| Título formiga no SE ou outdoor no monitor | `var(--tamanho-titulo)`, que já é `clamp` |
| Blocos com vãos diferentes | margem do navegador zerada + `--espaco-*` |
| Página rola para o lado | `border-box`, `max-width`, imagem com `max-width: 100%` |
| Dedo erra o menu ou o telefone | `display: block` ou `inline-block`, `min-height: var(--toque)`, medir a caixa no Computed |
| Link azul padrão | `a { color: var(--cor-marca); }` e o `.cta` com a cor dele |
| Cardápio desalinhado | `ul.lista` com o mesmo padding e a mesma borda |

---

## Conferir antes de entregar

Em **375**, nas cinco páginas:

1. A nav leva de uma página à outra.
2. Os dois CSS carregam (Network ou Elements).
3. Mudar `--cor-fundo` muda o fundo nas cinco. Mudar `--cor-cta` muda o botão.
4. Nome e título dão para ler. No largo, o título não vira outdoor.
5. Computed do `.cta` e de um link da nav: altura ≥ 44.
6. O `href` do telefone é `tel:+55...` (só números depois do `+`).
7. Não aparece barra horizontal.

No largo: o miolo fica limitado pelo `--largura-conteudo` e o texto continua uma coluna legível.

---

## Se travar

1. CSS “não pega” — o HTML está na raiz de `cafe-horizonte/` e o `href` é `css/tokens.css` e `css/estilos.css`.
2. Espaço aleatório — ainda tem margem padrão em `h1` / `p` / `ul`, ou cada bloco usa um pixel diferente.
3. Botão fino — `inline-block` (ou `block`) + `min-height: var(--toque)` + padding. A medida é a caixa, no Computed.
4. Título explode no monitor — o `font-size` precisa ser o token com `clamp`, com máximo.
5. Páginas com cara diferente — cor ou tamanho escritos no HTML, ou classe nova sem token.

Bom café. E bom Computed.
