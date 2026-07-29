# Simulador de Metros Lineares de Trilhos · Patrimar Móveis

Ferramenta que **importa o PDF do lote, extrai as medidas das peças e calcula os metros
lineares de trilho/esteira** que o corte vai consumir — substituindo a planilha que hoje é
preenchida na mão.

Você exporta do sistema o relatório **“OFs Relacionadas ao Lote” (.pdf)**, arrasta o arquivo
na tela e o simulador monta tudo:

- **Leitura do PDF** — lê cada peça (código, descrição, OF) e **extrai as medidas**
  `COMP × LARG × ALT` embutidas na descrição (ex.: `RACK YAN TAMPO 900X400X15 …` → 900 × 400 × 15 mm).
  As quantidades das várias OFs de um mesmo código são **somadas**.
- **Cadastro de Tábuas (pranchas)** — os tamanhos padrão que você corta (já vem com
  1200×500, 1360×500, 1600×500, 1850×500). O simulador escolhe sozinho a tábua que **gasta menos
  esteira**: testa todas as cadastradas, nas duas orientações da peça, e fica com a de menor
  consumo (empate → a mais curta). Você pode trocar a qualquer momento — a escolha manual manda.
  A coluna **Mín. tábua** mostra o comprimento mínimo que a peça exige (peça + folga das 2 pontas).
- **Encaixe em pé ou virada** — o simulador testa as **duas orientações** da peça na tábua e fica com a
  que rende **mais peças por lastro** (coluna **Giro**: `Auto` · `Em pé` · `Virada ↻`, travável por peça).
  Ex.: `417×141` numa tábua `1200×500` → **virada**: 7 por lastro em vez de 6. Travar `Em pé`/`Virada`
  vale mesmo quando a peça passa da tábua — o desenho mostra quanto passa.
- **Avanço permitido** (⚙ Cadastros, padrão **25 mm de cada lado**):
  - *no comprimento* — a peça avança sobre a folga de segurança (folga 100 − avanço 25 = **75 reais**).
    Ex.: `350×380` na tábua `1200` → `3 × 350 = 1050` cabe, **3 por lastro** em vez de 2.
  - *na largura* — quanto a peça pode passar da tábua em **cada lado** (padrão **60 mm/lado**),
    limitado ainda pelo **apoio mínimo da peça** (padrão **67%** da largura dela sobre a tábua).
    Ex.: `615×420` virada numa tábua de 500 passa 115 mm → 57,5 mm de cada lado, apoia 91% → **ok**.
    Já uma ripa de 48 mm com 38 mm no ar é recusada — a fileira da ponta não teria apoio.
- **Ajustar pelo desenho** — cada card tem os controles do arranjo (unidade, tábua, giro, folga, peças por lastro `I × J` e camadas)
- **Ajustar pelo desenho (detalhe)** — três seletores que valem para todas as peças daquele
  arranjo, todos mostrando **quanto cada opção rende por lastro**: a **tábua** (`1600×500 · 2/lastro`),
  o **giro** (Auto · Em pé · Virada ↻) e a **folga** de cada ponta (`folga 100` ou `folga 75`).
  A economia do giro aparece só quando existe ganho real; giros que deixariam a peça **sem apoio**
  na tábua não entram na conta nem no botão **“Aplicar o melhor giro”**.
- **Barra de ocupação fixa** — enquanto você mexe nos desenhos, uma barra grudada no topo mostra
  metros do lote, **ocupação do trilho**, ocupação por unidade e nº de tábuas, com a **variação da
  última alteração** (ex.: `▼ 0,4 m`).
- **Tábuas do lote** — resumo de **quantas tábuas de cada medida** o lote consome (cada pilha ocupa
  uma tábua no trilho), com itens, peças, metros e o % do lote. Serve como lista de separação para o
  corte e sai também no **PDF do lote** e na **impressão dos desenhos**.
- **Unidade no desenho** — cada card mostra a unidade de corte da peça e permite **trocá-la ali mesmo**;
  a impressão dos desenhos sai **separada por unidade** (uma página por unidade, com os KPIs e as
  tábuas daquela unidade).
- **Imprimir só os desenhos** — botões na própria seção, para o lote atual ou para todos os lotes.
  No desenho, o eixo horizontal é o **comprimento** da tábua e a cota vertical à direita é a
  **largura** (fixa, 500 mm). Todos os desenhos de um lote saem **na mesma escala**: a largura de
  500 mm tem sempre a mesma altura no papel, seja a tábua de 910 ou de 1850 — o que muda é o
  comprimento. Além do lote inteiro, dá para imprimir **só os desenhos de uma unidade**.
- No desenho as peças ficam **centralizadas na tábua**: a sobra (ou o que passa) é dividida
  igualmente entre os dois lados, nos dois sentidos.
