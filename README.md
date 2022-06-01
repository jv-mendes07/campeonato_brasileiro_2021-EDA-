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
  
  **(1)** A coluna relativa às datas dos jogos foi conversível de tipo object (ou texto) para tipo datetime, que é o tipo de dados mais apropriado condizentemente à informações de datas e horários.
  
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
  
  **(2)** Mudamos o nome das colunas de 'mandante' e 'visitante' para 'time_mandante' e 'time_visitante' para deixar o título das colunas o mais específico e explícito possível para que a informação seja entendida.
  
* Criação de coluna:

  **(1)** Criamos uma coluna que some os dados numéricos da coluna de gols do time mandante em junção com os gols do time visitante, tal coluna criada expressa a quantidade de gols totais em cada partida do Brasileirão 2021.
  
  ```
  df_br_21['gols_na_partida'] = df_br_21['mandante_placar'] + df_br_21['visitante_placar']
  ```
  
* Formatação de colunas:
  
  **(1)** A coluna 'arena' foi formatada textualmente de letras minúsculas para o tipo textual de título com letras maiúsculas no início do nome de cada arena em que os jogos do Brasileirão foram ocorrentes em 2021.
    
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

Em suma, o gráfico de pizza expressa informacionalmente que do total de gols marcados em todos os Brasileirões de 2003 até 2021, aproximadamente 8% desses gols foram marcados nos anos de 2003, e de 100% do total de gols de todos os Brasileirões, 23% representa a quantidade de gols marcados nos anos entre 2003 e 2005, ou seja, quase 1 / 4 dos gols de todos os Brasileirões foi marcado em tal intervalo entre 2003 e 2005.

Após termos tal noção de quais foram às edições anuais do Brasileirão com mais gols contabilizados, podemos inicializar a análise restrita exclusivamente ao Brasileirão 2021.

#### **(2)** Quais foram os times com mais vitórias registradas no Brasileirão 2021?

Basicamente, antes de respondermos tal pergunta, poderemos classificar quais foram os times participantes do Brasileirão 2021 da série A:

```
'Atlético-MG',
 'Flamengo',
 'Palmeiras',
 'Fortaleza',
 'Fluminense',
 'Corinthians',
 'Bragantino',
 'Athletico-PR',
 'Atlético-GO',
 'América-MG',
 'Santos',
 'Internacional',
 'Grêmio',
 'Bahia',
 'Ceará',
 'Juventude',
 'São Paulo',
 'Cuiabá',
 'Sport',
 'Chapecoense'
 ```
Entre todos os times escritos acima, temos um gráfico de barras horizontais para expressar em ordem decrescente os times com mais vitórias até os times com menos vitórias contabilizadas em todo o Brasileirão 2021:

![](./img/A3.png)

Pelo gráfico de barras horizontais acima é observável conspicuamente que os cinco (ou seis) times que mais contabilizaram vitórias foram o Atlético Mineiro, Flamengo, Palmeiras, Fortaleza, e por fim Fluminense e Corinthians empatados.

Disto, poderemos ter dois gráficos de colunas para expressar ordenadamente os 4 times com mais vitórias contabilizadas em constraste aos times com menos vitórias contabilizadas em todo o campeonato.

![](./img/A4.png)

O time campeão do Brasileirão 2021 foi o Atlético Mineiro e intuitivamente é expectante que este tenha sido o time com mais vitórias no campeonato todo, o Flamengo foi o vice-campeão, o Palmeiras teve o término em terceiro lugar e o Fortaleza em quarto lugar na tabela do Brasileirão 2021.

Em outras palavras, como esperado os times com mais vitórias no campeonato foram os times que concluíram o campeonato no G4.

No entanto, inesperadamente os únicos times dos quatro times com menos vitórias que foram rebaixados para a série B foram o Sport e o Chapecoense.

Disto há como afirmarmos que caso um time seja um dos times com menos vitórias no campeonato brasileiro, isso não implica necessariamente que tal time será rebaixado, como neste caso o Grêmio teve mais vitórias do que o São Paulo e o Bahia teve mais vitórias que o Cuiabá no Brasileirão 2021. 

No entanto, o Grêmio e o Bahia foram rebaixados, enquanto o São Paulo e o Cuiabá, não.

À partir do insight de quais foram os times que terminaram no G4 e contabilizaram mais vitórias no campeonato, poderemos nos perguntar:

#### **(3)** Os times com mais vitórias no Brasileirão 2021, tiveram mais vitórias contabilizadas 'dentro' ou 'fora' de casa?

Primeiro, temos uma tabela para expressar a quantidade de vitórias contabilizadas dos times do G4 em jogos que tais times jogaram 'dentro' de casa: 

| index | time_mandante |     qtd_vitorias |
|-------|---------------|------------------|
| 3     | Atletico-MG   | 17               |
| 9     | Flamengo      | 13               |
| 10    | Fluminense    | 11               |
| 15    | Palmeiras     | 11               |
| 11    | Fortaleza     | 11               |
|       |               |                  |

Segundamente, temos uma tabela para expressar a quantidade de vitórias contabilizadas dos times do G4 em jogos que tais times jogaram 'fora' de casa:

|                 index | time_visitante |   qtd_vitorias |
|-----------------------|----------------|----------------|
| 3                     | Atlético-MG    | 9              |
| 16                    | Palmeiras      | 9              |
| 10                    | Flamengo       | 8              |
| 2                     | Atlético-GO    | 7              |
| 5                     | Bragantino     | 7              |
| 12                    | Fortaleza      | 6              |

Comparativamente, os times do G4 apresentam uma vantagem de vitórias ao jogarem 'dentro' de casa do que ao jogarem 'fora' de casa.

Como esperado o Atlético Mineiro foi o time com mais vitórias dentro e fora de casa no Brasileirão como um todo, o Palmeiras juntamente ao Atlético Mineiro foi um dos times com vitórias fora de casa também. 

Os times que terminaram no G4 da tabela do Brasileirão 2021 foram um dos times que mais contabilizaram vitórias 'dentro' e 'fora' de casa.

Entre os times com mais vitórias 'dentro' de casa que terminaram o campeonato fora do G4, temos somente o Fluminense, que foi o time que teve o término de campeonato na sétima posição do Brasileirão.

Já entre os times com mais vitórias 'fora' de casa que terminaram o campeonato fora do G4, temos o Bragantino e o Atlético-GO, em que o Bragantino teve um término de torneio próximo do G4 na sexta posição, e temos o Atlético-GO que teve o término na nona posição do campeonato.

Após respondermos a questão (2), poderemos continuar à explorar o motivo do porquê o São Paulo e o Cuiabá que foram um dos times com menos vitórias contabilizadas no Brasileirão, não foram rebaixados para à série B? Enquanto times como o Bahia e o Grêmio que foram times com mais vitórias em comparação, foram um dos quatro times rebaixados da série A para à série B.

Será que o São Paulo e o Cuiabá contabilizaram mais empates ao ponto de conseguirem superar em pontos o Bahia e o Grêmio? E será que o Bahia e o Grêmio foram um dos times com mais derrotas contabilizadas em comparação ao São Paulo e ao Cuiabá, e assim possibilitaram com que os dois times escapassem do rebaixamento pelos pontos obtidos por empates?

Bom, iremos responder tais questões nas próximas perguntas:

#### **(4)** Quais times do Brasileirão 2021 contabilizaram mais empates em todo o campeonato?
