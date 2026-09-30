# UCS × BFS: rotas em transporte público com custos diferentes

Implementação e comparação da **busca em largura (BFS)** e da **busca de custo uniforme (UCS)** aplicadas ao planejamento de rotas numa rede de transporte público em que cada trecho tem tempo e tarifa diferentes.

Este repositório tem o material das **partes 4 (implementação) e 5 (análise e comparação)** da apresentação.

> ⚠️ Os bairros e pontos de referência são de **João Pessoa (PB)**, mas **as linhas, os tempos e as tarifas são fictícios**. Eles foram escolhidos para ilustrar as diferenças entre os algoritmos.

## A ideia em uma frase

A **BFS** minimiza o **número de trechos** e ignora os custos. A **UCS** minimiza o **custo acumulado**, seja ele tempo, tarifa ou uma combinação dos dois. Em transporte público, a rota com menos trechos quase nunca é a mais rápida nem a mais barata.

| Viagem UFPB → Tambaú | Rota | Trechos | Tempo |
|---|---|---|---|
| BFS | UFPB → Castelo Branco → Tambaú | 2 | 45 min |
| UCS (tempo) | UFPB → Castelo Branco → Torre → Tambaú | 3 | **32 min** |

## Conteúdo do notebook

O notebook fica em [`notebooks/ucs_vs_bfs_transporte.ipynb`](notebooks/ucs_vs_bfs_transporte.ipynb) e já está salvo com as saídas, então dá para ler direto no GitHub.

**Modelagem**
- Grafo não-direcionado com 16 paradas e 23 trechos (ônibus, trem e caminhada), representado como lista de adjacência.
- Funções de custo: `custo_tempo`, `custo_tarifa`, `custo_generalizado` (tempo + 6 min × tarifa) e `custo_unitario`.
- Desenho do grafo com posições que aproximam a geografia da cidade e destaque das rotas encontradas.

**Parte 4: implementação**
- BFS com `collections.deque` e teste de objetivo na geração.
- UCS com `heapq`, contador de desempate, *lazy deletion* e teste de objetivo na expansão. Ela recebe a função de custo como parâmetro.
- Verificação de corretude da UCS contra `networkx.dijkstra_path_length` em todos os pares de paradas.

**Parte 5: análise e comparação**
- Cinco viagens comparadas em tempo, tarifa, custo generalizado e baldeações.
- Quanto a BFS perde em relação ao ótimo de cada critério.
- Prova prática de que a UCS com custo unitário encontra o mesmo número de trechos que a BFS.
- Custo computacional: nós expandidos, gerados, pico da fronteira e tempo de execução.
- Experimento de escala em grades de 5×5 a 50×50 com pesos aleatórios.
- Quadro teórico e conclusões, incluindo o A\* como próximo passo.

BFS e UCS são implementadas **do zero**. O `networkx` é usado só para desenhar o grafo e validar a UCS.

## Principais resultados

- A rota da BFS **não foi a mais rápida em nenhuma** das 5 viagens. Ela ficou em média **27%** acima do ótimo em tempo, chegando a **54%**.
- Em tarifa, a BFS chegou a custar **R$ 20,00** onde a rota ótima custa **R$ 5,50** (UFPB → Cabedelo).
- Nas grades aleatórias, a BFS não encontrou a rota mais rápida em **87%** dos 140 casos. O excesso médio cresce com o tamanho do grafo: cerca de 36% na grade 5×5 e 73% na 50×50.
- Os dois algoritmos expandem uma quantidade parecida de nós. A UCS é cerca de **2 a 3 vezes mais lenta** por causa do custo do heap, um preço baixo por rotas corretas.

## Como executar

Requer Python 3.10 ou mais recente.

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# Reexecuta o notebook inteiro e salva as saídas no próprio arquivo
jupyter nbconvert --to notebook --execute --inplace notebooks/ucs_vs_bfs_transporte.ipynb
```

Para abrir e editar o notebook de forma interativa, use o VS Code com a extensão Jupyter ou instale o JupyterLab (`pip install jupyterlab` e depois `jupyter lab`).

A execução completa leva cerca de 15 segundos. Todos os resultados são reprodutíveis, exceto os tempos em microssegundos, que variam de máquina para máquina.

## Estrutura

```
.
├── notebooks/
│   └── ucs_vs_bfs_transporte.ipynb   # partes 4 e 5 da apresentação
├── requirements.txt                  # pandas, matplotlib, networkx, ipykernel, nbconvert
└── README.md
```