- **Simulação do lote** — por peça: `I × J` (quantas cabem por lastro, sugerido e editável),
  `K = I×J`, peças por pilha `M = ARREDONDAR.BAIXO(altura da pilha / ALT × K)`,
  nº de pilhas `N = ARREDONDAR.CIMA(QTDE / M)` e **metros de esteira `O = (comp da tábua / 1000) × N`**.
- **Comparar tábuas** — como a tábua cadastrada costuma ser maior que a peça, **sobra tábua** e gasta
  mais esteira. Esta visão sugere a **menor tábua** que ainda encaixa o **mesmo nº de peças por lastro**
  (corta a sobra sem perder encaixe) e **padroniza** os tamanhos num **passo fixo** (padrão: de **5 em 5 mm**,
  ex.: 893 → 895), para não virar medida quebrada. Mostra lado a lado *atual × sugeridas* (metros e ocupação, com
  a economia) e lista os tamanhos sugeridos que surgem. O botão **“Aplicar tábuas sugeridas”** troca as
  tábuas das peças e cadastra os novos tamanhos — reversível.
- **Parâmetros** — altura da pilha (padrão 1200 mm) e o **trilho disponível por unidade**
  (ex.: Unidade I = 116 m, Unidade II = 40 m → 156 m), usados para a **% de ocupação**.
- **GERAL** — consolida todos os lotes importados (necessidade × disponível × ocupação).
- **Aba “🖼 Desenhos”** — os desenhos e a lista de tábuas do lote ganharam tela própria, para não
  rolar o Simulador inteiro. A barra de ocupação acompanha em todas as telas.
- **Cor da borda por medida de tábua** — 16 cores simples de pintar (Azul, Verde, Vermelho, Amarelo,
  Laranja, Roxo, Rosa, Marrom, Preto, Branco, Cinza, Azul claro, Verde claro, Turquesa, Bege, Vinho),
  escolhidas em ⚙ Cadastros. A cor **e o nome**
  aparecem no desenho (`tábua 1500 × 500 mm · borda PRETO`), na coluna **Cor da borda** da tabela de
  tábuas, na coluna Tábua da simulação e **saem impressos** nos relatórios.
- **Desenho do lastro** — mostra **como as peças ficam deitadas na tábua**, vista de cima: a folga de
  segurança de cada ponta, as peças encaixadas (`I × J`) e a **sobra de tábua**. Sai **1 exemplo de cada
  arranjo** do lote (peças com a mesma tábua + mesma medida + mesmo encaixe entram no mesmo desenho) e
  também **entra no PDF**, uma página por lote. Se a peça for mais larga que a tábua, o desenho avisa.
- **Salvar / imprimir** — os botões ficam no **topo** da tela:
  - **Imprimir TODOS os lotes (PDF)** — um único PDF com o **resumo de todos os lotes** na 1ª página
    (consolidado + ocupação por lote e por unidade) e depois **uma página por lote**, com KPIs,
    quebra por unidade de corte e a tabela completa de peças.
  - **Relatório deste lote (PDF)** — só o lote selecionado.
  - **Salvar TODOS os lotes (.xlsx)** — uma aba por lote + aba GERAL.
  - **Comparação de TODOS os lotes (PDF)** — na tela *Comparar tábuas*, uma página por lote.
  - O arquivo **já sai nomeado pelo lote**: `LOTE 140.pdf` / `LOTES 140, 142.pdf` / `LOTE 140.xlsx`
    (o nome sugerido pelo navegador em *Salvar como PDF* vem do título da página).

Tudo roda **no navegador** — nenhum dado é enviado para a internet. O cadastro de tábuas, os
parâmetros e os lotes importados ficam guardados localmente (`localStorage`).

## Arquivos
- `index.html` — o simulador (HTML/CSS/JS autossuficiente).
- `pdf.min.js` + `pdf.worker.min.js` — [pdf.js](https://mozilla.github.io/pdf.js/) (Mozilla, Apache-2.0),
  para ler o PDF no navegador.
- `xlsx.full.min.js` — [SheetJS](https://sheetjs.com/), para exportar `.xlsx`.
- `manifest.json` + ícones — PWA / marca Patrimar.
- `vercel.json` — site estático, `/` → `index.html`.

## Publicar (Vercel)
1. Vercel → **Add New → Project** → selecione este repositório.
2. Framework preset: **Other** (site estático). Sem build.
3. Deploy. A cada `push` na branch de produção, publica sozinho.

## Como o PDF é lido
O relatório “OFs Relacionadas ao Lote” traz, por sub-grupo, uma linha por peça no formato
`QTDE  UN<código>  <descrição com as medidas>  <OF>`. O simulador reconstrói as linhas pelas
coordenadas do texto, ignora as linhas de “Total do Grupo/Sub Grupo”, captura
quantidade + código + medidas + OF e **agrega por código de produto**. O número do lote vem do
cabeçalho (`… - LT 142/26 …`).
