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

=
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
