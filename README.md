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

Duas bases de dados foram importadas, uma em relação aos jogos, times e placares dos jogos do Brasileirão registrados de 2003 até 2021, e a segunda base de dados é concernente às estatísticas gerais e técnicas de todos os jogos registrados em cada campeonato.

```
df = pd.read_csv('/content/drive/MyDrive/campeonato-brasileiro-full.csv')
```

```
df_2 = pd.read_csv('/content/drive/MyDrive/campeonato-brasileiro-estatisticas-full.csv', sep = ',')
```

## Colunas de cada dataset

Colunas do dataset de jogos do campeonato brasileiro:

```
'ID', 'rodada', 'data', 'hora', 'dia', 'mandante', 'visitante',
       'formacao_mandante', 'formacao_visitante', 'tecnico_mandante',
       'tecnico_visitante', 'vencedor', 'arena', 'mandante_placar',
       'visitante_placar', 'mandante_estado', 'visitante_estado',
       'estado_vencedor', 'ano_brasileirao', 'gols_na_partida'
```

Colunas do dataset de estatísticas de jogos do campeonato brasileiro:

```
'partida_id', 'rodada', 'clube', 'chutes', 'chutes_no_alvo',
       'posse_de_bola', 'passes', 'precisao_passes', 'faltas',
       'cartao_amarelo', 'cartao_vermelho', 'impedimentos', 'escanteios'
```
## Processo de exploração dos dados

### **(1)** Tratamento dos dados

* Tipo dos dados:
  
  **(1)** A coluna relativa às datas dos jogos foi conversível de tipo object (ou texto) para tipo datetime, que é o tipo de dados mais apropriados condizentemente à informações de datas e horários.
  
  **(2)** As colunas relativas às estatísticas do Brasileirão foram conversíveis de tipo object (ou texto) para tipo float, por serem colunas que informavam a porcentagem de posse de bola e precisão no passe dos times em cada jogo do campeonato, e como porcentagem é um tipo de dado numérico, então o tipo float é o tipo de dado mais adequado e correspondente com os dados de ambas às colunas.
  
* Filtragem dos dados:
  
  **(1)** A coluna de data dos jogos foi convertida de object para datetime para que pudessémos com mais facilidade, filtrarmos os jogos ocorrentes somente no Brasileirão 2021, porém havia jogos do ano de 2021 que eram jogos da edição passada (2020), então filtramos os jogos de 2021 à partir do mês de inicialização do Brasileirão 2021, ou seja, filtramos os dados à partir do mês de Maio de 2021.
  
  ```
  df_br_21 = df[(df['data'].dt.year == 2021) & (df['data'].dt.month.isin([5, 6, 7, 8, 9, 10, 11, 12]))]
  ```
 
  **(2)** No dataset de estatísticas filtramos os dados relativos ao Brasileirão de 2021 pela coluna partida_id, analisamos o intervalo de id's das partidas ocorrentes no Brasileirão 2021 para podermos ter somente os dados estatísticos relativos aos jogos do Brasileirão 2021.
  
  ```
  df_br_est_21 = df_2.loc[(df_2['partida_id'] >=  7266) & (df_2['partida_id'] <= 7645)]
  ```
  
* Renomeação de coluna:

  **(1)** Renomeamos as colunas de precisão do passe e posse de bola para inserirmos um sinal de porcentagem que indique explicitamente que tais colunas informam dados percentuais.
  
* Criação de coluna:

  **(1)** Criamos uma coluna que some os dados numéricos da coluna de gols do time mandante em junção com os gols do time visitante, tal coluna criada expressa a quantidade de gols totais em cada partida do Brasileirão 2021.
  
  ```
  df_br_21['gols_na_partida'] = df_br_21['mandante_placar'] + df_br_21['visitante_placar']
  ```
### **(2)** Conhecimento exploratório dos dados

