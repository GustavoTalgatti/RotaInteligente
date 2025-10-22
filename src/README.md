# Rota Inteligente - Instruções de Execução

Este diretório contém o código principal do projeto "Rota Inteligente", implementado em um notebook Python (`Rota_Inteligente_para_Sabor_Express.ipynb`).

## Pré-requisitos

* **Git:** Para clonar o repositório.
* **Python:** Versão 3.8 ou superior recomendada.
* **Ambiente Virtual (Opcional, mas recomendado):** Ferramentas como `venv` ou `conda` para isolar as dependências do projeto.
* **Jupyter:** Jupyter Notebook, Jupyter Lab, Google Colab ou uma IDE compatível (como VS Code com a extensão Python/Jupyter) para executar o arquivo `.ipynb`.

## Passo a Passo para Execução

1.  **Clonar o Repositório:**
    Abra seu terminal ou prompt de comando e clone o repositório principal (substitua `<URL_DO_REPOSITORIO>` pela URL real do seu GitHub):
    ```bash
    git clone <URL_DO_REPOSITORIO>
    cd RotaInteligente
    ```

2.  **Navegar para a Pasta `src`:**
    Entre no diretório `src` onde está o notebook:
    ```bash
    cd src
    ```

3.  **(Opcional) Criar Ambiente Virtual:**
    Recomendamos criar um ambiente virtual para instalar as bibliotecas:
    ```bash
    # Usando venv (padrão Python 3)
    python -m venv venv
    # Ativar no Linux/macOS
    source venv/bin/activate
    # Ativar no Windows (cmd)
    venv\Scripts\activate
    # Ativar no Windows (PowerShell)
    venv\Scripts\Activate.ps1
    ```
    *Se estiver usando Conda, crie um ambiente com `conda create -n rota_inteligente python=3.9` (ou outra versão) e ative com `conda activate rota_inteligente`.*

4.  **Instalar Dependências:**
    Instale as bibliotecas listadas no arquivo `requirements.txt` (que está na pasta raiz, por isso `../`):
    ```bash
    pip install -r ../requirements.txt
    ```
    *Se estiver usando Conda e preferir, pode instalar individualmente: `conda install networkx matplotlib numpy pandas scikit-learn`.*

5.  **Executar o Notebook:**
    Inicie o ambiente Jupyter de sua preferência e abra o arquivo `Rota_Inteligente_para_Sabor_Express.ipynb`:
    ```bash
    # Usando Jupyter Notebook
    jupyter notebook Rota_Inteligente_para_Sabor_Express.ipynb

    # Ou usando Jupyter Lab
    jupyter lab
    ```
    *Alternativamente, abra a pasta `src` no VS Code ou faça upload do notebook para o Google Colab.*

    Dentro do ambiente Jupyter, execute as células do notebook sequencialmente (por exemplo, usando "Run All" ou Shift+Enter em cada célula).

## Execução e Resultados (Importante!)

* **Geração Aleatória:** Este projeto **gera dados de forma aleatória** a cada execução:
    * Os **pesos das arestas** (distâncias simuladas das ruas) recebem um fator de realismo aleatório.
    * Os **pontos de entrega** são selecionados aleatoriamente no grafo.
* **Resultados Variáveis:** Devido à aleatoriedade, **cada execução do notebook gerará um cenário ligeiramente diferente**, resultando em:
    * Agrupamentos (clusters) que podem variar.
    * Distâncias totais de rota diferentes.
    * Visualizações (gráficos) distintas.
* **Lógica Consistente:** Apesar da variação nos dados de entrada, a lógica de otimização (K-Means para agrupar e A\* com heurística gulosa para rotear) **sempre buscará a melhor solução possível para o cenário gerado naquela execução específica**.
* **Saídas:** Os gráficos (mapa inicial, clusters, rotas finais com direções e distâncias) são exibidos diretamente como saída das células no notebook. A pasta `images/` neste diretório pode ser usada para salvar exemplos dessas saídas, se desejado.

---
