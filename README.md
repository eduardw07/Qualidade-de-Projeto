# Qualidade-de-Projeto
## Introdução ao dataset
Este dataset consiste na junção de dois datasets, contendo informações a respeito de projetos relacionados ou não a boas praticas de Machine Learning. O intuito proposto para este dataset é que através dele, seja possivel perceber as diferenças a respeito do que são bons projetos e o que não são bons proejetos de machine learning.

## Descrição dos dados
O dataset conta com sete colunas:
* project_name: Possui o nome do projeto minerado.
* commit_hash: Possui a hash do commit.
* author_name: Nome do author do commit.
* committer_name: Nome do committer.
* in_main: Uma coluna booleana onde 1 significa que o commit está na main e 0 que não está.
* is_merge: Uma coluna booleana onde 1 significa que o commit se encontra em uma branch e 0 que não está.
* Engenheirado: Uma coluna booleana onde 1 significa que o projeto é relacionado a boas praticas e 0 que não é relacionado a boas praticas.

## O que é necessario para utiliza-lo
Para utilizar este dataset, primeiramente é necessario ter a biblioteca Pandas instalado.

## Como manusear o dataset
Caso você já tenha adicionado o dataset ao seu projeto, utilize o codigo a seguir:
```Python
import pandas as pd
df = pd.read_csv("arquivo_final.csv")
```

Caso você ainda não tenha adicionado o dataset ao seu projeto, utilize este codigo:
```Python
import pandas as pd
df = pd.read_csv(r"caminho_do_dataset/arquivo_final.csv")
```

Após carregar o dataset, para iniciar sua analise, considere utilizar o seguinte codigo:
```Python
print(df.head(5))
```

## Resultados preliminares 