Nesta fase, os dados do Brasileirão 2021 começaram à ser explorados para que novas informações fossem extraídas de tal base de dados, primeiramente aplicamos o método .cumsum() para obtermos a soma cumulativa de gols por partida no Brasileirão 2021, e por fim obtivemos o resultado 842, que em outras palavras significa que 842 gols foram marcados no Brasileirão 2021 como um todo.

À partir de tal informação, nos perguntamos:

#### **(1)** Quais foram às edições do campeonato brasileiro que tiveram mais gols contabilizados em todo o torneio?

Para responder tal questão, agrupamos os anos de cada Brasileirão e contabilizamos a quantidade de gols que foram marcados em cada edição anual do campeonato brasileiro, e assim obtivemos este resultado:

| index | ano  | qtd_de_gols |   |
|-------|------|-------------|---|
| 0     | 2003 | 1592        |   |
| 1     | 2004 | 1534        |   |
| 2     | 2005 | 1451        |   |
| 3     | 2006 | 1030        |   |
| 4     | 2007 | 1047        |   |
| 5     | 2008 | 1035        |   |
| 6     | 2009 | 1094        |   |
| 7     | 2010 | 978         |   |
| 8     | 2011 | 1017        |   |
| 9     | 2012 | 940         |   |
| 10    | 2013 | 936         |   |
| 11    | 2014 | 860         |   |
| 12    | 2015 | 897         |   |
| 13    | 2016 | 912         |   |
| 14    | 2017 | 923         |   |
| 15    | 2018 | 827         |   |
| 16    | 2019 | 876         |   |
| 17    | 2020 | 944         |   |
| 18    | 2021 | 842         |   |
|       |      |             |   |
 
A tabela acima está organizada em ordem crescente cronologicamente, isto é, do menor ano até o maior ano em que os gols totais de cada torneio foram ordenados.

Após isto, ordenamos às cinco edições do Brasileirão que mais gols foram marcados durante todo o campeonato:

|           index |              ano | total_de_gols |
|-----------------|------------------|---------------|
| 0               | 2003             | 1592          |
| 1               | 2004             | 1534          |
| 2               | 2005             | 1451          |
| 6               | 2009             | 1094          |
| 4               | 2007             | 1047          |

Curiosamente, os torneios da década de 2000 foram os anos em que o campeonato brasileiro teve edições com o maior número de gols marcados, 2003, 2004 e 2005 consecutivamente foram os anos com mais gols marcados em um campeonato brasileiro, à partir desses anos em diante o campeonato brasileiro teve gols marcados abaixo de tais edições.

Inversamente, ordenamos também as edições do Brasileirão que tiveram menos gols contabilizados em todo o campeonato:

|           index |                   ano | total_de_gols |
|-----------------|-----------------------|---------------|
| 15              | 2018                  | 827           |
| 18              | 2021                  | 842           |
| 11              | 2014                  | 860           |
| 16              | 2019                  | 876           |
| 12              | 2015                  | 897           |

Às edições mais recentes do Brasileirão de 2018, 2019 e 2021 são uma das edições registradas com o menor número de gols marcados em todo o campeonato.

Após tal exposição, poderemos apresentar um gráfico de colunas para representar a quantidade de gols marcados em cada edição anual do Brasileirão:

![](./img/A1.png)

Um gráfico de pizza seria útil para representar proporcionalmente a quantidade de gols marcados em cada ano do Brasileirão em relação a quantidade de gols marcados durante todos os Brasileirões de 2003 até 2021.

![](./img/A2.png)

Em suma, o gráfico de pizza expressa informacionalmente que do total de gols marcados em todos os Brasileirões de 2003 até 2021, aproximadamente entre 7 e 8% desses gols foram marcados entre os anos de 2003 e 2005, e de 100% do total de gols de todos os Brasileirões, 23% representa a quantidade de gols marcados nos anos de 2003 até 2005, ou seja, quase 1 / 4 dos gols de todos os Brasileirões foi marcado em tal intervalo entre 2003 e 2005.



