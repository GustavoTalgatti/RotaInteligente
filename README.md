# Rota Inteligente: Otimização de Entregas com Algoritmos de IA

## 1. Descrição do Problema, Desafio Proposto e Objetivos

### Problema

A "Sabor Express", uma pequena empresa local de delivery de alimentos na região central da cidade, enfrenta dificuldades operacionais significativas durante os horários de pico (almoço e jantar). As entregas frequentemente atrasam devido a rotas ineficientes percorridas pelos entregadores. Essa ineficiência resulta em aumento nos custos de combustível e, principalmente, na insatisfação dos clientes. Atualmente, a definição das rotas é feita manualmente, baseando-se apenas na experiência dos entregadores, sem qualquer suporte tecnológico, o que impede a empresa de otimizar suas operações e se manter competitiva.

### Desafio Proposto

Desenvolver uma solução inteligente, utilizando algoritmos de Inteligência Artificial, para otimizar as rotas de entrega da "Sabor Express". A solução deve modelar a área de atuação como um grafo, onde nós representam locais de entrega e arestas representam as ruas, com pesos baseados em distância. O sistema deve ser capaz de encontrar o caminho mais curto entre múltiplos pontos de entrega e, em momentos de alta demanda, agrupar pedidos próximos geograficamente para otimizar a distribuição do trabalho entre os entregadores.

### Objetivos

* **Criar um Sistema Inteligente:** Implementar uma solução baseada em IA para o roteamento de entregas.
* **Otimizar Rotas:** Sugerir as rotas mais eficientes (menor distância) para os entregadores.
* **Reduzir Custos e Tempo:** Diminuir a distância total percorrida, economizando combustível e reduzindo o tempo de entrega.
* **Melhorar Eficiência Operacional:** Aumentar a capacidade de entregas, especialmente em horários de pico.
* **Aumentar Satisfação do Cliente:** Garantir entregas mais rápidas e pontuais.
* **Dividir Carga de Trabalho:** Agrupar entregas por proximidade para distribuir o trabalho entre os entregadores de forma equilibrada.

## 2. Explicação Detalhada da Abordagem Adotada

A solução foi desenvolvida em um ambiente de notebook Python (Google Colab), permitindo a execução sequencial de código, visualização de resultados e documentação. As principais bibliotecas utilizadas foram:

* `networkx`: Para a criação, manipulação e visualização do grafo que representa a cidade.
* `matplotlib`: Para gerar os gráficos e visualizações do grafo e das rotas.
* `numpy`: Para cálculos numéricos, especialmente manipulação de coordenadas.
* `scikit-learn`: Para a implementação do algoritmo de clustering K-Means.
* `math` e `random`: Para cálculos de distância e geração de dados aleatórios.

O fluxo de trabalho seguiu os seguintes passos:

