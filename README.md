# Qualidade-de-Projeto
## Introdução ao dataset
Este dataset consiste na junção de dois datasets, contendo informações a respeito de projetos relacionados ou não a boas praticas de Machine Learning. O intuito proposto para este dataset é que através dele, possa constatar diferenças a respeito do que são bons projetos e o que não são bons proejetos de machine learning.

## Descrição dos dados
O dataset conta com sete colunas:
* project_name: Possui o nome do projeto minerado.
* commit_hash: Possui a hash do commit.
* author_name: Nome do author do commit.
* committer_name: Nome do committer.
* in_main: Se o commit está na main.
* is_merge: Se o commit se encontra em alguma branch.
* Engenheirado: Se o commit é relacionado ou não a boas praticas.

## O que é necessario para utiliza-lo
Para utilizar este dataset, primeiramente é necessario ter a biblioteca Pandas instalado.

## Como manusear o dataset
Caso você já tenha adicionado o dataset ao seu projeto, utilize o codigo a seguir:
```Python
import pandas as pd
df = pd.read_csv("arquivo_final.csv")
```

## Resultados preliminares 
Com o objetivo de obter uma parte dos
