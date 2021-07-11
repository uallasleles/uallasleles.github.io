---
title       : "Early Stage Diabetes Risk Prediction"
name        : Early Stage Diabetes Risk Prediction
author      : "<i>Author: Uallas Leles</i>"
date        : "<i>Date: 26 de junho, 2021</i>"
tools       : [R, RStudio, Markdown]
image       :
description : "Prevendo o risco de diabetes no estágio inicial."
---

Early Stage Diabetes Risk Prediction
================
<i>Author: Uallas Leles</i>
<i>Date: 26 de junho, 2021</i>

------------------------------------------------------------------------

<br> **Prevendo o risco de diabetes no estágio inicial:**<br>

> Este projeto usa técnicas de mineração de dados e de machine learning
> para obter uma probabilidade que indique se uma pessoa tem determinada
> doença.

<br>

***Definição do Problema:**<br> Dado o conjunto de parâmetros, podemos
prever uma pessoa com diabetes em estágio inicial.*<br>

***Fonte dos dados:** <br> Estes dados foram coletados por meio de
questionários diretos de pacientes do Hospital de Diabetes Sylhet em
Sylhet, Bangladesh, e aprovado por um médico.<br> Link: [UCI: Early
stage diabetes risk prediction
dataset.](https://archive.ics.uci.edu/ml/datasets/Early+stage+diabetes+risk+prediction+dataset.)*

------------------------------------------------------------------------

### Importando os Pacotes

``` r
require(moments) # Coeficiente de Assimetria

#install.packages("rvest")
require(rvest) # %>% - Concatenação no uso de funções

# install.packages("fdth")
require(fdth) # Execute tabelas de distribuição de frequência, histogramas e polígonos associados de objetos vetoriais, data.frame e matriz para variáveis numéricas e categóricas.

# install.packages("tidyverse")
require(tidyverse) # Para função tibble()

# install.packages("gmodels")
require(gmodels)

# install.packages("mice")
require(mice)

# Scatterplot Matrix
# install.packages("psych")
require(psych)

# install.packages('stargazer')
require(stargazer)

# Gerando uma curva ROC em R
# install.packages("ROCR")
require(ROCR)

# install.packages("e1071")
require(e1071)

# Gerando Confusion Matrix com o Caret
# install.packages("caret")
require(caret)

# install.packages("Amelia")
# require(Amelia) # missmap - Mapa de valores missing

require(xtable)
# options(xtable.floating = FALSE)
# options(xtable.timestamp = "")

#install.packages("DT")
require(DT)

require(knitr)

# require(lemon)
# knit_print.data.frame <- lemon_print

#install.packages("ggplot2")
require(ggplot2)

#install.packages("dplyr")
require(dplyr)

#install.packages("hrbrthemes")
require(hrbrthemes)

# install.packages("gridExtra")
require(gridExtra)

# install.packages('corrplot')
library(corrplot)

#install.packages("skimr")
library(skimr)
```

### Definindo Variáveis

``` r
DATADIC = 'data/dictionary.txt'
```

### Criando Funções

``` r
# Função para cálculo da moda.
moda <- function(dados){
  vetor = table(as.vector(dados))
  m = names(vetor)[vetor == max(vetor)]
  return(m)}
```

### Obtendo os Dados

``` r
df <- read.csv(
  file = 'data/diabetes.csv', 
  stringsAsFactors = TRUE)
```

Pré-visualização

``` r
df %>% head(1) %>% kable()
```

| Age | Gender | Polyuria | Polydipsia | sudden.weight.loss | weakness | Polyphagia | Genital.thrush | visual.blurring | Itching | Irritability | delayed.healing | partial.paresis | muscle.stiffness | Alopecia | Obesity | class    |
|----:|:-------|:---------|:-----------|:-------------------|:---------|:-----------|:---------------|:----------------|:--------|:-------------|:----------------|:----------------|:-----------------|:---------|:--------|:---------|
|  40 | Male   | No       | Yes        | No                 | Yes      | No         | No             | No              | Yes     | No           | Yes             | No              | Yes              | Yes      | Yes     | Positive |

``` r
target_names <- c('class')
feature_names = names(df[!names(df) %in% target_names])
```

### Dicionário de Atributos

``` r
# Read a txt file
read.delim(DATADIC, sep='|')
```

    ##              Attribute              Information
    ## 1                 Age                   1.20-65
    ## 2                 Sex         1. Male, 2.Female
    ## 3            Polyuria              1.Yes, 2.No.
    ## 4          Polydipsia              1.Yes, 2.No.
    ## 5  sudden weight loss              1.Yes, 2.No.
    ## 6            weakness              1.Yes, 2.No.
    ## 7          Polyphagia              1.Yes, 2.No.
    ## 8      Genital thrush              1.Yes, 2.No.
    ## 9     visual blurring              1.Yes, 2.No.
    ## 10            Itching              1.Yes, 2.No.
    ## 11       Irritability              1.Yes, 2.No.
    ## 12    delayed healing              1.Yes, 2.No.
    ## 13    partial paresis              1.Yes, 2.No.
    ## 14   muscle stiffness              1.Yes, 2.No.
    ## 15           Alopecia              1.Yes, 2.No.
    ## 16            Obesity              1.Yes, 2.No.
    ## 17              Class   1.Positive, 2.Negative.

### Resumo do Dataset

``` r
skim(df)
```

|                                                  |      |
|:-------------------------------------------------|:-----|
| Name                                             | df   |
| Number of rows                                   | 520  |
| Number of columns                                | 17   |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |      |
| Column type frequency:                           |      |
| factor                                           | 16   |
| numeric                                          | 1    |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |      |
| Group variables                                  | None |

Data summary

**Variable type: factor**

| skim\_variable     | n\_missing | complete\_rate | ordered | n\_unique | top\_counts        |
|:-------------------|-----------:|---------------:|:--------|----------:|:-------------------|
| Gender             |          0 |              1 | FALSE   |         2 | Mal: 328, Fem: 192 |
| Polyuria           |          0 |              1 | FALSE   |         2 | No: 262, Yes: 258  |
| Polydipsia         |          0 |              1 | FALSE   |         2 | No: 287, Yes: 233  |
| sudden.weight.loss |          0 |              1 | FALSE   |         2 | No: 303, Yes: 217  |
| weakness           |          0 |              1 | FALSE   |         2 | Yes: 305, No: 215  |
| Polyphagia         |          0 |              1 | FALSE   |         2 | No: 283, Yes: 237  |
| Genital.thrush     |          0 |              1 | FALSE   |         2 | No: 404, Yes: 116  |
| visual.blurring    |          0 |              1 | FALSE   |         2 | No: 287, Yes: 233  |
| Itching            |          0 |              1 | FALSE   |         2 | No: 267, Yes: 253  |
| Irritability       |          0 |              1 | FALSE   |         2 | No: 394, Yes: 126  |
| delayed.healing    |          0 |              1 | FALSE   |         2 | No: 281, Yes: 239  |
| partial.paresis    |          0 |              1 | FALSE   |         2 | No: 296, Yes: 224  |
| muscle.stiffness   |          0 |              1 | FALSE   |         2 | No: 325, Yes: 195  |
| Alopecia           |          0 |              1 | FALSE   |         2 | No: 341, Yes: 179  |
| Obesity            |          0 |              1 | FALSE   |         2 | No: 432, Yes: 88   |
| class              |          0 |              1 | FALSE   |         2 | Pos: 320, Neg: 200 |

**Variable type: numeric**

| skim\_variable | n\_missing | complete\_rate |  mean |    sd |  p0 | p25 |  p50 | p75 | p100 | hist  |
|:---------------|-----------:|---------------:|------:|------:|----:|----:|-----:|----:|-----:|:------|
| Age            |          0 |              1 | 48.03 | 12.15 |  16 |  39 | 47.5 |  57 |   90 | ▂▇▇▃▁ |

``` r
# Exporta os dados
dput(df, file = "data/df.R")
```

## Análise Exploratória

### Classificando as Variáveis

Uma boa forma de iniciar uma análise descritiva adequada é verificar os
tipos de variáveis disponíveis.

``` r
# Qualitativas Nominais
var.fct <- c()
for(i in 1:ncol(df)){
  if(is.factor(df[,i]) && !is.ordered(df[,i])){
    var.fct = c(var.fct, names(df)[i])}}

# Qualitativas Ordinais
var.fct_ord <- c()
for(i in 1:ncol(df)){
  if(is.factor(df[,i]) && is.ordered(df[,i])){
    var.fct_ord = c(var.fct_ord, names(df)[i])}}

# Quantitativas Discretas
var.num <- c()
for(i in 1:ncol(df)){
  if(is.integer(df[,i])){
    var.num <- c(var.num, names(df)[i])}}

# Quantitativas Contínuas
var.num_con <- c()
for(i in 1:ncol(df)){
  if(typeof(df[,i]) == "double"){
    var.num_con <- c(var.num_con, names(df)[i])}}

d1 = data.frame("Qualitativas_Nominais - " = var.fct)
d2 = data.frame("Qualitativas_Ordinais - " = var.fct_ord)
d3 = data.frame("Quantitativas_Discretas - " = var.num)
d4 = data.frame("Quantitativas_Contínuas - " = var.num_con)
var_type_list = list(d1, d2, d3, d4)

for(i in var_type_list){
  if(dim(i)[1] != 0){
    print(i)}}
```

    ##    Qualitativas_Nominais...
    ## 1                    Gender
    ## 2                  Polyuria
    ## 3                Polydipsia
    ## 4        sudden.weight.loss
    ## 5                  weakness
    ## 6                Polyphagia
    ## 7            Genital.thrush
    ## 8           visual.blurring
    ## 9                   Itching
    ## 10             Irritability
    ## 11          delayed.healing
    ## 12          partial.paresis
    ## 13         muscle.stiffness
    ## 14                 Alopecia
    ## 15                  Obesity
    ## 16                    class
    ##   Quantitativas_Discretas...
    ## 1                        Age

### Análise Univariada

``` r
describe(df) %>% kable()
```

|                      | vars |   n |      mean |         sd | median |   trimmed |     mad | min | max | range |       skew |   kurtosis |        se |
|:---------------------|-----:|----:|----------:|-----------:|-------:|----------:|--------:|----:|----:|------:|-----------:|-----------:|----------:|
| Age                  |    1 | 520 | 48.028846 | 12.1514660 |   47.5 | 47.649039 | 13.3434 |  16 |  90 |    74 |  0.3274616 | -0.2121409 | 0.5328770 |
| Gender\*             |    2 | 520 |  1.630769 |  0.4830612 |    2.0 |  1.663461 |  0.0000 |   1 |   2 |     1 | -0.5403777 | -1.7112718 | 0.0211836 |
| Polyuria\*           |    3 | 520 |  1.496154 |  0.5004667 |    1.0 |  1.495192 |  0.0000 |   1 |   2 |     1 |  0.0153407 | -2.0036067 | 0.0219469 |
| Polydipsia\*         |    4 | 520 |  1.448077 |  0.4977755 |    1.0 |  1.435096 |  0.0000 |   1 |   2 |     1 |  0.2082192 | -1.9604037 | 0.0218289 |
| sudden.weight.loss\* |    5 | 520 |  1.417308 |  0.4935894 |    1.0 |  1.396635 |  0.0000 |   1 |   2 |     1 |  0.3344208 | -1.8917897 | 0.0216453 |
| weakness\*           |    6 | 520 |  1.586539 |  0.4929284 |    2.0 |  1.608173 |  0.0000 |   1 |   2 |     1 | -0.3504446 | -1.8807944 | 0.0216163 |
| Polyphagia\*         |    7 | 520 |  1.455769 |  0.4985194 |    1.0 |  1.444711 |  0.0000 |   1 |   2 |     1 |  0.1771073 | -1.9724150 | 0.0218615 |
| Genital.thrush\*     |    8 | 520 |  1.223077 |  0.4167104 |    1.0 |  1.153846 |  0.0000 |   1 |   2 |     1 |  1.3265354 | -0.2407558 | 0.0182740 |
| visual.blurring\*    |    9 | 520 |  1.448077 |  0.4977755 |    1.0 |  1.435096 |  0.0000 |   1 |   2 |     1 |  0.2082192 | -1.9604037 | 0.0218289 |
| Itching\*            |   10 | 520 |  1.486538 |  0.5003000 |    1.0 |  1.483173 |  0.0000 |   1 |   2 |     1 |  0.0537104 | -2.0009521 | 0.0219396 |
| Irritability\*       |   11 | 520 |  1.242308 |  0.4288921 |    1.0 |  1.177885 |  0.0000 |   1 |   2 |     1 |  1.1993541 | -0.5626206 | 0.0188082 |
| delayed.healing\*    |   12 | 520 |  1.459615 |  0.4988463 |    1.0 |  1.449519 |  0.0000 |   1 |   2 |     1 |  0.1616007 | -1.9776774 | 0.0218759 |
| partial.paresis\*    |   13 | 520 |  1.430769 |  0.4956607 |    1.0 |  1.413461 |  0.0000 |   1 |   2 |     1 |  0.2788102 | -1.9259576 | 0.0217362 |
| muscle.stiffness\*   |   14 | 520 |  1.375000 |  0.4845891 |    1.0 |  1.343750 |  0.0000 |   1 |   2 |     1 |  0.5149089 | -1.7382004 | 0.0212506 |
| Alopecia\*           |   15 | 520 |  1.344231 |  0.4755743 |    1.0 |  1.305289 |  0.0000 |   1 |   2 |     1 |  0.6538187 | -1.5755399 | 0.0208553 |
| Obesity\*            |   16 | 520 |  1.169231 |  0.3753167 |    1.0 |  1.086539 |  0.0000 |   1 |   2 |     1 |  1.7592245 |  1.0969914 | 0.0164587 |
| class\*              |   17 | 520 |  1.615385 |  0.4869727 |    2.0 |  1.644231 |  0.0000 |   1 |   2 |     1 | -0.4729740 | -1.7797070 | 0.0213552 |

### Distribuições de Frequências

A primeira tarefa de uma análise estatística de um conjunto de dados
consiste em resumí-los. As técnicas disponíveis para essa finalidade
dependem dos tipos de variáveis envolvidas.

#### Variáveis Qualitativas

As distribuições para *Variáveis Qualitativas* podem ser representadas
por meio de:

-   Gráfico de Barras
-   Gráfico do tipo Pizza

> 1.  **Distribuições de Frequências para as Variáveis Qualitativas
>     Nominais**

``` r
# Distribuição de Frequências para as Variáveis Qualitativas Nominais
for(i in var.fct){
  fdt_cat(df[,i]) %>% 
    tibble() -> df_var
  
  names(df_var)[1] = c(colnames(df[i]))
  print(kable(df_var[order(df_var[2], decreasing = TRUE),]))
}
```

    ## 
    ## 
    ## |Gender |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:------|---:|---------:|--------:|---:|---------:|
    ## |Male   | 328| 0.6307692| 63.07692| 328|  63.07692|
    ## |Female | 192| 0.3692308| 36.92308| 520| 100.00000|
    ## 
    ## 
    ## |Polyuria |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:--------|---:|---------:|--------:|---:|---------:|
    ## |No       | 262| 0.5038462| 50.38462| 262|  50.38462|
    ## |Yes      | 258| 0.4961538| 49.61538| 520| 100.00000|
    ## 
    ## 
    ## |Polydipsia |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:----------|---:|---------:|--------:|---:|---------:|
    ## |No         | 287| 0.5519231| 55.19231| 287|  55.19231|
    ## |Yes        | 233| 0.4480769| 44.80769| 520| 100.00000|
    ## 
    ## 
    ## |sudden.weight.loss |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:------------------|---:|---------:|--------:|---:|---------:|
    ## |No                 | 303| 0.5826923| 58.26923| 303|  58.26923|
    ## |Yes                | 217| 0.4173077| 41.73077| 520| 100.00000|
    ## 
    ## 
    ## |weakness |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:--------|---:|---------:|--------:|---:|---------:|
    ## |Yes      | 305| 0.5865385| 58.65385| 305|  58.65385|
    ## |No       | 215| 0.4134615| 41.34615| 520| 100.00000|
    ## 
    ## 
    ## |Polyphagia |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:----------|---:|---------:|--------:|---:|---------:|
    ## |No         | 283| 0.5442308| 54.42308| 283|  54.42308|
    ## |Yes        | 237| 0.4557692| 45.57692| 520| 100.00000|
    ## 
    ## 
    ## |Genital.thrush |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:--------------|---:|---------:|--------:|---:|---------:|
    ## |No             | 404| 0.7769231| 77.69231| 404|  77.69231|
    ## |Yes            | 116| 0.2230769| 22.30769| 520| 100.00000|
    ## 
    ## 
    ## |visual.blurring |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:---------------|---:|---------:|--------:|---:|---------:|
    ## |No              | 287| 0.5519231| 55.19231| 287|  55.19231|
    ## |Yes             | 233| 0.4480769| 44.80769| 520| 100.00000|
    ## 
    ## 
    ## |Itching |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:-------|---:|---------:|--------:|---:|---------:|
    ## |No      | 267| 0.5134615| 51.34615| 267|  51.34615|
    ## |Yes     | 253| 0.4865385| 48.65385| 520| 100.00000|
    ## 
    ## 
    ## |Irritability |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:------------|---:|---------:|--------:|---:|---------:|
    ## |No           | 394| 0.7576923| 75.76923| 394|  75.76923|
    ## |Yes          | 126| 0.2423077| 24.23077| 520| 100.00000|
    ## 
    ## 
    ## |delayed.healing |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:---------------|---:|---------:|--------:|---:|---------:|
    ## |No              | 281| 0.5403846| 54.03846| 281|  54.03846|
    ## |Yes             | 239| 0.4596154| 45.96154| 520| 100.00000|
    ## 
    ## 
    ## |partial.paresis |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:---------------|---:|---------:|--------:|---:|---------:|
    ## |No              | 296| 0.5692308| 56.92308| 296|  56.92308|
    ## |Yes             | 224| 0.4307692| 43.07692| 520| 100.00000|
    ## 
    ## 
    ## |muscle.stiffness |   f|    rf| rf(%)|  cf| cf(%)|
    ## |:----------------|---:|-----:|-----:|---:|-----:|
    ## |No               | 325| 0.625|  62.5| 325|  62.5|
    ## |Yes              | 195| 0.375|  37.5| 520| 100.0|
    ## 
    ## 
    ## |Alopecia |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:--------|---:|---------:|--------:|---:|---------:|
    ## |No       | 341| 0.6557692| 65.57692| 341|  65.57692|
    ## |Yes      | 179| 0.3442308| 34.42308| 520| 100.00000|
    ## 
    ## 
    ## |Obesity |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:-------|---:|---------:|--------:|---:|---------:|
    ## |No      | 432| 0.8307692| 83.07692| 432|  83.07692|
    ## |Yes     |  88| 0.1692308| 16.92308| 520| 100.00000|
    ## 
    ## 
    ## |class    |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:--------|---:|---------:|--------:|---:|---------:|
    ## |Positive | 320| 0.6153846| 61.53846| 320|  61.53846|
    ## |Negative | 200| 0.3846154| 38.46154| 520| 100.00000|

> 2.  **Gráfico de Barras: Variáveis Qualitativas**

``` r
# Gráficos de barras para variáveis categóricas
par(mfrow=c(2,2), cex = 0.55)
for(i in c(var.fct, var.fct_ord)){
  bp <- barplot(table(df[i]), 
                main = names(df[i]),
                ylim = c(0, max(table(df[i]))*1.4),
                col = "green")
  text(x = as.vector(bp),
       y = table(df[,i]) + 2,
       label =  round(table(df[i]), 1), 
       pos = 3,
       col = "black")}
```

![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Barras:%20Variáveis%20Categóricas-1.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Barras:%20Variáveis%20Categóricas-2.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Barras:%20Variáveis%20Categóricas-3.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Barras:%20Variáveis%20Categóricas-4.png)<!-- -->

#### Variáveis Quantitativas

-   Discretas
-   Contínuas

> Agrupam-se os valores das variáveis em classes e obtêm-se as
> frequências em cada classe.

As distribuições para *Variáveis Quantitativas* podem ser representadas
por meio de:

-   Gráfico de Dispersão Unidimensional (dotplot)
-   Gráfico Ramo e Folhas (Steam and Leaf)
-   Histograma

> 3.  **Distribuição de Frequências: Variáveis Quantitativas**

``` r
par(mfrow=c(2,1))
# Distribuição de Frequências: Variáveis Quantitativas
for(i in var.num_con){
        df_n <- fdt(df[,i])
        #names(df)[[1]] = c(colnames(aed.trn[i]))
        print(kable(df_n, caption = "Title of the table"))
}
```

``` r
# Build dataset with different distributions

# Represent it
df %>%
  ggplot(aes(x = Age, fill=Gender)) +
  geom_histogram(
    color="#e9ecef", 
    alpha=0.6, 
    position = 'identity', 
    binwidth = 5)
```

![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

``` r
df %>%
  ggplot( aes(x=Age, fill=Gender) ) +
    geom_histogram( color="#e9ecef", alpha=0.6, position = 'identity') +
    scale_fill_manual( values=c("#69b3a2", "#404080") ) +
    labs(fill="Sexo")
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

### Medidas de Posição

São as estatísticas que representam uma série de dados orientando-nos
quanto à posição da distribuição em relação ao eixo horizontal (eixo
“x”) do gráfico da curva de freqüência. As medidas de posição mais
importantes são as medidas de tendência central, no qual se verifica uma
tendência dos dados observados a se agruparem em torno dos valores
centrais.

``` r
  stargazer(df, median = T, mean.sd = T, iqr = T, type = "text", title = "Sumário Estatístico")
```

    ## 
    ## Sumário Estatístico
    ## ==============================================================
    ## Statistic  N   Mean  St. Dev. Min Pctl(25) Median Pctl(75) Max
    ## --------------------------------------------------------------
    ## Age       520 48.029  12.151  16     39     47.5     57    90 
    ## --------------------------------------------------------------

***Moda***

Moda - A moda é especialmente útil para dados qualitativos. Não é
possível analisar a média ou mediana de dados não ordenados, como cidade
ou preferência musical. Então a moda entra em ação.

``` r
n = c()
m = c()
# Moda
for(i in 1:ncol(df)){
    if(is.factor(df[,i])){
        n = c(n, names(df)[i])
        m = c(m, moda(df[,i]))
    }
}
kable(data.frame("Variável" = n, "Moda" = m), align = "l")
```

| Variável           | Moda     |
|:-------------------|:---------|
| Gender             | Male     |
| Polyuria           | No       |
| Polydipsia         | No       |
| sudden.weight.loss | No       |
| weakness           | Yes      |
| Polyphagia         | No       |
| Genital.thrush     | No       |
| visual.blurring    | No       |
| Itching            | No       |
| Irritability       | No       |
| delayed.healing    | No       |
| partial.paresis    | No       |
| muscle.stiffness   | No       |
| Alopecia           | No       |
| Obesity            | No       |
| class              | Positive |

### Medidas Separatrizes

> **Quintis**

``` r
# Quintis
par(mfrow=c(2,2))
for(i in 1:ncol(df)){
    if(is.numeric(df[,i])){
        quintis = seq(.2, .8, .2)
        q <- data.frame(quantile(df[,i], quintis))
        names(q) <- names(df)[i]
        print(q)
    }
}
```

    ##     Age
    ## 20%  37
    ## 40%  44
    ## 60%  50
    ## 80%  58

> **Boxplot**

O boxplot é um gráfico baseado nos quantis que serve como alternativa ao
histograma para resumir a distribuição das dados. Esse gráfico permite
que identifiquemos a posição dos 50% centrais dos dados (entre o
primeiro e terceiro quartis), a posição da mediana, os valores atípicos,
se existirem, assim como permite uma avaliação da simetria da
distribuição. Boxplots são úteis para a comparação de vários conjuntos
de dados.

``` r
# libraries
#install.packages("gridExtra")
library(ggplot2)
library(gridExtra)
 
# Make 3 simple graphics:
g1 <- ggplot(df, aes(x=Age)) + geom_density(fill="slateblue")
g2 <- ggplot(df, aes(x=Age, y=class, color=class)) + geom_point(size=5) + theme(legend.position="none")
g3 <- ggplot(df, aes(x=factor(Gender), y=Age, fill=class)) + geom_boxplot() + theme(legend.position="none")
g4 <- ggplot(df , aes(x=factor(Gender), fill=factor(Gender))) +  geom_bar()
 
# Plots
grid.arrange(g2, arrangeGrob(g3, g4, ncol=2), nrow = 2)
```

![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Boxplot-1.png)<!-- -->

``` r
grid.arrange(g1, g2, g3, nrow = 3)
```

![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Boxplot-2.png)<!-- -->

``` r
grid.arrange(g2, arrangeGrob(g3, g4, ncol=2), nrow = 1)
```

![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Boxplot-3.png)<!-- -->

``` r
grid.arrange(g2, arrangeGrob(g3, g4, nrow=2), nrow = 1)
```

![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Boxplot-4.png)<!-- -->
\#\#\# Medidas de Dispersão

As medidas de dispersão são a amplitude total, a variância, o
desvio-padrão e o coeficiente de variação.

------------------------------------------------------------------------

> **Amplitude Total**

É a diferença entre o maior e menor dos valores da série. A utilização
da amplitude total como medida de dispersão é muito limitada, pois é uma
medida que depende apenas dos valores extremos, não sendo afetada pela
variabilidade interna dos valores da série.

``` r
# Amplitude Total
for(i in 1:ncol(df)){
    if(is.numeric(df[,i])){
      amp <- diff(range(df[,i]))
      print(amp)
    }
}
```

    ## [1] 74

> **Variância**

    A variância é a medida de dispersão mais empregada geralmente, pois leva em consideração a totalidade dos valores da variável em estudo. Baseia-se nos desvios em torno da média aritmética, sendo um indicador de variabilidade.

    Para medir o grau de variabilidade dos valores em torno da média, nada mais interessante do que estudarmos o comportamento dos desvios de cada valor individual da série em relação à média.

    Queremos calcular a média dos desvios, porém sua soma pode ser nula. Como solução a esse problema a variância considera o quadrado de cada desvio evitando com isso que o somatório seja nulo.

``` r
# Variância
for(i in 1:ncol(df)){
    if(is.numeric(df[,i])){
        print(
            var(df[,i])
            )
    }
}
```

    ## [1] 147.6581

> **Desvio Padrão**

    Seguindo a mesma linha de raciocínio usado para o cálculo da variância, necessitamos, agora, aproximar a medida de dispersão da variável original. Para isso, calculamos o desvio padrão, que é a raiz quadrada da variância.

    Podemos representar o desvio padrão por uma distribuição normal:

    - 68,26% das ocorrências se concentrarão na área do gráfico demarcada por um
    desvio padrão à direita e um desvio padrão à esquerda da linha média;

    - 95,44% das ocorrências estão a dois desvios padrão, para a direita e a esquerda da média e, finalmente;

    - 99,72% das ocorrências ocorrem a três desvios padrão ao redor da média aritmética.

``` r
# Desvio Padrão
for(i in 1:ncol(df)){
    if(is.numeric(df[,i])){
        print(
            sd(df[,i])
        )
    }
}
```

    ## [1] 12.15147

-   **Coeficiente de Variação**

    Trata-se de uma medida relativa de dispersão, útil para a comparação
    em termos relativos do grau de concentração em torno da média de
    séries distintas.

    A importância de se estudar o coeficiente de variação se dá, pois o
    desvio-padrão é relativo à média. E como duas distribuições podem
    ter médias diferentes, o desvio destas distribuições não é
    comparável. Logo, o coeficiente de variação é muito utilizado para
    comparação entre amostras.

``` r
# Coeficiente de Variação
for(i in 1:ncol(df)){
    if(is.numeric(df[,i])){
        cv <- 100*sd(df[,i]/mean(df[,i]))
        print(cv)
    }
}
```

    ## [1] 25.30035

### Medidas de Assimetria e Achatamento

-   Assimetria

    Duas distribuições podem se diferenciar uma da outra em termos de
    assimetria ou achatamento, ou de ambas. A assimetria e o achatamento
    têm importância devido a hipótese de que populações são distribuídas
    normalmente.

    A assimetria de Pearson, é baseada nas relações entre a média,
    mediana e moda. Essas três medidas são idênticas em valor para uma
    distribuição unimodas simétria, mas, para uma distribuição
    assimétrica, a média distancia-se da moda situando-se a mediana em
    uma posição intermediária, à medida que aumenta a assimetria da
    distribuição.

    Consequentemente, a distância entre a média e a moda poderia ser
    usada para medir a assimetria.

-   Achatamento

    Curtose é o grau de achatamento em uma distribuição de frequência
    que têm apenas uma moda, ou seja, uma unimodal, em relação à normal.
    Mede o agrupamento de valores em torno do centro. Quanto maior esse
    agrupamento, maior será o valor da curtose.

    Os tipos de curtose são:

    -   Leptocúrtica

    -   Mesocúrtica

    -   Platicúrtica

    A curtose analisa a curva de frequência de forma vertical,
    relacionando a sua característica com a característica de uma
    distribuição normal.

``` r
par(mfrow=c(2,2))
for(i in 1:ncol(df)){
    if(is.numeric(df[,i])){
        
        datasim <- data.frame(df[,i])
        g <- ggplot(datasim, aes(x = df[,i]), binwidth = 2) + 
          geom_histogram(aes(y = ..density..), fill = 'red', alpha = 0.5) + 
          geom_density(colour = 'blue') + xlab(expression(bold('Dados'))) + 
          ylab(expression(bold('Densidade')))
        
        print(g)
    }
}
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Histograma-1.png)<!-- -->

``` r
# Coeficiente de Assimetria (Skew)
for(i in 1:ncol(df)){
    if(is.numeric(df[,i])){
        ca <- skewness(df[,i])
        print(ca)
    }
}
```

    ## [1] 0.3274616

``` r
# Coeficiente de Curtose
for(i in 1:ncol(df)){
    if(is.numeric(df[,i])){
        ck <- kurtosis(df[,i])
        print(ck)
    }
}
```

    ## [1] -0.2121409

## Análise Bivariada

O objetivo principal das análises nessa situação é ***explorar
relações*** (similaridades) ***entre as colunas***, ou algumas vezes
***entre as linhas*** através da ***distribuição conjunta das
frequências***.

Em algumas situações, podem ter dois ou mais conjuntos de dados
provenientes da observação da mesma variável.

-   Quando se considera duas variáveis ou dois conjuntos de dados,
    existem três situações:

    -   As duas variáveis são qualitativas;

    -   As duas variáveis são quantitativas; e

    -   Uma variável é qualitativa e a outra é quantitativa.

### Duas Categóricas

Quando as variáveis são qualitativas, os dados são resumidos em
***tabelas de dupla entrada*** (ou de ***contingência***), onde
aparecerão as frequências absolutas. Pode-se ainda, adicionar o total
por linha e o total por coluna. As distribuições assim obtidas são
chamadas tecnicamente de ***distribuições marginais***.

Em vez de se trabalhar com frequências absolutas, constrói-se tabelas
com frequências relativas. Porém, existem três possibilidades de se
expressar as frequências relativas de cada casela (célula):

1.  Em relação ao total geral;

2.  Em relação ao total de cada linha; e,

3.  Em relação ao total de cada coluna.

#### Tabelas de Contingência

A função *CrossTable* implementa uma tabela de tabulação cruzada, com
testes de qui-quadrado, Fisher e McNemar de independência de todos os
fatores da tabela. Seu resultado já apresenta as três possibilidades de
frequências relativas.

***Qui Quadrado*** é um teste de hipótese que encontra um ***valor de
dispersão para duas variáveis nominais***. Compara proporções,
divergências entre frequências observadas e esperadas para um certo
evento.

``` r
par(mfrow=c(1,2))
for(i in var.fct[-1]){
  CrossTable(df$class, 
             df[,i],
             prop.t = T,
             chisq = T,
             digits = 2,
             dnn = c("class", i))
  }
```

    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## | Chi-square contribution |
    ## |           N / Row Total |
    ## |           N / Col Total |
    ## |         N / Table Total |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  520 
    ## 
    ##  
    ##              | Polyuria 
    ##        class |        No |       Yes | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Negative |       185 |        15 |       200 | 
    ##              |     70.41 |     71.50 |           | 
    ##              |      0.92 |      0.07 |      0.38 | 
    ##              |      0.71 |      0.06 |           | 
    ##              |      0.36 |      0.03 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Positive |        77 |       243 |       320 | 
    ##              |     44.00 |     44.69 |           | 
    ##              |      0.24 |      0.76 |      0.62 | 
    ##              |      0.29 |      0.94 |           | 
    ##              |      0.15 |      0.47 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       262 |       258 |       520 | 
    ##              |      0.50 |      0.50 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  230.5954     d.f. =  1     p =  4.42081e-52 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  227.8658     d.f. =  1     p =  1.740912e-51 
    ## 
    ##  
    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## | Chi-square contribution |
    ## |           N / Row Total |
    ## |           N / Col Total |
    ## |         N / Table Total |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  520 
    ## 
    ##  
    ##              | Polydipsia 
    ##        class |        No |       Yes | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Negative |       192 |         8 |       200 | 
    ##              |     60.34 |     74.33 |           | 
    ##              |      0.96 |      0.04 |      0.38 | 
    ##              |      0.67 |      0.03 |           | 
    ##              |      0.37 |      0.02 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Positive |        95 |       225 |       320 | 
    ##              |     37.72 |     46.46 |           | 
    ##              |      0.30 |      0.70 |      0.62 | 
    ##              |      0.33 |      0.97 |           | 
    ##              |      0.18 |      0.43 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       287 |       233 |       520 | 
    ##              |      0.55 |      0.45 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  218.8448     d.f. =  1     p =  1.615687e-49 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  216.1716     d.f. =  1     p =  6.18701e-49 
    ## 
    ##  
    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## | Chi-square contribution |
    ## |           N / Row Total |
    ## |           N / Col Total |
    ## |         N / Table Total |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  520 
    ## 
    ##  
    ##              | sudden.weight.loss 
    ##        class |        No |       Yes | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Negative |       171 |        29 |       200 | 
    ##              |     25.45 |     35.54 |           | 
    ##              |      0.85 |      0.14 |      0.38 | 
    ##              |      0.56 |      0.13 |           | 
    ##              |      0.33 |      0.06 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Positive |       132 |       188 |       320 | 
    ##              |     15.91 |     22.21 |           | 
    ##              |      0.41 |      0.59 |      0.62 | 
    ##              |      0.44 |      0.87 |           | 
    ##              |      0.25 |      0.36 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       303 |       217 |       520 | 
    ##              |      0.58 |      0.42 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  99.10772     d.f. =  1     p =  2.391337e-23 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  97.2963     d.f. =  1     p =  5.969166e-23 
    ## 
    ##  
    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## | Chi-square contribution |
    ## |           N / Row Total |
    ## |           N / Col Total |
    ## |         N / Table Total |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  520 
    ## 
    ##  
    ##              | weakness 
    ##        class |        No |       Yes | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Negative |       113 |        87 |       200 | 
    ##              |     11.11 |      7.83 |           | 
    ##              |      0.56 |      0.43 |      0.38 | 
    ##              |      0.53 |      0.29 |           | 
    ##              |      0.22 |      0.17 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Positive |       102 |       218 |       320 | 
    ##              |      6.94 |      4.89 |           | 
    ##              |      0.32 |      0.68 |      0.62 | 
    ##              |      0.47 |      0.71 |           | 
    ##              |      0.20 |      0.42 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       215 |       305 |       520 | 
    ##              |      0.41 |      0.59 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  30.77496     d.f. =  1     p =  2.897527e-08 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  29.76792     d.f. =  1     p =  4.869843e-08 
    ## 
    ##  
    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## | Chi-square contribution |
    ## |           N / Row Total |
    ## |           N / Col Total |
    ## |         N / Table Total |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  520 
    ## 
    ##  
    ##              | Polyphagia 
    ##        class |        No |       Yes | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Negative |       152 |        48 |       200 | 
    ##              |     17.11 |     20.43 |           | 
    ##              |      0.76 |      0.24 |      0.38 | 
    ##              |      0.54 |      0.20 |           | 
    ##              |      0.29 |      0.09 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Positive |       131 |       189 |       320 | 
    ##              |     10.69 |     12.77 |           | 
    ##              |      0.41 |      0.59 |      0.62 | 
    ##              |      0.46 |      0.80 |           | 
    ##              |      0.25 |      0.36 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       283 |       237 |       520 | 
    ##              |      0.54 |      0.46 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  61.00063     d.f. =  1     p =  5.705666e-15 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  59.59525     d.f. =  1     p =  1.165158e-14 
    ## 
    ##  
    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## | Chi-square contribution |
    ## |           N / Row Total |
    ## |           N / Col Total |
    ## |         N / Table Total |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  520 
    ## 
    ##  
    ##              | Genital.thrush 
    ##        class |        No |       Yes | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Negative |       167 |        33 |       200 | 
    ##              |      0.87 |      3.02 |           | 
    ##              |      0.83 |      0.16 |      0.38 | 
    ##              |      0.41 |      0.28 |           | 
    ##              |      0.32 |      0.06 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Positive |       237 |        83 |       320 | 
    ##              |      0.54 |      1.89 |           | 
    ##              |      0.74 |      0.26 |      0.62 | 
    ##              |      0.59 |      0.72 |           | 
    ##              |      0.46 |      0.16 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       404 |       116 |       520 | 
    ##              |      0.78 |      0.22 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  6.324962     d.f. =  1     p =  0.01190501 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  5.792149     d.f. =  1     p =  0.0160979 
    ## 
    ##  
    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## | Chi-square contribution |
    ## |           N / Row Total |
    ## |           N / Col Total |
    ## |         N / Table Total |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  520 
    ## 
    ##  
    ##              | visual.blurring 
    ##        class |        No |       Yes | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Negative |       142 |        58 |       200 | 
    ##              |      9.05 |     11.15 |           | 
    ##              |      0.71 |      0.29 |      0.38 | 
    ##              |      0.49 |      0.25 |           | 
    ##              |      0.27 |      0.11 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Positive |       145 |       175 |       320 | 
    ##              |      5.66 |      6.97 |           | 
    ##              |      0.45 |      0.55 |      0.62 | 
    ##              |      0.51 |      0.75 |           | 
    ##              |      0.28 |      0.34 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       287 |       233 |       520 | 
    ##              |      0.55 |      0.45 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  32.83894     d.f. =  1     p =  1.001189e-08 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  31.80846     d.f. =  1     p =  1.701504e-08 
    ## 
    ##  
    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## | Chi-square contribution |
    ## |           N / Row Total |
    ## |           N / Col Total |
    ## |         N / Table Total |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  520 
    ## 
    ##  
    ##              | Itching 
    ##        class |        No |       Yes | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Negative |       101 |        99 |       200 | 
    ##              |      0.03 |      0.03 |           | 
    ##              |      0.50 |      0.49 |      0.38 | 
    ##              |      0.38 |      0.39 |           | 
    ##              |      0.19 |      0.19 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Positive |       166 |       154 |       320 | 
    ##              |      0.02 |      0.02 |           | 
    ##              |      0.52 |      0.48 |      0.62 | 
    ##              |      0.62 |      0.61 |           | 
    ##              |      0.32 |      0.30 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       267 |       253 |       520 | 
    ##              |      0.51 |      0.49 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  0.09314444     d.f. =  1     p =  0.7602171 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  0.04623544     d.f. =  1     p =  0.8297484 
    ## 
    ##  
    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## | Chi-square contribution |
    ## |           N / Row Total |
    ## |           N / Col Total |
    ## |         N / Table Total |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  520 
    ## 
    ##  
    ##              | Irritability 
    ##        class |        No |       Yes | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Negative |       184 |        16 |       200 | 
    ##              |      6.95 |     21.74 |           | 
    ##              |      0.92 |      0.08 |      0.38 | 
    ##              |      0.47 |      0.13 |           | 
    ##              |      0.35 |      0.03 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Positive |       210 |       110 |       320 | 
    ##              |      4.35 |     13.59 |           | 
    ##              |      0.66 |      0.34 |      0.62 | 
    ##              |      0.53 |      0.87 |           | 
    ##              |      0.40 |      0.21 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       394 |       126 |       520 | 
    ##              |      0.76 |      0.24 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  46.63387     d.f. =  1     p =  8.556828e-12 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  45.20835     d.f. =  1     p =  1.771483e-11 
    ## 
    ##  
    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## | Chi-square contribution |
    ## |           N / Row Total |
    ## |           N / Col Total |
    ## |         N / Table Total |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  520 
    ## 
    ##  
    ##              | delayed.healing 
    ##        class |        No |       Yes | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Negative |       114 |        86 |       200 | 
    ##              |      0.32 |      0.38 |           | 
    ##              |      0.57 |      0.43 |      0.38 | 
    ##              |      0.41 |      0.36 |           | 
    ##              |      0.22 |      0.17 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Positive |       167 |       153 |       320 | 
    ##              |      0.20 |      0.24 |           | 
    ##              |      0.52 |      0.48 |      0.62 | 
    ##              |      0.59 |      0.64 |           | 
    ##              |      0.32 |      0.29 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       281 |       239 |       520 | 
    ##              |      0.54 |      0.46 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  1.147679     d.f. =  1     p =  0.2840355 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  0.9620937     d.f. =  1     p =  0.3266599 
    ## 
    ##  
    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## | Chi-square contribution |
    ## |           N / Row Total |
    ## |           N / Col Total |
    ## |         N / Table Total |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  520 
    ## 
    ##  
    ##              | partial.paresis 
    ##        class |        No |       Yes | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Negative |       168 |        32 |       200 | 
    ##              |     25.76 |     34.04 |           | 
    ##              |      0.84 |      0.16 |      0.38 | 
    ##              |      0.57 |      0.14 |           | 
    ##              |      0.32 |      0.06 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Positive |       128 |       192 |       320 | 
    ##              |     16.10 |     21.27 |           | 
    ##              |      0.40 |      0.60 |      0.62 | 
    ##              |      0.43 |      0.86 |           | 
    ##              |      0.25 |      0.37 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       296 |       224 |       520 | 
    ##              |      0.57 |      0.43 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  97.17375     d.f. =  1     p =  6.350314e-23 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  95.38763     d.f. =  1     p =  1.565289e-22 
    ## 
    ##  
    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## | Chi-square contribution |
    ## |           N / Row Total |
    ## |           N / Col Total |
    ## |         N / Table Total |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  520 
    ## 
    ##  
    ##              | muscle.stiffness 
    ##        class |        No |       Yes | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Negative |       140 |        60 |       200 | 
    ##              |      1.80 |      3.00 |           | 
    ##              |      0.70 |      0.30 |      0.38 | 
    ##              |      0.43 |      0.31 |           | 
    ##              |      0.27 |      0.12 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Positive |       185 |       135 |       320 | 
    ##              |      1.12 |      1.88 |           | 
    ##              |      0.58 |      0.42 |      0.62 | 
    ##              |      0.57 |      0.69 |           | 
    ##              |      0.36 |      0.26 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       325 |       195 |       520 | 
    ##              |      0.62 |      0.38 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  7.8     d.f. =  1     p =  0.005224623 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  7.288667     d.f. =  1     p =  0.006939096 
    ## 
    ##  
    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## | Chi-square contribution |
    ## |           N / Row Total |
    ## |           N / Col Total |
    ## |         N / Table Total |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  520 
    ## 
    ##  
    ##              | Alopecia 
    ##        class |        No |       Yes | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Negative |        99 |       101 |       200 | 
    ##              |      7.88 |     15.02 |           | 
    ##              |      0.49 |      0.50 |      0.38 | 
    ##              |      0.29 |      0.56 |           | 
    ##              |      0.19 |      0.19 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Positive |       242 |        78 |       320 | 
    ##              |      4.93 |      9.39 |           | 
    ##              |      0.76 |      0.24 |      0.62 | 
    ##              |      0.71 |      0.44 |           | 
    ##              |      0.47 |      0.15 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       341 |       179 |       520 | 
    ##              |      0.66 |      0.34 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  37.21247     d.f. =  1     p =  1.059342e-09 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  36.06414     d.f. =  1     p =  1.909279e-09 
    ## 
    ##  
    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## | Chi-square contribution |
    ## |           N / Row Total |
    ## |           N / Col Total |
    ## |         N / Table Total |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  520 
    ## 
    ##  
    ##              | Obesity 
    ##        class |        No |       Yes | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Negative |       173 |        27 |       200 | 
    ##              |      0.28 |      1.38 |           | 
    ##              |      0.86 |      0.14 |      0.38 | 
    ##              |      0.40 |      0.31 |           | 
    ##              |      0.33 |      0.05 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Positive |       259 |        61 |       320 | 
    ##              |      0.18 |      0.87 |           | 
    ##              |      0.81 |      0.19 |      0.62 | 
    ##              |      0.60 |      0.69 |           | 
    ##              |      0.50 |      0.12 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       432 |        88 |       520 | 
    ##              |      0.83 |      0.17 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  2.708675     d.f. =  1     p =  0.09980384 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  2.327474     d.f. =  1     p =  0.127108 
    ## 
    ##  
    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## | Chi-square contribution |
    ## |           N / Row Total |
    ## |           N / Col Total |
    ## |         N / Table Total |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  520 
    ## 
    ##  
    ##              | class 
    ##        class |  Negative |  Positive | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Negative |       200 |         0 |       200 | 
    ##              |    196.92 |    123.08 |           | 
    ##              |      1.00 |      0.00 |      0.38 | 
    ##              |      1.00 |      0.00 |           | 
    ##              |      0.38 |      0.00 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Positive |         0 |       320 |       320 | 
    ##              |    123.08 |     76.92 |           | 
    ##              |      0.00 |      1.00 |      0.62 | 
    ##              |      0.00 |      1.00 |           | 
    ##              |      0.00 |      0.62 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       200 |       320 |       520 | 
    ##              |      0.38 |      0.62 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  520     d.f. =  1     p =  4.231963e-115 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  515.7836     d.f. =  1     p =  3.498538e-114 
    ## 
    ## 

### Duas Numéricas

É útil identificar se existe uma *associação linear* entre duas
variáveis ou entre mais de duas variáveis e, se apropriado, *quantificar
a associação*.

Um dispositivo bastante útil para se verificar a *associação* entre duas
variáveis quantitativas, ou entre dois conjuntos de dados, é o
***diagrama de dispersão***.

Sua associação pode ser *quantificada* utilizando-se uma medida
estatística chamada ***coeficiente de correlação*** ou *grau de
associação*.

#### Diagramas de Dispersão

A ***relação*** entre as variáveis pode ser *fortemente linear*, *não
linear* ou mesmo *inexistente*. Portanto, um diagrama de dispersão ***é
uma primeira indicação*** útil da possível existência ***de uma
associação entre duas variáveis***.

``` r
# Diagramas de Dispersão - Matriz de Correlação
plot(df[,c(var.num, var.num_con)])
```

![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Matriz%20de%20Correlação-1.png)<!-- -->

#### Coeficiente de Correlação

Mede a ***força*** de associação entre duas variáveis. Essa medição leva
em consideração a dispersão entre os valores dados. Quanto menos
dispersos estiverem os dados, mais forte será a dependência, isto é, a
associação entre as variáveis.

O coeficiente de correlação “R” assume um valor entre \[– 1 e + 1\],
isto é:

-   Se r = 1, a correlação é positiva perfeita;
-   Se r = -1, a correlação é negativa perfeita;
-   Se r = 0, a correlação é nula.

``` r
# Coeficiente de Correlação - feature numérico x target categórico
cor(df$Age,
    df[target_names] %>% unlist() %>% as.numeric())
```

    ## [1] 0.108679

``` r
# var, cov e cor calculam a variância de x e a covariância ou correlação de x e y se esses forem vetores. 
# Se x e y são matrizes, então as covariâncias (ou correlações) entre as colunas de x e as colunas de y são calculadas.
```

``` r
# Gráfico dos Coeficientes de Correlação
# df[target_names] = df[target_names] %>% unlist() %>% as.numeric()
# cor.plot(df[,c(var.num, target_names)])
df[target_names] = df[target_names] %>% unlist() %>% as.numeric()
df[feature_names] = df[feature_names] %>% unlist() %>% as.numeric()
M = cor(df[,c(feature_names, target_names)])
```

``` r
corrplot(M, method = 'color')
```

![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

#### Covariância

``` r
cov(df[,c(var.num, target_names)])
```

    ##               Age     class
    ## Age   147.6581258 0.6431006
    ## class   0.6431006 0.2371424

``` r
# Diagramas de Dispersão + Coeficientes de Correlação
pairs.panels(df[,c(var.num, target_names)])
```

![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Diagramas%20de%20Dispersão%20+%20Coeficientes%20de%20Correlação-1.png)<!-- -->

### Uma Númerica e outra Categórica

Em geral ***analisa-se o que acontece com a variável quantitativa dentro
de cada categoria da variável qualitativa.***. Essa análise pode ser
conduzida por meio de ***medidas-resumo*** ou ***box plot***.

As medidas-resumo são agrupadas por categoria da variável qualitativa. A
partir daí, constroem-se boxplots baseados em cada medida-resumo. Então,
os boxplots são comparados visualmente.

#### Medidas-resumo

*As medidas-resumo são calculadas para a variável quantitativa, a
variável que se quer observar o comportamento.*

#### Boxplot

``` r
# Boxplots valor conforme as Categóricas
par(mfrow=c(1,3), cex = 0.65)
for(i in var.fct){
  boxplot(df[,var.num] ~ df[,i], 
          beside = TRUE, 
          xlab = names(df)[grep(i, names(df))],
          ylab = names(df)[grep(var.num, names(df))])
}
```

![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numericas%20x%20Categóricas-1.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numericas%20x%20Categóricas-2.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numericas%20x%20Categóricas-3.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numericas%20x%20Categóricas-4.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numericas%20x%20Categóricas-5.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numericas%20x%20Categóricas-6.png)<!-- -->

``` r
# Boxplots Numéricas conforme as Survived
par(mfrow=c(1,1))
for(i in 0:length(df)){
  if(is.numeric(df[,i])){
    boxplot(df[,i] ~ class, ylab = names(df)[i], data = df)
  }
}
```

![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-1.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-2.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-3.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-4.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-5.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-6.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-7.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-8.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-9.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-10.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-11.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-12.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-13.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-14.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-15.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-16.png)<!-- -->![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-17.png)<!-- -->

-   Intervalo Interquartil (IQ): Diferença entre o 3º e o 1º quartil. O
    comprimento do lado vertical.

-   Limite Inferior (LI): *Q1 - 1,5IQ*

-   Limite Superior (LS): *Q3 + 1,5IQ*

-   Outliers: Um dado será considerado outlier se ele for *menor que o*
    ***LI*** ou *maior que o* ***LS***.

    -   O que fazer com os outliers:

        1.  Excluir - valores absurdos, p.ex. idade de 180 anos;
        2.  Buscar possíveis causas;
        3.  Verificar diferenças na análise com e sem o outlier;
        4.  Categorizar a variável, p.ex. o outlier entraria em valores
            maiores que x.

-   Informações que podem ser retiradas:

    1.  Número de outliers;
    2.  Valor mediano aproximado;
    3.  Intervalo onde estão 50% dos valores;
    4.  Simetria da variável baseada na distância dos quartis até a
        mediana.

# Pré-Processamento

## Escala e Normalização

Como o intervalo de valores dos dados brutos varia muito, em alguns
algoritmos de aprendizado de máquina, as funções objetivas não
funcionarão corretamente sem
[normalização](https://en.wikipedia.org/wiki/Normalization_(statistics)).
Outra razão pela qual o dimensionamento de recursos é aplicado é que a
[descida do gradiente](https://en.wikipedia.org/wiki/Gradient_descent)
converge muito mais rapidamente com o dimensionamento de recursos do que
sem ele.

-   Métodos: *[Feature
    Scaling](https://en.wikipedia.org/wiki/Feature_scaling)*

    4.  **Re-escalonar (Normalização Min-Max):** é o método mais simples
        e consiste em redimensionar o intervalo de recursos para
        dimensionar o intervalo em \[0, 1\] ou \[-1, 1\].

    5.  **Escore-Z (Padronização):** No aprendizado de máquina, podemos
        lidar com vários tipos de dados, por exemplo, sinais de áudio e
        valores de pixel para dados de imagem, e esses dados podem
        incluir várias dimensões. A padronização de recursos faz com que
        os valores de cada recurso nos dados tenham média zero (ao
        subtrair a média no numerador) e variação de unidade. Esse
        método é amplamente utilizado para normalização em muitos
        algoritmos de aprendizado de máquina (por exemplo, máquinas de
        vetores de suporte, regressão logística e redes neurais
        artificiais). O método geral de cálculo é determinar a média de
        distribuição e o desvio padrão para cada recurso. Em seguida,
        subtraímos a média de cada recurso. Em seguida, dividimos os
        valores (a média já está subtraída) de cada recurso pelo seu
        desvio padrão.

### Normalização (Min-Max)

### Padronização (Z-Score)

``` r
Z_df <- dget(file = "data/df.R")
# ------------------------------------------------------------------------------

# Padroniza
Z_df[c(var.num, var.num_con)] <- sapply(Z_df[c(var.num, var.num_con)], scale)
```

``` r
# Exporta Dados Padronizados
dput(Z_df, file = "data/Z_df.R")
```

``` r
print('Original:')
```

    ## [1] "Original:"

``` r
stargazer(df, type = "text")
```

    ## 
    ## ================================================================
    ## Statistic           N   Mean  St. Dev. Min Pctl(25) Pctl(75) Max
    ## ----------------------------------------------------------------
    ## Age                520 48.029  12.151  16     39       57    90 
    ## Gender             520 1.631   0.483    1     1        2      2 
    ## Polyuria           520 1.496   0.500    1     1        2      2 
    ## Polydipsia         520 1.448   0.498    1     1        2      2 
    ## sudden.weight.loss 520 1.417   0.494    1     1        2      2 
    ## weakness           520 1.587   0.493    1     1        2      2 
    ## Polyphagia         520 1.456   0.499    1     1        2      2 
    ## Genital.thrush     520 1.223   0.417    1     1        1      2 
    ## visual.blurring    520 1.448   0.498    1     1        2      2 
    ## Itching            520 1.487   0.500    1     1        2      2 
    ## Irritability       520 1.242   0.429    1     1        1      2 
    ## delayed.healing    520 1.460   0.499    1     1        2      2 
    ## partial.paresis    520 1.431   0.496    1     1        2      2 
    ## muscle.stiffness   520 1.375   0.485    1     1        2      2 
    ## Alopecia           520 1.344   0.476    1     1        2      2 
    ## Obesity            520 1.169   0.375    1     1        1      2 
    ## class              520 1.615   0.487    1     1        2      2 
    ## ----------------------------------------------------------------

``` r
print('Padronizado:')
```

    ## [1] "Padronizado:"

``` r
stargazer(Z_df, type = "text")
```

    ## 
    ## ===========================================================
    ## Statistic  N  Mean  St. Dev.  Min   Pctl(25) Pctl(75)  Max 
    ## -----------------------------------------------------------
    ## Age       520 0.000  1.000   -2.636  -0.743   0.738   3.454
    ## -----------------------------------------------------------

## Train, Test, Split

``` r
# Packages
# ==============================================================================
## install.packages("caret")
#library(caret) 
## install.packages("kernlab")
#library(kernlab)
# ==============================================================================

# Carrega conjuntos de dados especificados 
# ou lista os conjuntos de dados disponíveis.
#data(df)
#df = Z_df

# Cria uma série de partições de teste / treinamento
inTrain <- createDataPartition(y = df$class, # Um vetor de resultados.
                               p = 0.75,     # Percentual para treino.
                               list = FALSE)

# Coloca cada partição em um dataframe diferente
training <- df[inTrain,]
testing <- df[-inTrain,]

dim(training)
```

    ## [1] 390  17

``` r
# Exporta os dados
dput(training, file = "data/training.R")
dput(testing,  file = "data/testing.R")
```

## Feature Selection

# Modelagem

``` r
massa <- training
mdl.tst <- testing
```

------------------------------------------------------------------------

## Amostragem

> **Dividir os dados em Treinamento, Validação e Teste.**

-   ***Conjunto de dados de treinamento:*** a amostra de dados usada
    para ajustar o modelo.

-   ***Conjunto de dados de validação:*** é usado para avaliar um
    determinado modelo. Usamos esses dados para ajustar os
    hiperparâmetros do modelo. Portanto, o modelo ocasionalmente vê
    esses dados, mas nunca “aprende” com eles. Nós usamos os resultados
    do conjunto de validação e atualizamos os hiperparâmetros de nível
    superior. Portanto, a validação definida de uma maneira afeta um
    modelo, mas indiretamente.

-   ***Conjunto de dados de teste:*** a amostra de dados usada para
    fornecer uma avaliação imparcial de um ajuste final do modelo no
    conjunto de dados de treinamento. Só é usado quando um modelo é
    completamente treinado (usando os conjuntos de treino e validação).
    O conjunto de testes geralmente é o que é usado para avaliar modelos
    concorrentes (por exemplo, em muitas competições Kaggle, o conjunto
    de validação é lançado inicialmente juntamente com o conjunto de
    treinamento e o conjunto de testes real é lançado somente quando a
    competição está prestes a fechar e é o resultado do modelo no
    conjunto de teste que decide o vencedor). Muitas vezes, o conjunto
    de validação é usado como o conjunto de teste, mas não é uma boa
    prática. O conjunto de testes geralmente é bem organizado. Ele
    contém dados cuidadosamente amostrados que abrangem as várias
    classes que o modelo enfrentaria, quando usado no mundo real.

> ***Nota sobre validação cruzada:*** *Muitas vezes, as pessoas primeiro
> dividem seus conjuntos de dados em 2 - Treinar e Testar. Depois disso,
> eles deixam de lado o conjunto de testes e escolhem aleatoriamente X%
> do conjunto de dados de trem para ser o conjunto de trens real e o
> restante (100-X)% para o conjunto de validação , onde X é um número
> fixo (por exemplo, 80% ), o modelo é treinado e validado
> iterativamente nesses diferentes conjuntos. Existem várias maneiras de
> fazer isso e é comumente conhecido como Validação Cruzada.
> Basicamente, você usa seu conjunto de treinamento para gerar várias
> divisões dos conjuntos de treinamento e validação. A validação cruzada
> evita o ajuste e está se tornando cada vez mais popular, com a
> Validação Cruzada K-fold sendo o método mais popular de validação
> cruzada.*

**Fontes:**

1.  *<https://towardsdatascience.com/train-validation-and-test-sets-72cb40cba9e7>*

2.  *<https://machinelearningmastery.com/difference-test-validation-datasets/>*

> ***Resampling***

A semente é um número criado a partir do horário atual e do ID do
processo, para garantir a aleatoriedade dos resultados. Fixá-lo permite
que esses resultados sejam reproduzíveis.

``` r
# Configurando uma semente para que o Gerador Aleatório de Números seja reproduzível.
set.seed(1618)

# Criando uma coluna com índices randômicos.
massa[ ,'index'] <- ifelse(runif(nrow(massa)) < 0.8, 1, 0)

# Criando os conjuntos de treino e de validação.
mdl.trn <- massa[massa$index == 1, ]
mdl.vld <- massa[massa$index == 0, ]

# Obtem o índice (posição), no vetor de nomes, onde corresponde ao valor 'index'.
col_idx <- grep('index', names(mdl.trn))

# Remove a coluna 'index' dos datasets, utilizando a posição obtida.
mdl.trn <- mdl.trn[ , -col_idx]
mdl.vld <- mdl.vld[ , -col_idx]
```

------------------------------------------------------------------------

## Treino

------------------------------------------------------------------------

> Treinando um ***Modelo Linear Generalizado*** para classificação.

``` r
# Gerar Modelos de Classificação
ajt.trn <- glm(formula = class ~ .,
               family = gaussian,
               data = mdl.trn)
```

**Resumo do modelo treinado:**

``` r
summary(ajt.trn)
```

    ## 
    ## Call:
    ## glm(formula = class ~ ., family = gaussian, data = mdl.trn)
    ## 
    ## Deviance Residuals: 
    ##      Min        1Q    Median        3Q       Max  
    ## -0.63942  -0.18024  -0.06404   0.17182   0.95921  
    ## 
    ## Coefficients:
    ##                     Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)         0.809587   0.130725   6.193 1.83e-09 ***
    ## Age                 0.001014   0.001670   0.607 0.544140    
    ## Gender             -0.259993   0.038575  -6.740 7.53e-11 ***
    ## Polyuria            0.294123   0.043389   6.779 5.95e-11 ***
    ## Polydipsia          0.263893   0.047062   5.607 4.49e-08 ***
    ## sudden.weight.loss  0.029548   0.038908   0.759 0.448155    
    ## weakness            0.010102   0.039589   0.255 0.798759    
    ## Polyphagia          0.072935   0.038696   1.885 0.060374 .  
    ## Genital.thrush      0.235252   0.045179   5.207 3.46e-07 ***
    ## visual.blurring     0.057552   0.041775   1.378 0.169276    
    ## Itching            -0.125061   0.038717  -3.230 0.001368 ** 
    ## Irritability        0.122403   0.039756   3.079 0.002260 ** 
    ## delayed.healing    -0.132178   0.039528  -3.344 0.000925 ***
    ## partial.paresis     0.133958   0.041014   3.266 0.001210 ** 
    ## muscle.stiffness   -0.020352   0.038493  -0.529 0.597370    
    ## Alopecia           -0.018793   0.044671  -0.421 0.674264    
    ## Obesity            -0.068837   0.044562  -1.545 0.123409    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for gaussian family taken to be 0.08330428)
    ## 
    ##     Null deviance: 78.078  on 332  degrees of freedom
    ## Residual deviance: 26.324  on 316  degrees of freedom
    ## AIC: 135.97
    ## 
    ## Number of Fisher Scoring iterations: 2

------------------------------------------------------------------------

## Validação

------------------------------------------------------------------------

> Validando o modelo através de predições utilizando o *Conjunto de
> Validação*.

``` r
pred <- predict(
  ajt.trn, 
  newdata = mdl.vld,
  type = "response")

# Criando um dataframe com os dados observados e os preditos.
previsoes <- data.frame(
  observado = mdl.vld[,target_names] %>% factor(labels = c("Negative", "Positive")),
  previsto = pred
  %>% 
    round() %>% 
    factor(labels = c("Negative", "Positive")))
```

``` r
CrossTable(previsoes$observado, 
           previsoes$previsto)
```

    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## | Chi-square contribution |
    ## |           N / Row Total |
    ## |           N / Col Total |
    ## |         N / Table Total |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  57 
    ## 
    ##  
    ##                     | previsoes$previsto 
    ## previsoes$observado |  Negative |  Positive | Row Total | 
    ## --------------------|-----------|-----------|-----------|
    ##            Negative |        20 |         0 |        20 | 
    ##                     |    14.372 |    11.228 |           | 
    ##                     |     1.000 |     0.000 |     0.351 | 
    ##                     |     0.800 |     0.000 |           | 
    ##                     |     0.351 |     0.000 |           | 
    ## --------------------|-----------|-----------|-----------|
    ##            Positive |         5 |        32 |        37 | 
    ##                     |     7.769 |     6.069 |           | 
    ##                     |     0.135 |     0.865 |     0.649 | 
    ##                     |     0.200 |     1.000 |           | 
    ##                     |     0.088 |     0.561 |           | 
    ## --------------------|-----------|-----------|-----------|
    ##        Column Total |        25 |        32 |        57 | 
    ##                     |     0.439 |     0.561 |           | 
    ## --------------------|-----------|-----------|-----------|
    ## 
    ## 

``` r
chisq.test(previsoes$observado, previsoes$previsto)
```

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  previsoes$observado and previsoes$previsto
    ## X-squared = 36.004, df = 1, p-value = 1.97e-09

![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

------------------------------------------------------------------------

### Avaliação

Para avaliar o desempenho do modelo nos dados de validação, foi
construída uma *matriz de confusão*.

``` r
cm <- confusionMatrix(previsoes$observado, previsoes$previsto, positive = 'Positive')
```

Matriz de Confusão - Usando o pacote Caret

``` r
print(cm)
```

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction Negative Positive
    ##   Negative       20        0
    ##   Positive        5       32
    ##                                          
    ##                Accuracy : 0.9123         
    ##                  95% CI : (0.807, 0.9709)
    ##     No Information Rate : 0.5614         
    ##     P-Value [Acc > NIR] : 7.06e-09       
    ##                                          
    ##                   Kappa : 0.8179         
    ##                                          
    ##  Mcnemar's Test P-Value : 0.07364        
    ##                                          
    ##             Sensitivity : 1.0000         
    ##             Specificity : 0.8000         
    ##          Pos Pred Value : 0.8649         
    ##          Neg Pred Value : 1.0000         
    ##              Prevalence : 0.5614         
    ##          Detection Rate : 0.5614         
    ##    Detection Prevalence : 0.6491         
    ##       Balanced Accuracy : 0.9000         
    ##                                          
    ##        'Positive' Class : Positive       
    ## 

------------------------------------------------------------------------

***Accuracy***

Através da *Matriz de Confusão* obtivemos a ***Acurácia de 91.23%***,
ela corresponde a fração das premissas corretas em relação ao total de
observações. Esta métrica também poderia ser calculada utilizando a
função *table()* ou a função *CrossTable()*, pois ela corresponde a soma
das predições corretas dividida pelo total de observações.  
Porém, não é suficiente (e nem segura) para avaliarmos a eficiência do
modelo. Analisamos então, outras métricas obtidas pela Confusion Matrix,
que nos informe não apenas o percentual de acertos, mas também a
precisão e a sensibilidade.

------------------------------------------------------------------------

***95% CI***

O ***Intervalo de Confiança***, denotado por *95% CI*, é ***\[0.807,
0.9709\]***. Ele é uma *estimativa por intervalo* de um *parâmetro
populacional*, que com dada frequência (*Nível de Confiança*), inclui o
parâmetro de interesse. Nesse caso específico, significa dizer que 95%
dos *intervalos de confiança* observados têm o valor real do parâmetro.

***No Information Rate***

***P-Value \[Acc &gt; NIR\]***

***Kappa***

O ***Teste de Concordância Kappa*** foi igual a ***0.8179***. O
coeficiente de Kappa tem a finalidade de medir o grau de concordância
entre proporções. Ele demonstra se uma dada classificação pode ser
considerada confiável.

O seu valor pode ser interpretado por meio de uma tabela.

------------------------------------------------------------------------

***Mcnemar’s Test P-Value***

------------------------------------------------------------------------

***Sensitivity***

***Specificity***

***Pos Pred Value***

***Neg Pred Value***

***Prevalence***

***Detection Rate***

***Detection Prevalence***

***Balanced Accuracy***

``` r
fourfoldplot(cm$table)
```

![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Plot%20Confusion%20Matrix-1.png)<!-- -->

------------------------------------------------------------------------

O pacote Caret nos dá, já calculadas, as principais métricas
estatísticas, baseadas nos tipos de acertos e, nos tipos de erros. Para
a *Acurácia*, não havia diferenças, apenas acertos e erros.

Agora, os erros são classificados como ***“Erro do Tipo I”*** e ***“Erro
do Tipo II”***. O primeiro é *baseado nos* ***Falsos Positivos (FP)***,
o segundo é *baseado nos* ***Falsos Negativos (FN)***.

O **Erro do Tipo I** *(FP)*, é o resultado que tem *Significância
Estatística*. A probabilidade de se cometer um *erro do tipo I* em um
teste de hitótese é denominada ***Nível de Significância***,
representado por ***() (alfa)***. Sua interpretação é que em *() vezes*
(p.ex. 5%) rejeitaremos a *hipótese nula (H0)* quando ela é
verdadeira.  
O **Erro do Tipo II** *(FN)*, é o ***Poder do Teste***, representado por
***() (beta)***. Ocorre quando a hipótese nula é falsa, mas erroneamente
falhamos ao ser rejeitada.

Normalmente, ao se testar uma hipótese, é definido o *nível de
significância* do *Erro do Tipo I*, chamado de α, tipicamente de 5%. Ou
seja, com α = 0,05, existe 5% de chance de se rejeitar a hipótese nula,
no caso dela ser verdadeira.  
- Ao se diminuir a probabilidade de ocorrer um *Erro do Tipo I*, ou
seja, diminuindo o valor de α, aumenta-se a probabilidade de se ocorrer
um *Erro do Tipo II*. A probabilidade da ocorrência do Erro do tipo II é
chamada de β.

``` r
mosaicplot(cm$table, color = TRUE, shade = TRUE, main = "Mosaico para Confusion Matrix")
```

![](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Mosaico%20Confusion%20Matrix-1.png)<!-- -->

A partir destas definições construímos as duas principais métricas, a
***Precisão*** e a ***Sensibilidade***.

A **Precisão** demonstra que de todos os casos previstos como **TP**,
quantos realmente estavam certos.  
A **Sensibilidade** mostra que de todos os possíveis casos onde o target
era 1, quanto o modelo conseguiu capturar.

Se *diminuírmos o erro do tipo I* (FP), tornaremos nosso modelo ***mais
preciso***.  
Se *diminuírmos o erro do tipo II* (FN), tornaremos o modelo ***mais
sensível***.

Para manipular o *Erro Tipo I*…  
Para manipular o *Erro Tipo II* devemos utilizar modelos diferentes ou
aumentar o tamanho da amostra.

Para alterarmos esses valores, devemos analizar as predições como uma
distribuição de probabilidade, encontrando a probabilidade de cada
observação pertencer a classe 1 ou 0.

O ***Threshold*** é o percentual que definirá essa escolha, o corte que
separará as classes.

A ***F1-Score*** é uma alternativa para não utilizarmos a Precisão e a
Sensibilidade. Ela é uma média harmônica entre essas duas, o que torna a
métrica mais sensível a desproporções.

Outra forma é a ***F-Beta*** onde podemos escolher o Peso entre Precisão
e Sensibilidade através do parâmetro Beta.

------------------------------------------------------------------------

#### Métricas de Avaliação

Avaliar um modelo de Classificação Binária:

*Acurácia (Accuracy)*, *Precisão (Precision)*, *Sensibilidade (Recall)*
e *Pontuação F1 (F1-Score)*.

``` r
acuracia <- (cm$table[1,1] + cm$table[2,2])/(cm$table[1,1] + cm$table[1,2] + cm$table[2,1] + cm$table[2,2])
```

``` r
# Precision
precisao <- cm$table[1,1] / (cm$table[1,1] + cm$table[1,2])
```

``` r
sensibilidade <- cm$table[1,1] / (cm$table[1,1] + cm$table[2,1])
```

``` r
f1 <- (2 * sensibilidade * precisao) / (sensibilidade + precisao)
```

``` r
metricas <- data.frame(
  "Acuracia"      = acuracia, 
  "Precisao"      = precisao, 
  "Sensibilidade" = sensibilidade, 
  "F1-Score"      = f1)

metricas
```

    ##    Acuracia Precisao Sensibilidade  F1.Score
    ## 1 0.9122807        1           0.8 0.8888889

#### Curva ROC

``` r
class1 <- predict(ajt.trn, newdata = mdl.vld, type = "response")
class2 <- mdl.vld[target_names]

pred <- prediction(class1, class2)
perf <- performance(pred, "tpr", "fpr")
```

``` r
# Gerando uma curva ROC em R
plot(perf, col = rainbow(10, alpha = NULL))
```

![Gerando uma curva ROC em
R](Early_Stage_Diabetes_Risk_Prediction_files/figure-gfm/Output%20ROC-1.png)

------------------------------------------------------------------------

Realizando o ***Teste Chi-Quadrado*** (X²).

``` r
chisq.test(previsoes$observado, previsoes$previsto)
```

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  previsoes$observado and previsoes$previsto
    ## X-squared = 36.004, df = 1, p-value = 1.97e-09

------------------------------------------------------------------------

## Teste

Predição de teste utilizando novos dados.

``` r
predict(ajt.trn, 
        newdata = mdl.tst, 
        type = "response") %>% 
  round() %>% 
  factor(labels = c("Negative", "Positive")) -> predicao
```

Tabela para as predições de teste.

``` r
CrossTable(predicao, prop.t = T, digits = 2)
```

    ## 
    ##  
    ##    Cell Contents
    ## |-------------------------|
    ## |                       N |
    ## |         N / Table Total |
    ## |-------------------------|
    ## 
    ##  
    ## Total Observations in Table:  130 
    ## 
    ##  
    ##           |  Negative |  Positive | 
    ##           |-----------|-----------|
    ##           |        63 |        67 | 
    ##           |      0.48 |      0.52 | 
    ##           |-----------|-----------|
    ## 
    
___