1.  **Modelagem do Grafo:** A região central da cidade foi simulada como um grafo não-direcionado utilizando `networkx`. Foi criada uma grade 10x10, onde cada ponto (nó) representa um cruzamento ou localidade. As ruas (arestas) conectam nós adjacentes (horizontal, vertical e diagonalmente). **O peso de cada aresta representa a distância *simulada* em quilômetros. Essa distância é calculada multiplicando a distância geométrica base (1.0 para horizontal/vertical, √2 para diagonal) por um `fator de realismo aleatório` (entre 0.9 e 1.5, simulando variações como trânsito leve, semáforos, etc.) e por um `fator de escala` (`FATOR_ESCALA_KM = 0.5`), onde cada unidade da grade corresponde a 0.5 km.**
2.  **Simulação dos Pedidos:** A localização do depósito da "Sabor Express" foi definida em um nó específico (`depot_node = 25`). Em seguida, 15 pontos de entrega (`delivery_nodes`) foram gerados aleatoriamente no grafo, excluindo o próprio depósito.
3.  **Agrupamento (Clustering):** Para lidar com múltiplos pedidos e distribuí-los entre 3 entregadores (`num_clusters = 3`), foi aplicado o algoritmo K-Means da biblioteca `scikit-learn`. O algoritmo agrupou os 15 pontos de entrega com base em suas coordenadas (posição no grafo), criando clusters de pedidos geograficamente próximos.
4.  **Otimização de Rota:** Para cada cluster de entregas, foi implementada uma função (`find_optimized_route`) que busca a rota mais curta partindo do depósito, visitando todos os pontos de entrega do cluster e retornando ao depósito. Esta função utiliza uma abordagem heurística "gulosa" (vizinho mais próximo) para determinar a *ordem* de visita aos pontos. Para calcular a distância e o caminho mais curto entre dois pontos *específicos* (segmentos da rota), foi utilizado o algoritmo A\* (`nx.astar_path` e `nx.astar_path_length`), que considera os pesos (distâncias em km) das arestas. A heurística utilizada dentro do A\* foi a distância euclidiana.
5.  **Visualização:** Foram gerados gráficos com `matplotlib` e `networkx` para ilustrar:
    * A representação inicial do grafo da cidade.
    * A localização do depósito e dos pontos de entrega simulados.
    * Os clusters de entrega identificados pelo K-Means.
    * As rotas finais otimizadas para cada cluster, com setas indicando a direção do percurso e rótulos mostrando a distância de cada segmento de rua.

## 3. Algoritmos Utilizados

Os seguintes algoritmos de Inteligência Artificial foram empregados nesta solução:

* **K-Means (Aprendizado Não Supervisionado):** Utilizado para o agrupamento (clustering) dos pontos de entrega. Sua função foi dividir os pedidos em zonas geograficamente coesas, permitindo que cada entregador seja responsável por uma região específica, otimizando a logística e equilibrando a carga de trabalho.
* **A\* (Algoritmo de Busca Heurística):** Empregado para encontrar o caminho mais curto entre dois nós no grafo. Dentro da abordagem de roteamento, o A\* foi crucial para calcular a distância e a sequência de ruas a serem percorridas entre o ponto atual (depósito ou ponto de entrega) e o próximo ponto de entrega mais próximo (definido pela heurística gulosa), sempre considerando as distâncias (pesos) das ruas (arestas).

*Nota: A determinação da ordem de visita aos pontos dentro de cada cluster foi feita usando uma heurística gulosa (vizinho mais próximo), que por sua vez utilizou o A\* para calcular as distâncias entre os pontos.*

## 4. Diagrama do Grafo/Modelo Usado na Solução

As visualizações do grafo e do modelo de solução são geradas dinamicamente pelo notebook Python (`Rota_Inteligente_para_Sabor_Express.ipynb`). Ao executar o código, os seguintes diagramas principais são exibidos:

1.  **Representação da Cidade (Grafo Inicial):** Mostra a grade 10x10 com nós (cruzamentos) e arestas (ruas, incluindo diagonais, com pesos variáveis).
    *Exemplo:*
    ```
    (Execução da Célula 4 do notebook)
    ```
    2.  **Mapa de Entregas:** Apresenta o grafo base com o nó do depósito ("Sabor Express") destacado e os 15 nós de entrega aleatórios marcados.
    *Exemplo:*
    ```
    (Execução da Célula 6 do notebook)
    ```
    3.  **Agrupamento por Zona (K-Means):** Ilustra os pontos de entrega coloridos de acordo com o cluster ao qual foram atribuídos pelo K-Means.
    *Exemplo:*
    ```
    (Execução da Célula 8 do notebook)
    ```
    4.  **Rotas Otimizadas Finais:** O diagrama mais importante, mostrando as rotas calculadas para cada cluster, partindo e retornando ao depósito. As setas indicam a direção do percurso e os rótulos nas arestas mostram as distâncias em km (que agora variam aleatoriamente).
    *Exemplo:*
    ```
    (Execução da Célula 12 do notebook)
    ```
    *Nota: Como os pontos de entrega e os pesos das arestas são gerados aleatoriamente, os diagramas exatos podem variar a cada execução do notebook.*

