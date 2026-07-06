# CLAUDE.md — Manual de MikroTik

Livro aberto em **Quarto** (HTML + PDF), da série *Manuais de Ciências*.
RouterOS do básico ao avançado, com foco em operação de provedor (fibra +
rádio rural). Publicado no GitHub Pages via GitHub Actions.

## §0 — Bootstrap da sessão (fazer ANTES de qualquer render/preview)

`quarto install tinytex` NÃO coloca os binários no PATH da sessão. Sem este
bootstrap, as figuras TikZ **falham em silêncio** e o `tikz.lua` erra com
`imgdata nil` (~linha 587) — a causa é PATH, **não** pacote faltando.

```bash
# Windows (Git Bash) — prepender o bin do TinyTeX ao PATH da sessão:
export PATH="$HOME/AppData/Roaming/TinyTeX/bin/windows:$PATH"
# Linux/macOS: export PATH="$HOME/.TinyTeX/bin/<plataforma>:$PATH"

# Garantir os pacotes das figuras (idempotente, rápido se já instalados):
tlmgr install standalone pgf pgfplots dvisvgm xcolor amsmath amsfonts
```

Depois disso, `quarto preview` e `quarto render` funcionam de primeira.

## Comandos

```bash
quarto preview            # site local com hot-reload (só HTML) — uso diário
quarto render             # gera site + PDF em _book/ (exige o bootstrap §0)
```

- Deploy é **automático**: push na `main` dispara `.github/workflows/publish.yml`.
- O workflow já contém os fixes obrigatórios: `chrome-headless-shell` (mermaid
  no PDF), localização do `tlmgr` via `find` + persistência no `GITHUB_PATH`,
  e `tlmgr install standalone pgf pgfplots dvisvgm xcolor amsmath amsfonts`.
- A branch `gh-pages` precisa existir ANTES do primeiro run (ver README.md).

## Estrutura

```
_quarto.yml    configuração do livro (lang: pt-BR na RAIZ, nunca em book:)
index.qmd      apresentação + grafo mermaid de pré-requisitos
ROADMAP.md     fila de trabalho autoritativa: capítulos, status, pré-requisitos
CLAUDE.md      este arquivo (COMO fazer)
volumes/       capítulos em volumes/vNN-tema/NN-nome.qmd
_extensions/danmackinlay/tikz/   extensão TikZ COM PATCHES LOCAIS
```

## Extensão TikZ (regras invioláveis)

- **NUNCA rodar `quarto add` ou `quarto update`** — sobrescreve os patches
  locais (paleta/estilos no preâmbulo, caminhos absolutos p/ Windows).
- No `_quarto.yml`: filtro `danmackinlay/tikz` vem **antes** de `quarto`;
  `tikz: svg-engine: dvisvgm`.
- Figuras TikZ exigem TeX no PATH **mesmo em render só-HTML** (cada figura
  compila pdflatex → dvisvgm). Ver §0.

## Convenções de escrita

- Idioma pt-BR, tom didático e direto; explicar cada conceito antes de usá-lo;
  analogias quando ajudarem. Pré-requisitos entre capítulos: ver ROADMAP.md.
- O capítulo `volumes/v01-primeiros-passos/01-o-que-e-mikrotik.qmd` é o
  **gabarito de estilo** — espelhe a anatomia dele em todo capítulo novo:
  objetivos de aprendizagem → corpo didático (com figuras/callouts) →
  **🧪 Laboratório** → resumo → bibliografia do capítulo.
- Referência cruzada sempre via `@sec-`, `@fig-`, `@tbl-` — nunca "Figura 2.1"
  na mão.
- Matemática (rara): MathJax padrão. **Não usar** `siunitx`, `physics` nem
  pacotes PDF-only.

### Figuras

- **TikZ** para diagramas esquemáticos (topologias, fluxos de pacote):
  div `::: {#fig-nome}` contendo bloco ```` ```{.tikz} ```` com `%%| filename:`
  e `%%| alt:`; legenda em texto antes do `:::` de fechamento; referenciar com
  `@fig-nome`.
- Estilos prontos no preâmbulo da extensão: `curva`, `destaque`, `auxiliar`,
  `eixo`, `ponto`, `vetor`; cores `manualblue/red/green/yellow/gray`.
- Imagens embutidas (png/jpg) **apenas** para screenshots (WinBox, WebFig).

### Laboratórios (obrigatórios em TODO capítulo)

Seção `## 🧪 Laboratório` com quatro partes, nesta ordem:

1. **Ambiente** — qual topologia CHR usar (ex.: "2 CHRs em rede interna");
   referenciar o capítulo de setup (`@sec-lab-chr`) em vez de reensinar.
2. **Comandos passo a passo** — blocos ```` ```routeros ```` prontos para
   copiar/colar, cada bloco precedido da explicação do que faz e seguido da
   saída esperada (`/ip address print` etc.).
3. **Verificação** — como confirmar que funcionou (`print`, `torch`, ping).
4. **Desafios** — 2 a 4 exercícios sem solução visível; soluções em
   `::: {.callout-tip collapse="true"}` com título `## Solução ...`.

Convenções de código RouterOS:

- Bloco de terminal: ```` ```routeros ```` (padrão único em todo o manual).
- Prompt `[admin@MikroTik] >` **só** ao ilustrar sessão interativa com saída;
  blocos para copiar contêm apenas os comandos.
- Comando destrutivo (reset, netinstall, remove) → `::: {.callout-warning}`
  imediatamente antes.

## Fluxo de trabalho

- **ROADMAP.md é a fonte única da verdade**: a próxima tarefa é sempre o
  primeiro item `[ ]` na ordem do arquivo. Fatia vertical: fechar o volume
  antes de abrir o próximo; fechar a fase antes da próxima.
- Ao escrever um capítulo: usar o caminho exato do ROADMAP; registrar em
  `chapters:` no `_quarto.yml` (descomentar o bloco `part:` ao abrir volume
  novo); validar com `quarto render`; marcar `[x]` no ROADMAP; commitar.
- Um capítulo por sessão; `/clear` entre capítulos; `/compact` a ~80% de
  contexto.

## Commits

- Formato: `cap NN: <título curto>` (ex.: `cap 03: licenças e RouterBOOT`);
  mudanças de infra: prefixo `scaffold:`/`fix:`/`docs:`.
- **Windows**: usar apenas `git commit -m "mensagem simples"`. NUNCA
  here-strings do PowerShell (`@'...'@`) dentro do Bash — o `@` vaza para a
  mensagem.
