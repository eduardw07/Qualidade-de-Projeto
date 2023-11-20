# Qualidade-de-Projeto
## Introdução ao dataset
Este dataset consiste na junção de dois datasets, contendo informações a respeito de projetos relacionados, ou não, a boas práticas de Machine Learning. O intuito proposto para este dataset é que, através dele, seja possível perceber as diferenças a respeito do que são bons projetos e o que não são bons projetos de machine learning.

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
O arquivo de nome requirements.txt, possui informações a cerca das bibliotecas e versões utilizadas nos passos a seguinte, porém inicialmente para utilizar este dataset é necessario apenas possuir a biblioteca pandas instalado.

## Como manusear o dataset
Inicialmente, baixe os arquivos .zip, devido ao seu tamanho, o dataset teve que ser compactado para que pudesse ser disponibilizado. Após baixar os arquivos, descompacte utilizando um descompactador de sua preferencia.

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
Embora ainda existe uma longa caminhada para que seja possivel captar todas as diferenças a respeito do que realmente são ou não bons projetos de Machine Learning, com o estudo desenvolvido até o momento já se tornou possivel obter resultados preliminares. Com o avanço do estudo a cerca do dataset, algumas ideias surgiram para tentar identificar diferenças a respeito dos projetos. 
Para conseguir captar algumas diferenças, foi realizado uma contagem entre as colunas commit_hash e project_name com o objetivo de se ter uma contagem de commits por projeto, utilizando o seguinte codigo:
```Python
total_commits_por_repositorio = df.groupby('project_name')['commit_hash'].count()
```
Que obteve o seguinte resultado:

![image](https://github.com/eduardw07/Qualidade-de-Projeto/assets/45314550/5ff9ed76-b9ad-473f-a637-a7e6726334e6)

A partir disso, foi utilizado um algoritmo utilizando a coluna Engenheirados, para mostrar graficamente os 10 repositorios com mais commits, com o intuito de inferir informações a cerca desta visualização. Segue o algoritmo utilizado e o resultado grafico deste algoritmo.
```Python
import matplotlib.pyplot as plt

total_commits_por_repositorio = df.groupby('project_name')['commit_hash'].count()
total_commits_por_repositorio = total_commits_por_repositorio.reset_index()

total_commits_por_repositorio = total_commits_por_repositorio.sort_values(by='commit_hash', ascending=False)

top_10_repositorios = total_commits_por_repositorio.head(10)

plt.figure(figsize=(10, 6))
plt.bar(top_10_repositorios['project_name'], top_10_repositorios['commit_hash'])
plt.title('Top 10 Repositórios com Mais Commits')
plt.xlabel('Repositório')
plt.ylabel('Número de Commits')
plt.xticks(rotation=90)
plt.show()
```

![Projetos com mais commits](https://github.com/eduardw07/Qualidade-de-Projeto/assets/45314550/e32e102e-848d-4883-bccf-9c350e7e6b5a)

Ao analisar o grafico, foi possivel perceber que 9 dos 10 repositorios com mais commits, são relacionados a boas praticas. A partir disso pode-se inferir que em geral, repositorios com mais commits possuem uma qualidade maior, pois passam por mais revisão em suas atividades.
Para garantir a validade deste teste, foi realizado um algoritmo de classificação KNN com um classification report, para obter um relatorio mais preciso dos meus resultados.

O seguinte algoritmo foi utilizado:
```Python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import classification_report
from sklearn.preprocessing import LabelEncoder

df = pd.read_csv("arquivo_final.csv")

df2 = df.drop(['author_name','committer_name'],axis=1)

label_encoder = LabelEncoder()

df2['project_name'] = label_encoder.fit_transform(df['project_name'])
df2['commit_hash'] = label_encoder.fit_transform(df['commit_hash'])

X = df2.drop('Engenheirado', axis=1)
y = df2['Engenheirado']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

knn_model = KNeighborsClassifier(n_neighbors=3)

knn_model.fit(X_train, y_train)

y_pred = knn_model.predict(X_test)


print("Relatório de Classificação:\n", classification_report(y_test, y_pred))
```

O resultado obtido:

![image](https://github.com/eduardw07/Qualidade-de-Projeto/assets/45314550/61711d41-a936-4226-bfee-a7fa92a2f239)

O que se pode inferir é que com a aplicação do classificador KNN, os resultados obtidos foram consistentes com o resultado de que projetos com mais commits possuem uma maior qualidade. Ao analisar o resultado do KNN atraves do classification report é possivel notar a precisão, o recall e o F1 score para ambas as classes. Adotando o fato de que as classes que indicam boas praticas são representadas pelo valor 1, a precisão, recall e F1 score possuem valores altos, o que sugere que o modelo consegue ser consistente na identificação desta classe.
Para corroborar ainda mais com os resultados preliminares, foi utilizada uma metrica para avaliar o desempenho do modelo classificador KNN, a curva ROC.

Segue o codigo utilizado para a realização da curva ROC:
```Python
from sklearn.metrics import roc_curve, auc

y_prob = knn_model.predict_proba(X_test)[:, 1]


fpr, tpr, thresholds = roc_curve(y_test, y_prob)
roc_auc = auc(fpr, tpr)


plt.figure(figsize=(8, 6))
plt.plot(fpr, tpr, color='darkorange', lw=2, label='Curva ROC (área = {:.2f})'.format(roc_auc))
plt.plot([0, 1], [0, 1], color='navy', lw=2, linestyle='--', label='Aleatório (área = 0.5)')
plt.xlabel('Taxa de Falsos Positivos (1 - Especificidade)')
plt.ylabel('Taxa de Verdadeiros Positivos (Sensibilidade)')
plt.title('Curva ROC')
plt.legend(loc='lower right')
plt.show()
```
Visualização da curva ROC:

![Curva_ROC_PROJETO INTEGRADOR](https://github.com/eduardw07/Qualidade-de-Projeto/assets/45314550/6f38adc5-46c4-422f-a480-d5dce58551b5)

Analisando o grafico da curva ROC, é perceptivel que a AUC-ROC é alta (Proxima de 1), o que indica que o modelo esta sendo capaz de distinguir muito bem entre as duas classes. O ponto superior esquerdo da curva ROC indica um bom desempenho, indicando uma alta taxa de verdadeiros positivos (sensibilidade) e uma baixa taxa de falsos positivos (1 - especificidade). Por fim, podemos afirmar que a alta AUC-ROC sugere que o modelo é eficaz na discriminação entre as classes, o que valida a observação inicial de que projetos com mais commits em geral possuem uma maior qualidade.
