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
  1200×380, 1360×380, 1600×380, 1850×380). O simulador **sugere** a tábua de menor comprimento
  que comporta a peça (peça + 400 mm); você pode trocar a qualquer momento. A coluna
  **Sug. tábua** mostra a medida sugerida (comp×larg) e marca com **≠** as peças cujo encaixe
  atual difere da sugestão. O botão **“Simular com tábuas sugeridas”** aplica a tábua sugerida a
  todas as peças do lote de uma vez.
- **Simulação do lote** — por peça: `I × J` (quantas cabem por lastro, sugerido e editável),
  `K = I×J`, peças por pilha `M = ARREDONDAR.BAIXO(altura da pilha / ALT × K)`,
  nº de pilhas `N = ARREDONDAR.CIMA(QTDE / M)` e **metros de esteira `O = (comp da tábua / 1000) × N`**.
- **Por tábua** — consolida o lote **pela tábua usada** (a sugerida pelo algoritmo, ou a que você
  trocou): por tamanho de tábua, quantos tipos de peça caem nela, quantas peças, **pilhas**,
  **metros de esteira** e a **% dos metros** do lote.
- **Parâmetros** — altura da pilha (padrão 1200 mm) e o **trilho disponível por unidade**
  (ex.: Unidade I = 116 m, Unidade II = 40 m → 156 m), usados para a **% de ocupação**.
- **GERAL** — consolida todos os lotes importados (necessidade × disponível × ocupação).
- **Exportar** — baixa a simulação em `.xlsx` (uma aba por lote + aba GERAL) ou gera um relatório
  em PDF (impressão do navegador).

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
