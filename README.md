# UCS × BFS: rotas em transporte público com custos diferentes

Aplicação e comparação da **busca em largura (BFS)** e da **busca de custo uniforme (UCS)** no planejamento de rotas numa rede de transporte público em que cada trecho tem tempo e tarifa diferentes.

Este repositório tem o material das **partes 3 (aplicação) e 4 (resultados)** da apresentação.

> ⚠️ Os bairros e pontos de referência são de **João Pessoa (PB)**, mas **as linhas, os tempos e as tarifas são fictícios**. Eles foram escolhidos para ilustrar as diferenças entre os algoritmos.

## A ideia em uma frase

A **BFS** encontra a rota com **menos trechos** e ignora os custos. A **UCS** encontra a rota de **menor custo acumulado**, aqui o tempo de viagem. Em transporte público, a rota com menos trechos muitas vezes não é a mais rápida.

## Notebook da apresentação

O notebook oficial é [`notebooks/ucs_vs_bfs_apresentacao.ipynb`](notebooks/ucs_vs_bfs_apresentacao.ipynb), pensado para uma apresentação de ~10 minutos. Ele já está salvo com as saídas, então dá para ler direto no GitHub.

**Parte 3: aplicação**
- **O problema:** a rede como grafo, com 16 paradas e 23 trechos de ônibus, trem e caminhada. Cada trecho tem tempo e tarifa.
- **Os dois algoritmos**, explicados brevemente:
  - **BFS:** explora a rede em camadas com uma fila comum e para assim que enxerga o destino.
  - **UCS:** explora sempre a parada de menor custo acumulado, com uma fila de prioridade, e só para quando o destino sai da fila.
- Uma figura que mostra **o que cada algoritmo "vê"** na mesma rede: para a BFS, todo trecho vale 1; para a UCS, cada trecho vale seus minutos.
- O código dos dois algoritmos, implementados **do zero**.
- **Três percursos**, cada um com o mapa das duas rotas e o itinerário trecho a trecho:
  - UFPB → Tambaú;
  - Valentina → Bayeux;
  - Mangabeira → Bessa.

**Parte 4: resultados**
- Tabela comparando trechos, tempo, tarifa, baldeações e paradas examinadas.
- Gráfico com três painéis: nº de trechos (a BFS sempre vence), tempo total (a UCS sempre vence) e esforço de cada algoritmo.
- Conclusões, com o A\* como próximo passo.

## Resultados

| Percurso | BFS | UCS (tempo) | BFS leva a mais |
|---|---|---|---|
| UFPB → Tambaú | 2 trechos, 45 min | 3 trechos, **32 min** | 13 min (41%) |
| Valentina → Bayeux | 3 trechos, 75 min, R$ 15,00 | 4 trechos, **70 min**, R$ 5,50 | 5 min (7%) |
| Mangabeira → Bessa | 4 trechos, 97 min | 5 trechos, **63 min** | 34 min (54%) |

- A **BFS** sempre encontra a rota com menos trechos. A **UCS** sempre encontra a mais rápida, mesmo que isso exija um trecho a mais.
- Em Valentina → Bayeux, a UCS usa o trem: a rota sai mais rápida **e** mais barata.
- A UCS examina um pouco mais de paradas, porque só para quando tem certeza de que não existe rota mais rápida. Nesta rede, isso leva microssegundos.

## Material complementar

O notebook [`notebooks/ucs_vs_bfs_transporte.ipynb`](notebooks/ucs_vs_bfs_transporte.ipynb) é uma **versão estendida**, para quem quiser se aprofundar. Ele traz:

- outras funções de custo: tarifa, custo generalizado e custo unitário;
- visualização **passo a passo** da execução, com o conteúdo da fila e da heap a cada passo;
- verificação da UCS contra o Dijkstra do `networkx` em todos os pares de paradas;
- mapa de calor da perda da BFS nos 240 pares e o espaço de rotas tempo × tarifa (fronteira de Pareto);
- custo computacional, experimento de escala em grades de até 50×50 e quadro teórico.

Nos dois notebooks, BFS e UCS são implementadas do zero. O `networkx` é usado só para desenhar o grafo (e, na versão estendida, para validar a UCS).

## Como executar

Requer Python 3.10 ou mais recente.

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# Reexecuta o notebook da apresentação e salva as saídas no próprio arquivo
jupyter nbconvert --to notebook --execute --inplace notebooks/ucs_vs_bfs_apresentacao.ipynb

# (opcional) versão estendida
jupyter nbconvert --to notebook --execute --inplace notebooks/ucs_vs_bfs_transporte.ipynb
```

Para abrir e editar os notebooks de forma interativa, use o VS Code com a extensão Jupyter ou instale o JupyterLab (`pip install jupyterlab` e depois `jupyter lab`).

## Estrutura

```
.
├── notebooks/
│   ├── ucs_vs_bfs_apresentacao.ipynb # notebook da apresentação (partes 3 e 4)
│   └── ucs_vs_bfs_transporte.ipynb   # versão estendida (material complementar)
├── requirements.txt                  # pandas, matplotlib, networkx, ipykernel, nbconvert
└── README.md
```
