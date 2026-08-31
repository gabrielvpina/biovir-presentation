# Modelo de apresentações — Biovir Lab

Tema **Quarto + reveal.js** para seminários, qualificações e defesas do
Laboratório de Bioinformática e Virologia (UESC).

Você escreve um arquivo de texto (`.qmd`); o Quarto devolve uma apresentação
em HTML com a identidade visual do laboratório — fundo branco, acentos
`#304ea1` / `#326db5`, fontes locais — em **um único arquivo**, que abre com
duplo clique em qualquer computador, sem internet e sem pasta ao lado.

---

## Índice

1. [O que entra e o que sai](#o-que-entra-e-o-que-sai)
2. [Instalar o Quarto](#1-instalar-o-quarto)
3. [Instalar o tema](#2-instalar-o-tema)
4. [Criar sua apresentação](#3-criar-sua-apresentação)
5. [Sintaxe básica](#4-sintaxe-básica)
6. [Componentes do tema](#5-componentes-do-tema)
7. [Referência rápida de classes](#6-referência-rápida-de-classes)
8. [Slide da equipe](#7-slide-da-equipe)
9. [Bibliografia](#8-bibliografia)
10. [Exportar e compartilhar](#9-exportar-e-compartilhar)
11. [Estrutura do projeto](#10-estrutura-do-projeto)
12. [Problemas comuns](#11-problemas-comuns)

---

## O que entra e o que sai

### Entrada — `minha-apresentacao.qmd`

Texto puro: um cabeçalho **YAML** entre `---` e, depois dele, Markdown.
Nada de tema, cores ou logo — isso tudo já vem de `_quarto.yml`.

```markdown
---
title: "Diversidade viral em solos de Caatinga"
subtitle: "Resultados preliminares do sequenciamento"
author:
  - name: "Maria Silva"
    email: "maria@uesc.br"
institute: "Universidade Estadual de Santa Cruz · Ilhéus, Bahia"
date: today
---

## Roteiro

1. Contexto
2. Métodos
3. Resultados

# Resultados {.divider background-color="#304ea1"}

## Cobertura por amostra

::: {.columns}
::: {.column width="60%"}
![](assets/cobertura.png){.framed width="100%"}
:::

::: {.column width="40%"}
::: {.highlight}
87% das amostras passaram do limiar de 30×.
:::
:::
:::
```

### Comando

```bash
quarto render minha-apresentacao.qmd
```

### Saída — `minha-apresentacao.html`

Uma apresentação **reveal.js** navegável pelo teclado, com:

| O que você escreveu | O que aparece |
|:--|:--|
| bloco YAML | slide de capa, com logo, autores e data formatada em português |
| `## Roteiro` | um slide de conteúdo, título azul com filete de acento |
| `# Resultados {.divider ...}` | slide de divisão de seção, faixa azul de largura total |
| `::: {.columns}` | duas colunas lado a lado |
| `{.framed}` | imagem com moldura arredondada e sombra |
| `::: {.highlight}` | caixa azul-clara com barra lateral |

Mais: numeração `3/24` no canto, barra de progresso, cápsula viral no rodapé,
menu de navegação (`M`), visão geral dos slides (`O`) e tela do apresentador
com cronômetro e notas (`S`).

O arquivo é **self-contained**: fontes, logos, fotos, CSS e JavaScript ficam
todos dentro do `.html`. Custo: o arquivo final costuma ter de 5 a 20 MB e a
renderização demora alguns segundos a mais.

> Quer ver antes de instalar? Renderize `tutorial.qmd` — o guia de uso é ele
> próprio uma apresentação feita com este tema.

---

## 1. Instalar o Quarto

O Quarto é o programa que converte `.qmd` em slides. Instala-se **uma vez por
computador**. Em qualquer sistema, o caminho mais confiável é o instalador
oficial: <https://quarto.org/docs/get-started/>

### Windows

Baixe o `.msi` e execute (duplo clique, *Next* até o fim).

Pelo terminal (PowerShell):

```powershell
winget install --id Posit.Quarto
```

Sem senha de administrador? O instalador oferece a opção **"somente para este
usuário"**, que funciona sem privilégios.

### macOS

Baixe o `.pkg` e execute. Com [Homebrew](https://brew.sh):

```bash
brew install --cask quarto
```

### Linux

**Debian / Ubuntu** — baixe o `.deb` da página oficial e:

```bash
sudo dpkg -i quarto-*-amd64.deb
```

**Fedora / RHEL:**

```bash
sudo rpm -i quarto-*-linux-amd64.rpm
```

**Qualquer distro, sem `sudo`** — use o `.tar.gz`:

```bash
mkdir -p ~/opt && tar -xzf quarto-*-linux-amd64.tar.gz -C ~/opt
mkdir -p ~/.local/bin
ln -s ~/opt/quarto-*/bin/quarto ~/.local/bin/quarto
```

Depois garanta que `~/.local/bin` está no `PATH` (acrescente
`export PATH="$HOME/.local/bin:$PATH"` ao `~/.bashrc` ou `~/.zshrc`).

### Conferir se deu certo

Abra o terminal — **Prompt de Comando** ou **PowerShell** no Windows,
**Terminal** no macOS e no Linux — e rode:

```bash
quarto --version
```

Se aparecer um número de versão (ex.: `1.9.38`), está instalado. Para um
diagnóstico completo:

```bash
quarto check
```

### Editor (recomendado, não obrigatório)

| Editor | Como |
|:--|:--|
| **VS Code** | instale a extensão *Quarto* (aba de extensões, busque por "Quarto") |
| **Positron** | já vem com suporte a Quarto embutido; bom para quem usa R e Python |
| **RStudio** | funciona sem instalar nada a mais |

Dá para escrever no Bloco de Notas e rodar `quarto render` no terminal — a
extensão só acrescenta preview ao vivo e realce de sintaxe.

### R ou Python (opcional)

Só é necessário se a apresentação for **executar código**. Os `.qmd` deste
modelo são texto puro e renderizam sem nenhum dos dois. Para executar chunks
(` ```{r} ` ou ` ```{python} `), instale o pacote `knitr` (R) ou `jupyter`
(Python) e confirme com `quarto check`.

---

## 2. Instalar o tema

O tema é este repositório inteiro: `_quarto.yml` configura a **pasta toda**,
então suas apresentações vivem dentro dela.

### Opção A — clonar (recomendado)

```bash
git clone https://github.com/gabrielvpina/biovir-presentation.git
cd biovir-presentation
```

Para atualizar depois, quando o tema mudar:

```bash
git pull
```

### Opção B — "Use this template" no GitHub

No topo da página do repositório, botão verde **Use this template** →
*Create a new repository*. Você fica com uma cópia própria, versionada, que
pode receber atualizações do original como um `remote` extra.

### Opção C — baixar o ZIP

**Code → Download ZIP**, descompacte e trabalhe dentro da pasta. Simples, mas
sem atualizações automáticas.

### Conferir a instalação do tema

```bash
quarto render tutorial.qmd
```

Abra `tutorial.html` no navegador. Se os slides aparecerem com fundo branco,
títulos azuis e a cápsula viral no canto inferior direito, está tudo certo.

---

## 3. Criar sua apresentação

```bash
# 1. entre na pasta do tema
cd biovir-presentation

# 2. copie o modelo
cp modelo-apresentacao.qmd seminario-2026-03.qmd

# 3. abra o preview (recarrega sozinho a cada save)
quarto preview seminario-2026-03.qmd
```

Escreva, salve, veja no navegador. Quando terminar:

```bash
quarto render seminario-2026-03.qmd
```

> **Onde colocar o arquivo:** o `.qmd` precisa ficar na raiz do projeto, ao
> lado de `_quarto.yml`. É de lá que saem o tema, os caminhos para `assets/`
> e a configuração da equipe.

**Regras do arquivo:** salve em UTF-8, sem espaços nem acentos no nome
(`seminario-2026-03.qmd`, não `Seminário 03.qmd`).

---

## 4. Sintaxe básica

### Como nascem os slides

| Você escreve | O que acontece |
|:--|:--|
| `## Título` | novo slide de conteúdo |
| `# Título {.divider background-color="#304ea1"}` | slide de divisão de seção |
| `# Título {.divider-light}` | divisão de seção em fundo branco |
| `---` (três traços) | novo slide sem título |
| `### Subtítulo` | subtítulo *dentro* do slide |

**Regra prática:** um `##` = um slide. Se o conteúdo transbordar, divida em
dois slides em vez de diminuir a fonte.

### Markdown essencial

```markdown
**negrito**  *itálico*  `código inline`  [link](https://biovir.br)

- item
- outro item
  - sub-item

1. primeiro
2. segundo

> citação em bloco
```

### Duas colunas

```markdown
::: {.columns}
::: {.column width="60%"}
Conteúdo da esquerda
:::

::: {.column width="40%"}
Conteúdo da direita
:::
:::
```

Os `:::` são **divs**. Sempre em pares: um para abrir, um para fechar.
Indentar não importa; contar sim.

### Imagens

```markdown
![](assets/minha-figura.png){width="70%"}
![](assets/foto.jpg){.framed width="60%"}     ← com moldura
![](assets/retrato.jpg){.round width="40%"}   ← recorte circular
```

O caminho é relativo ao `.qmd`: `assets/figura.png`, nunca
`/Users/.../figura.png`.

### Tabelas

```markdown
| Grupo    | n   | Cobertura |
|----------|----:|----------:|
| Controle | 120 |     94,2% |
| Teste A  |  98 |     88,7% |
```

Os dois-pontos na linha de traços definem o alinhamento: `|---:|` à direita,
`|:---|` à esquerda, `|:---:|` centralizado.

### Revelar por partes

```markdown
::: {.incremental}
- aparece primeiro
- depois este
:::
```

Ou uma pausa solta, com três pontos separados por espaços:

```markdown
Primeira parte.

. . .

Segunda parte.
```

### Blocos de código

Três crases e o nome da linguagem:

````markdown
```bash
minimap2 -ax map-ont referencia.fa leituras.fq | samtools sort -o out.bam
```
````

Para destacar linhas específicas:

````markdown
```{.python code-line-numbers="2-3"}
import pandas as pd
df = pd.read_csv("amostras.csv")
df = df.query("cobertura > 0.8")
```
````

Para **executar** o código e inserir o resultado no slide, troque `python`
por `{python}` (ou `{r}`):

````markdown
```{python}
#| label: fig-cobertura
#| fig-cap: "Cobertura por amostra"
#| echo: false

import matplotlib.pyplot as plt
plt.plot(dados.posicao, dados.cobertura)
plt.show()
```
````

### Notas do apresentador

```markdown
::: {.notes}
Lembrar de citar a colaboração com o LIKA.
:::
```

Só você vê, na tela do apresentador (tecla `S`).

### Slide cheio demais

```markdown
## Resultados detalhados {.smaller}     ← reduz a fonte do slide inteiro
## Tabela longa {.scrollable}           ← dá barra de rolagem
```

Use com moderação: slide cheio costuma ser sinal de que faltam slides, não de
que falta espaço.

---

## 5. Componentes do tema

Além do Markdown padrão, o tema traz blocos prontos com a identidade do
laboratório. Todos se aplicam com classes.

### Caixas

```markdown
::: {.highlight}
Ideia central do slide, conclusão parcial.
:::

::: {.box}
Bloco neutro, com contorno — detalhe metodológico, observação.
:::

::: {.highlight-strong}
Afirmação de máximo peso, em fundo azul.
:::
```

### Cartões em grade

```markdown
::: {.cards}
::: {.card}
#### 1. Coleta
Origem das amostras, período e critérios.
:::

::: {.card}
#### 2. Sequenciamento
Plataforma, química e profundidade alvo.
:::
:::
```

A grade se reorganiza sozinha conforme o número de cartões.

### Números em destaque

```markdown
::: {.number}
### 1.245
amostras analisadas
:::
```

### Cores alternativas

Some uma classe de cor a qualquer `.highlight`, `.box`, `.card` ou
`.highlight-strong`:

```markdown
::: {.card .red}
#### Limitação
Amostragem restrita a uma estação do ano.
:::
```

Disponíveis: `.red` `.amber` `.green` `.slate`. Para uma cor fora da paleta,
defina a variável CSS direto no `.qmd`:

```markdown
::: {.card style="--color: #7b2d8e"}
```

### Marcações dentro da linha

```markdown
[Em revisão]{.label}      ← etiqueta em pílula
[termo-chave]{.mark}      ← grifo azul
[texto]{.small}           ← 0,78em
[texto]{.tiny}            ← 0,62em
```

Colchetes com chaves — `[conteúdo]{.classe}` — aplicam estilo a um trecho
**dentro** da linha; os `:::` aplicam a um bloco inteiro.

### Legenda

```markdown
::: {.caption}
Figura 1. Distribuição das amostras por município. n = 1.245.
:::
```

---

## 6. Referência rápida de classes

**Do tema** (definidas em `styles/componentes.scss`):

| Classe | Onde se usa | O que faz |
|:--|:--|:--|
| `.divider` | `# Título` | divisão de seção, faixa azul |
| `.divider-light` | `# Título` | divisão de seção, fundo branco |
| `.highlight` | `:::` | caixa de ideia central |
| `.box` | `:::` | bloco neutro com contorno |
| `.highlight-strong` | `:::` | afirmação de máximo peso |
| `.cards` / `.card` | `:::` | grade de cartões |
| `.number` | `:::` | número grande + legenda |
| `.red` `.amber` `.green` `.slate` | `:::` | cor alternativa das caixas |
| `.label` | `[texto]{...}` | etiqueta em pílula |
| `.mark` | `[texto]{...}` | grifo azul |
| `.small` `.tiny` | `[texto]{...}` ou `:::` | texto menor |
| `.caption` | `:::` | legenda discreta |
| `.text-center` `.text-right` | `:::` | alinhamento |
| `.gray` `.blue` `.blue-light` | `[texto]{...}` | cor do texto |
| `.spaced` | `:::` | respiro acima do bloco |
| `.no-bullets` | `:::` | lista sem marcador |
| `.framed` | `![](f.png){...}` | moldura na imagem |
| `.round` | `![](f.jpg){...}` | recorte circular |
| `.team-full` | `## Título` | slide da equipe: mosaico + nomes |
| `.names` | `:::` | lista de nomes em 4 colunas |
| `.team` | `## Título` | fecho curto, foto ao fundo |

**Do Quarto / reveal.js** (funcionam aqui como em qualquer documento Quarto):

`.columns` `.column` `.incremental` `.fragment` `.notes` `.smaller`
`.scrollable`

> As classes do tema estão **em inglês**, como as do Quarto — assim não é
> preciso lembrar qual é qual. A documentação e os comentários do código
> seguem em português.

---

## 7. Slide da equipe

O slide final combina duas seções **independentes**:

```markdown
## A equipe {.team-full}

{{< equipe >}}          ← fotos, montadas automaticamente

::: {.names}            ← lista de nomes, editada à mão
- **Coordenação em negrito**
- Nome de cada pessoa
:::
```

As fotos vêm dos subdiretórios de `assets/members/`, e a hierarquia é a
própria estrutura de pastas:

```
assets/members/
├── professor/      → Coordenação
├── researcher/     → Pesquisadores
├── phd/            → Doutorado
├── master/         → Mestrado
└── undergraduate/  → Graduação   (vazia: não aparece)
```

**Manutenção:** entrou alguém, largue a foto na subpasta certa; saiu, apague o
arquivo. Não existe lista de fotos para atualizar. Os títulos e a ordem dos
grupos ficam em `_quarto.yml`, na chave `equipe:`; subpasta que exista no disco
mas não esteja lá aparece no fim, com o nome da própria pasta.

Formatos aceitos: `.png` `.jpg` `.jpeg` `.webp` `.gif` `.svg`, em qualquer
tamanho — são cortadas em círculo automaticamente. Retratos aproximadamente
quadrados ficam melhores.

Variações do shortcode:

| Você escreve | O que muda |
|:--|:--|
| `{{< equipe >}}` | mosaico completo, com o rótulo de cada grupo |
| `{{< equipe titulos=false >}}` | só as fotos, sem rótulo |
| `{{< equipe pasta=assets/outra >}}` | lê fotos de outra pasta |

---

## 8. Bibliografia

```bash
mv refs-exemplo.bib refs.bib
```

Só isso. A partir daí, todas as apresentações da pasta passam a ter citações
no padrão **ABNT** e um slide **Bibliografia** no fim, montado só com o que foi
citado. Sem `refs.bib`, nada aparece — nem slide, nem erro.

| No `.qmd` | Resultado |
|:--|:--|
| `[@silva2024]` | (Silva, 2024) |
| `@silva2024` | Silva (2024) |
| `[@silva2024, p. 42]` | (Silva, 2024, p. 42) |
| `[@a2024; @b2023]` | (A, 2024; B, 2023) |

O `.bib` sai pronto do Zotero, do Mendeley ou do Google Acadêmico
(botão "Citar" → BibTeX).

Opcional, no YAML do seu documento:

```yaml
bibliography: minhas-referencias.bib   # outro arquivo
titulo-bibliografia: "Referências"     # outro título de slide
csl: config/outro-estilo.csl           # outro estilo de citação
bibliografia-automatica: false         # não criar o slide
```

Para escolher **onde** o slide entra, escreva você mesmo `## Bibliografia`
seguido de `::: {#refs}` `:::` — o filtro detecta e não duplica.

---

## 9. Exportar e compartilhar

### Arquivo único

Já é o padrão (`embed-resources: true` em `_quarto.yml`). O `.html` gerado
carrega fontes, logos, fotos, CSS e JavaScript dentro de si: dá para mandar
por e-mail, copiar para um pen drive ou abrir no computador do auditório —
sem levar pasta nenhuma junto e sem depender de internet.

### PDF

1. Abra o `.html` no navegador
2. Acrescente `?print-pdf` no fim do endereço
3. `Ctrl/Cmd + P` → **Salvar como PDF**
4. Margens: **Nenhuma** · marque **Gráficos de segundo plano**

```
file:///.../seminario-2026-03.html?print-pdf
```

Animações e revelação incremental viram slides estáticos no PDF.

### Atalhos durante a apresentação

| Tecla | O que faz |
|:--|:--|
| `→` `←` | avança / volta |
| `F` | tela cheia |
| `S` | tela do apresentador (notas + cronômetro + próximo slide) |
| `O` | visão geral dos slides |
| `B` ou `.` | tela preta (pausa) |
| `M` | menu de navegação |
| `?` | lista todos os atalhos |

### Reduzir o peso do arquivo

Com as fotos originais da equipe (450 px, ~250 kB cada), o `.html` chega a
~14 MB. Como elas aparecem com ~72 px de lado, dá para reduzir muito sem perda
visível:

```bash
bash tools/otimizar-fotos.sh   # originais ficam em assets/members-originais/
quarto render
```

### Quadro-branco

O plugin `chalkboard` do reveal.js (teclas `B`/`C`, desenhar sobre o slide)
**não é compatível** com `embed-resources: true`. Se preferir o quadro ao
arquivo único, troque `embed-resources` para `false` e descomente o bloco
`chalkboard:` em `_quarto.yml`.

---

## 10. Estrutura do projeto

| Caminho | Para que serve |
|:--|:--|
| `_quarto.yml` | configuração de **todas** as apresentações: tema, logo, navegação, grupos da equipe |
| `modelo-apresentacao.qmd` | ponto de partida — copie este arquivo |
| `tutorial.qmd` | guia de uso em formato de slides |
| `styles/variaveis.scss` | **cores e fontes** — comece por aqui para mudar o visual |
| `styles/fonts.scss` | declaração `@font-face` das fontes locais |
| `styles/biovir.scss` | tema base: tipografia, listas, tabelas, código, rodapé |
| `styles/componentes.scss` | capa, divisores, caixas, cartões, números, slide da equipe |
| `config/titulos-em-blocos.lua` | impede que títulos dentro de blocos virem slides |
| `config/equipe.lua` | shortcode `{{< equipe >}}`: monta o mosaico de fotos |
| `config/bibliografia.lua` | liga citações e cria o slide "Bibliografia" quando existe `refs.bib` |
| `config/abnt.csl` | estilo de citação ABNT (padrão) |
| `refs-exemplo.bib` | renomeie para `refs.bib` para ativar as referências |
| `tools/otimizar-fotos.sh` | reduz o peso das fotos da equipe |
| `fonts/` | `.woff2` + licenças OFL |
| `assets/` | logos, foto coletiva e `members/` (fotos individuais) |

Para fazer slides você mexe **só no seu `.qmd`**. O resto já está pronto.

### Tipografia

| Papel | Fonte | Licença |
|:--|:--|:--|
| Títulos | Space Grotesk | SIL OFL |
| Texto | Inter | SIL OFL |
| Código | JetBrains Mono | SIL OFL |

Fontes variáveis, servidas localmente: um `.woff2` por estilo cobre toda a
faixa de pesos (~590 kB no total). Licenças em `fonts/OFL-*.txt`.

### Mudar cores ou fontes

Abra `styles/variaveis.scss` — é o único arquivo que precisa ser editado para
mudar a aparência:

```scss
$biovir-azul:       #304ea1;   // primária
$biovir-azul-claro: #326db5;   // secundária

$fonte-titulo: "Space Grotesk", sans-serif;
$fonte-texto:  "Inter", sans-serif;
$fonte-codigo: "JetBrains Mono", monospace;
```

Para trocar de fonte: coloque o `.woff2` em `fonts/`, declare o `@font-face`
em `styles/fonts.scss` e mude o nome acima.

### Mudar algo só na sua apresentação

Qualquer opção de `_quarto.yml` pode ser sobrescrita no YAML do seu `.qmd`:

```yaml
---
title: "Aula de bioinformática"
format:
  revealjs:
    footer: "BIO-042 · Turma 2026.1"
    slide-number: false
    transition: slide
---
```

O que você **não** escrever continua vindo de `_quarto.yml` — inclusive tema,
fontes e logos. Nunca é preciso repetir o bloco `theme:`.

### O que o repositório não versiona

O `.gitignore` deixa de fora tudo que o `quarto render` sabe reconstruir e
tudo que é pessoal: os `.html` gerados, `_freeze/`, `.quarto/`, PDFs, o
`refs.bib` de cada um e os `.qmd` de apresentações individuais (só
`modelo-apresentacao.qmd` e `tutorial.qmd` são versionados). Para versionar o
seu seminário, comente a regra `/*.qmd` ou use um repositório próprio.

---

## 11. Problemas comuns

### `quarto: command not found`

O terminal ainda não enxerga o Quarto. Feche e abra o terminal de novo; se
persistir, reinstale marcando a opção de adicionar ao `PATH`.

### "A apresentação pula sozinha de volta para o começo"

Quase sempre é um `:::` que **começa** com um título (`##`, `###`, `####`).
O pandoc transforma esse bloco em um *slide*, o reveal.js perde a conta dos
índices e volta ao início.

Nos componentes do tema — `.card`, `.box`, `.highlight`, `.highlight-strong`,
`.number` — isso já está resolvido pelo filtro `config/titulos-em-blocos.lua`.
Em um div **seu**, use `**Título**` em vez de `### Título`.

### Slide em branco no começo

Tudo o que vier entre o `---` do YAML e o primeiro `##` vira um slide sem
título — inclusive comentários `<!-- ... -->`. Coloque anotações **dentro** do
bloco YAML, onde `#` é comentário.

### `:::` desemparelhado

Cada `:::` que abre precisa de um que feche. Um a menos e o resto do slide
some.

### A imagem não aparece

O caminho é relativo ao `.qmd`: `assets/figura.png`, não
`/Users/.../figura.png`.

### A renderização engasga

Apague a pasta `_files/` e a pasta oculta `.quarto/`, depois rode
`quarto render` de novo.

### Armadilhas já resolvidas no CSS (não reintroduza)

1. **`padding` em `.column`** — o Quarto renderiza colunas como `inline-block`
   e o reveal.js não define `box-sizing: border-box`. Em content-box,
   `50% + padding` passa de 100% e a segunda coluna quebra para a linha de
   baixo. `componentes.scss` força `border-box` em `.column`; não remova.
2. **`font-size` em elemento e filho** — regras que casam tanto no `<p>` quanto
   no `<span>` dentro dele multiplicam o tamanho (2,2em × 2,2em ≈ 160px) e os
   textos se sobrepõem. Ver o comentário em `.number`.
3. **Altura dos slides** — o slide útil tem 924 × 616 px e o reveal.js corta o
   excesso sem avisar. O `.team-full` tem o orçamento de altura anotado no
   SCSS; refaça a conta se mexer nos tamanhos.
4. **`navigation-mode: linear`** — está em `_quarto.yml` para que `←`/`→`
   percorram todos os slides na ordem do arquivo. Sem isso, um `#` cria uma
   pilha vertical e a seta direita pula a seção inteira.

### Onde buscar ajuda

- Documentação do reveal.js no Quarto:
  <https://quarto.org/docs/presentations/revealjs/>
- Markdown básico:
  <https://quarto.org/docs/authoring/markdown-basics.html>
- No laboratório: abra `modelo-apresentacao.qmd` — quase toda a sintaxe deste
  guia está lá, pronta para copiar.

---

## Licença

Código e estilos sob [licença MIT](LICENSE). As fontes em `fonts/` são
distribuídas sob a SIL Open Font License (ver `fonts/OFL-*.txt`). Logos e
fotos da equipe são de uso interno do Biovir Lab.
