# Brasileirão 2021 (EDA)

![](./img/Brasileirao.jpg)

Neste projeto, apresento detalhadamente uma análise exploratória e explanatória de um dataset do Brasileirão 2021, com o uso programático da linguagem Python em junção com a biblioteca Pandas para manipulação de dados, e também com o auxílio das bibliotecas Matplotlib e Seaborn para visualização gráfica dos dados informacionais. 

## Importação de Bibliotecas

```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

```

## Fonte de dados

O dataset [Campeonato Brasileiro de futebol](https://www.kaggle.com/datasets/adaoduque/campeonato-brasileiro-de-futebol) utilizado em tal análise exploratória está hospedado disponivelmente no Kaggle para uso gratuito.

## Importação do Dataset 

```
df = pd.read_csv('/content/drive/MyDrive/campeonato-brasileiro-full.csv')
```
## Processo de exploração dos dados

### (1) Tratamento dos dados


