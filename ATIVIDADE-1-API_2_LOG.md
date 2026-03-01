# 📊 Análise de Importações -- COMEXSTAT (2024--2025)

Este projeto realiza a consolidação, tratamento e análise de dados de
importações a partir das bases do COMEXSTAT, utilizando Python e Pandas.

O objetivo principal é gerar uma série temporal mensal com o total
importado (em kg) no período de janeiro/2025 até janeiro/2026.

------------------------------------------------------------------------

## 🛠️ Tecnologias Utilizadas

-   Python 3
-   Pandas
-   Matplotlib
-   Google Colab
-   Google Drive

------------------------------------------------------------------------
## 🛠️ ATIVIDADE-1-API_2_LOG.ipynb

import pandas as pd

data = {'coluna_1':[1.5,2.0,1.7,2.2,1.9],
        'coluna_2':[10.0,12.0,11.5,13.0,11.0],
        'coluna_3':[0,1,0,1,0],
        'coluna_4': [1,0,1,1,0],
        'coluna_5': [20.5,25.5,22.0,27.0,23.0],}

df = pd.DataFrame(data)

print(df)

from google.colab import drive

drive.mount('/content/drive/')

origem ='/content/drive/MyDrive/data/COMEXSTAT/'

arquivo_1 = origem + 'EXP_2024.csv'
arquivo_2 = origem + 'EXP_2025.csv'
ncm = origem + 'NCM.csv'
pais = origem + 'PAIS.csv'
vias = origem + 'VIA.csv'
urf = origem + 'URF.csv'

exp24 = pd.read_csv(arquivo_1, low_memory=False, sep=';', encoding='UTF-8')
exp25 = pd.read_csv(arquivo_2, low_memory=False, sep=';', encoding='UTF-8')
expncm = pd.read_csv(ncm, low_memory=False, sep=';', encoding='UTF-8')
exppais = pd.read_csv(pais, low_memory=False, sep=';', encoding='UTF-8')
expvias = pd.read_csv(vias, low_memory=False, sep=';', encoding='UTF-8')
expurf = pd.read_csv(urf, low_memory=False, sep=';', encoding='UTF-8')

exp24.info()

exp25.info()

expncm.info()

exp_final = pd.concat([exp24,exp25], ignore_index=True)
exp_final . info()

exp_final.head(5)

exp_final.tail(5)

exp_final = exp_final.merge(exppais[['CO_PAIS', 'NO_PAIS']],on='CO_PAIS', how='left')

exp_final.head(5)

exp_final = exp_final.merge(expncm[['CO_NCM', 'NO_NCM_POR']],on='CO_NCM', how='left')

exp_final.head(5)

coluna = 'NO_PAIS'
CP = exp_final[coluna].value_counts().sort_values(ascending=False)
print(f'Total de Operações {coluna}:')
print(CP)

exp_final.to_csv(origem+'exp_final.csv', index=False)

import matplotlib.pyplot as plt 


exp_final['CO_ANO'] = exp_final['CO_ANO'].astype(int) 
exp_final['CO_MES'] = exp_final['CO_MES'].astype(int)

exp_final['data'] = pd.to_datetime( exp_final['CO_ANO'].astype(str) + '-' + exp_final['CO_MES'].astype(str) + '-01' )

filtro = (exp_final['data'] >= '2025-01-01') & (exp_final['data'] <= '2026-01-31')
df_periodo = exp_final.loc[filtro]

serie_mensal = (
    df_periodo
    .groupby('data')['KG_LIQUIDO']
    .sum()
    .reset_index()
)

plt.figure(figsize=(10,5))
plt.plot(serie_mensal['data'], serie_mensal['KG_LIQUIDO'], marker='o')

plt.xlabel('Meses')
plt.ylabel('Total Importado (kg)')
plt.title('Importações Mensais (Jan/2025 – Jan/2026)')

plt.xticks(rotation=45)
plt.grid(True)
plt.tight_layout()
plt.show()   

serie_mensal.to_csv(origem + 'importacoes_mensais_2025_2026.csv', index=False)

## 🔄 Etapas do Processo

### 1️⃣ Importação das Bibliotecas

``` python
import pandas as pd
import matplotlib.pyplot as plt
```

Pandas é utilizado para manipulação de dados.\
Matplotlib é utilizado para visualização gráfica.

------------------------------------------------------------------------

### 2️⃣ Leitura e Consolidação dos Dados

-   Leitura dos arquivos CSV de 2024 e 2025\
-   União das bases utilizando `pd.concat()`\
-   Junção com tabelas auxiliares (País e NCM) utilizando `merge()`

------------------------------------------------------------------------

### 3️⃣ Tratamento de Datas

-   Conversão das colunas `CO_ANO` e `CO_MES` para inteiro\
-   Criação de coluna `datetime` para análise temporal

------------------------------------------------------------------------

### 4️⃣ Filtro do Período

Seleção dos dados entre janeiro/2025 e janeiro/2026 utilizando filtro
condicional.

------------------------------------------------------------------------

### 5️⃣ Agregação Mensal

Agrupamento por mês com:

``` python
groupby('data')['KG_LIQUIDO'].sum()
```

Cálculo do total importado em kg por mês.

------------------------------------------------------------------------

### 6️⃣ Visualização

Geração de gráfico de linha mostrando a evolução mensal das importações.

------------------------------------------------------------------------

### 7️⃣ Exportação

Exportação da série mensal consolidada para arquivo CSV.

------------------------------------------------------------------------

## 📊 Resultado Final

O projeto gera:

-   Base consolidada 2024--2025
-   Série temporal mensal
-   Total importado por mês (kg)
-   Arquivo CSV exportado
-   Visualização gráfica da evolução

------------------------------------------------------------------------

## 📌 Conceitos Aplicados

-   ETL (Extract, Transform, Load)
-   Merge (JOIN)
-   Manipulação de Datas
-   Filtro Condicional
-   GroupBy + Sum
-   Visualização de Dados
-   Exportação de Arquivos

------------------------------------------------------------------------

## 👨‍💻 Autor

Projeto desenvolvido para fins de estudo e prática em análise de dados
com Python.