## 5. Análise dos Resultados, Eficiência da Solução, Limitações e Sugestões de Melhoria

### Análise dos Resultados

A execução do notebook demonstra que a solução de IA foi capaz de:

1.  **Agrupar** os 15 pedidos aleatórios em 3 clusters distintos com base na proximidade.
2.  **Calcular** uma rota otimizada (usando A\* e heurística gulosa) para cada cluster, iniciando e terminando no depósito e visitando todos os pontos do grupo, **considerando as distâncias variáveis das arestas**.
3.  **Visualizar** essas rotas, indicando a direção e as distâncias (variáveis) dos segmentos.
4.  **Quantificar** a distância total de cada rota em quilômetros (conforme exibido na saída da Célula 10).

A análise visual (Célula 12) e os resultados numéricos (distância total percorrida por todos os entregadores) indicam que as rotas traçadas pela IA são coesas dentro de suas zonas e buscam os caminhos mais curtos disponíveis no grafo simulado com pesos aleatórios.

### Eficiência da Solução

Comparada à abordagem manual e ineficiente descrita no problema, a solução baseada em IA apresenta ganhos de eficiência significativos:

* **Redução de Distância:** As rotas calculadas minimizam a distância total percorrida, resultando em economia de combustível e menor desgaste dos veículos.
* **Redução de Tempo:** Percursos mais curtos e otimizados diminuem o tempo total de entrega.
* **Otimização de Recursos:** O agrupamento por K-Means permite a distribuição eficiente do trabalho entre os entregadores, evitando deslocamentos desnecessários e sobreposição de áreas.
* **Melhora na Satisfação do Cliente:** Entregas mais rápidas e pontuais tendem a aumentar a satisfação do cliente.

### Limitações Encontradas

A solução atual, embora funcional para o desafio proposto, possui algumas limitações:

* **Simulação Simplificada:** O grafo em grade 10x10 é uma representação abstrata e não reflete a complexidade real de um mapa urbano.
* **Distâncias Aleatórias, mas Estáticas:** Os pesos das arestas (distâncias) variam aleatoriamente na criação do grafo, simulando pequenas diferenças entre ruas, mas não consideram fatores dinâmicos *dependentes do tempo* como trânsito em tempo real, condições da via, semáforos ou restrições de tráfego durante o cálculo da rota.
* **Otimização Aproximada do TSP:** A heurística gulosa (vizinho mais próximo) usada para definir a ordem de visita dentro do cluster é uma aproximação para o Problema do Caixeiro Viajante (TSP) e não garante a rota globalmente ótima, especialmente para um número maior de pontos.
* **Falta de Restrições Reais:** O modelo não incorpora restrições comuns em delivery, como capacidade do veículo, janelas de horário para entrega, prioridade de pedidos, etc.

### Sugestões de Melhoria

Com base nas limitações e nas pesquisas sugeridas, futuras melhorias poderiam incluir:

* **Mapas Reais:** Utilizar dados geográficos reais (ex: OpenStreetMap) ou APIs de mapas (ex: Google Maps API) para criar um grafo mais preciso da cidade.
* **Dados Dinâmicos:** Integrar APIs que forneçam informações de tráfego em tempo real para ajustar os pesos das arestas dinamicamente.
* **Algoritmos de Otimização Avançados:** Implementar algoritmos mais robustos para resolver o TSP, como Algoritmos Genéticos, Simulated Annealing ou ferramentas de otimizações lineares (MILP).
* **Inclusão de Restrições:** Modelar restrições práticas como janelas de tempo, capacidade dos veículos, compatibilidade de pedidos, etc.
* **Roteamento Dinâmico:** Desenvolver um sistema capaz de recalcular rotas em tempo real caso surjam novos pedidos ou imprevistos (ex: acidentes, bloqueios de rua).
* **Aprendizado por Reforço (RL):** Explorar técnicas de RL para que o sistema aprenda e melhore as estratégias de roteamento com base em dados históricos e feedback.
