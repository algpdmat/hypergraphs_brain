# Structural and Dynamic Differences in High-Order Brain Networks

This project is part of the PhD thesis titled "Understanding Structural and Dynamic Differences in High-Order Brain Networks between Individuals with Autism Spectrum Disorder and Typical Development: A Topological Data Analysis Approach."

## Prerequisites

- Python 3.7 or higher
- Required packages:
  - numpy
  - pandas
  - networkx
  - scikit-learn
  - glob
  - matplotlib
  - wordcloud

Install the required packages using the following command:
```bash
pip install numpy pandas networkx scikit-learn matplotlib wordcloud



Claro, vamos otimizar as funções, mantendo a estrutura e as funcionalidades. Abaixo estão as funções otimizadas a partir da seção que você especificou.

Funções Otimizadas
1. Criando Hiper-Grafos para Calcular os Hubs
Vamos unificar as funções HG_aut e HG_control em uma única função create_hypergraph que pode ser usada para ambos os casos. Isso elimina redundância e torna o código mais eficiente e fácil de manter.

python
Copiar código
import numpy as np
import networkx as nx

def create_hypergraph(graphs):
    """
    Cria hiper-grafos a partir de uma lista de grafos.

    Parâmetros:
    graphs (list): Lista de grafos NetworkX.

    Retorno:
    list: Lista de conjuntos de cliques (triângulos) para cada grafo.
    """
    cliques_list = []

    for G in graphs:
        triangles = [clique for clique in nx.enumerate_all_cliques(G) if len(clique) == 3]
        cliques_list.append(triangles)

    return cliques_list
2. Criando Conjuntos de Cliques para os Grupos control e aut
python
Copiar código
# CLIQUES PARA OS GRUPOS CONTROL E AUT
tri_control = create_hypergraph(control_graphs) 
tri_aut = create_hypergraph(aut_graphs)
3. Calculando os Hubs dos Grafos e Hiper-Grafos
python
Copiar código
def calculate_hubs(cliques_list):
    """
    Calcula os hubs dos grafos duais a partir dos cliques.

    Parâmetros:
    cliques_list (list): Lista de conjuntos de cliques.

    Retorno:
    list: Lista de DataFrames com centralidades para cada conjunto de cliques.
    """
    hubs = []
    for cliques in cliques_list:
        c = calculate_dual_centralities(cliques)
        hubs.append(c)
    return hubs

hubs_aut_hp = calculate_hubs(tri_aut)
hubs_control_hp = calculate_hubs(tri_control)
4. Separando os DataFrames por Métricas
python
Copiar código
def split_dataframe(df, metric):
    """
    Separa um DataFrame com base em uma métrica específica.

    Parâmetros:
    df (DataFrame): DataFrame com as centralidades.
    metric (str): Métrica a ser usada para ordenação e separação.

    Retorno:
    DataFrame: DataFrame contendo as colunas 'Clique' e a métrica especificada.
    """
    df = df.reset_index()
    sorted_df = df.sort_values(by=metric, ascending=False)
    result_df = pd.DataFrame({'Clique': sorted_df['Clique'], metric: sorted_df[metric]})
    return result_df

def split_hubs(hubs):
    """
    Separa os hubs em DataFrames individuais por métrica.

    Parâmetros:
    hubs (list): Lista de DataFrames com centralidades.

    Retorno:
    list: Lista de DataFrames separados por métrica.
    """
    hubs_split = []
    for df in hubs:
        df_bc = split_dataframe(df, 'BC')
        df_dc = split_dataframe(df, 'DC')
        df_ec = split_dataframe(df, 'EC')
        hubs_split.extend([df_bc, df_dc, df_ec])
    return hubs_split

hubs_aut_hp_split = split_hubs(hubs_aut_hp)
hubs_control_hp_split = split_hubs(hubs_control_hp)
5. Associando os Cliques às Regiões da Lista de Nomes
python
Copiar código
def associar_cliques_regioes(df, lista_nomes, metrica):
    """
    Associa os cliques às regiões com base na lista de nomes.

    Parâmetros:
    df (DataFrame): DataFrame com cliques e suas métricas.
    lista_nomes (list): Lista de nomes das regiões.
    metrica (str): Métrica a ser associada.

    Retorno:
    DataFrame: DataFrame com cliques, métricas e nomes das regiões.
    """
    cliques_regioes = []
    for clique in df['Clique']:
        regioes_clique = [lista_nomes[int(indice)-1] for indice in clique]
        cliques_regioes.append(regioes_clique)

    df_resultado = pd.DataFrame({'Clique': df['Clique'], metrica: df[metrica], 'Regioes': cliques_regioes})
    return df_resultado

# HUBS COM NOMES DAS REGIOES DOS HYPERGRAFOS
df_bc_aut_nomes = associar_cliques_regioes(df_bc_aut, nomes, 'BC')
df_dc_aut_nomes = associar_cliques_regioes(df_dc_aut, nomes, 'DC')
df_ec_aut_nomes = associar_cliques_regioes(df_ec_aut, nomes, 'EC')

df_bc_control_nomes = associar_cliques_regioes(df_bc_control, nomes, 'BC')
df_dc_control_nomes = associar_cliques_regioes(df_dc_control, nomes, 'DC')
df_ec_control_nomes = associar_cliques_regioes(df_ec_control, nomes, 'EC')
Exemplo Completo
python
Copiar código
# Exemplo de uso das funções otimizadas

# Supondo que control_graphs e aut_graphs já estejam definidos

# Criando os hiper-grafos para ambos os grupos
tri_control = create_hypergraph(control_graphs)
tri_aut = create_hypergraph(aut_graphs)

# Calculando os hubs dos hiper-grafos
hubs_aut_hp = calculate_hubs(tri_aut)
hubs_control_hp = calculate_hubs(tri_control)

# Separando os DataFrames por métricas
hubs_aut_hp_split = split_hubs(hubs_aut_hp)
hubs_control_hp_split = split_hubs(hubs_control_hp)

# Associando os cliques às regiões
nomes = [...]  # Lista de nomes das regiões

df_bc_aut_nomes = associar_cliques_regioes(hubs_aut_hp_split[0], nomes, 'BC')
df_dc_aut_nomes = associar_cliques_regioes(hubs_aut_hp_split[1], nomes, 'DC')
df_ec_aut_nomes = associar_cliques_regioes(hubs_aut_hp_split[2], nomes, 'EC')

df_bc_control_nomes = associar_cliques_regioes(hubs_control_hp_split[0], nomes, 'BC')
df_dc_control_nomes = associar_cliques_regioes(hubs_control_hp_split[1], nomes, 'DC')
df_ec_control_nomes = associar_cliques_regioes(hubs_control_hp_split[2], nomes, 'EC')
README.md
markdown
Copiar código
# Structural and Dynamic Differences in High-Order Brain Networks

This project is part of the PhD thesis titled "Understanding Structural and Dynamic Differences in High-Order Brain Networks between Individuals with Autism Spectrum Disorder and Typical Development: A Topological Data Analysis Approach."

## Prerequisites

- Python 3.7 or higher
- Required packages:
  - numpy
  - pandas
  - networkx
  - scikit-learn
  - glob
  - matplotlib
  - wordcloud

Install the required packages using the following command:
```bash
pip install numpy pandas networkx scikit-learn matplotlib wordcloud
Project Structure
create_hypergraph(graphs): Creates hypergraphs from a list of graphs.
calculate_hubs(cliques_list): Calculates hubs from a list of cliques.
split_dataframe(df, metric): Splits a DataFrame by a specified metric.
split_hubs(hubs): Splits hub DataFrames by metrics.
associar_cliques_regioes(df, lista_nomes, metrica): Associates cliques with regions based on a list of names.
