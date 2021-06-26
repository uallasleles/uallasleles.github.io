---
name: Early stage diabetes risk prediction dataset
title: "Early stage diabetes risk prediction dataset."
subtitle: "Aprendizado de Máquina"
description: Early stage diabetes risk prediction dataset.
author: "Uallas Leles"
date: "Início: Junho de 2021 | Atualização: `r format(Sys.Date(), '%d/%B/%Y')`"
output:
  html_notebook: 
    fig_caption: yes
    highlight: espresso
    number_sections: yes
    theme: paper
    toc: yes
encoding: "UTF-8"
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(  echo=TRUE )
                      # , message=FALSE
                      # , warning=FALSE
                      # , include=TRUE)
options(OutDec = ",")
```

```{r Configura_WD, include=FALSE}
source(file = "scripts/set_wd.R")
```

Diretório de Trabalho:
```{r Consulta_WD, echo=FALSE}
getwd()
```

# Dependências

## Pacotes
```{r Instalando Pacotes, eval=FALSE, include=FALSE}
source(file = "scripts/packages_install.R")
```

```{r Carregando Pacotes, include=FALSE}
source(file = "scripts/packages_require.R")
```

## Funções

```{r Funções, message=TRUE}
source(file = "scripts/functions.R")
```

# Definição do Problema
___
**Definição:**
*Early stage diabetes risk prediction dataset.. (2020). UCI Machine Learning Repository.*

**Dataset:**
*This dataset contains the sign and symptpom data of newly diabetic or would be diabetic patient.*
*Early stage diabetes risk prediction dataset.. (2020). UCI Machine Learning Repository.*

**Link:**
[UCI: Early stage diabetes risk prediction dataset.](https://archive-beta.ics.uci.edu/ml/datasets/529)

**Objetivo:**
*Prever o risco de diabetes no estágio inicial.*

___
# Planejamento

___
1. *Coleta de dados com StringAsFactor como false.*
3. *Limpeza e transformação das variáveis.*
4. *Dividir o conjunto de dados.*
5. *Fazer a Análise Exploratória do conjunto de treino.*

___
# Coleta

___
## Extração

```{r Importando os Dados}
source(file = "scripts/ds_load.R")
```

```{r Unindo os Datasets, eval=FALSE, include=FALSE}
source(file = "scripts/unindo_datasets.R")
```

___
### Pré-visualização dos Dados

```{r Pré-visualizando Treino}
# TREINO: Pré-visualização das primeiras observações:
etl.full %>% 
    head() %>% 
    tibble(format = "markdown", caption = "Primeiras observações")
print(etl.full)
```

___
## Resumo da Importação

___
### Dimensões
```{r Dimensoes do DataSet}
dim(etl.full) # Dimensões do dataset
```

```{r Dimensoes Treino, eval=FALSE, include=FALSE}
dim(treino) # Dimensões do dataset de treino
```

```{r Dimensões Teste, eval=FALSE, include=FALSE}
dim(teste)  # Dimensões do dataset de teste
```

___
### Estrutura

```{r Estrutura dos Dados}
str(etl.full) # Estrutura do dataset
```

___
## Dicionário de Atributos  

```{r Dicionario de Atributos, eval=FALSE, include=FALSE}
# Importar o dicionário de um arquvio texto.
```

___
## Limpeza e Transformação

Esta etapa corresponde a um pré-processamento para então iniciar a análise exploratória de dados.

___
### Dados Faltantes

```{r Verificando Missing Data, results="asis"}
# Soma de valores missing por variável em Treino
if(any(is.na(etl.full)))
  {
    subset(data.frame("SubTotal_NA" = apply(etl.full, 
                                     2, 
                                     function(x){
                                       sum(is.na(x))
                                       })), SubTotal_NA > 0)
}
```

```{r Mapa de Valores Missing, eval=FALSE, include=FALSE}
miss(etl.full)
```

___
### Imputação de Dados

```{r Imputando Missing Data, eval=FALSE, include=FALSE}
imp <- mice(etl.full, method='rf')
imp_out <- complete(imp)
etl.full$Age <- imp_out$Age
etl.full$Fare <- imp_out$Fare
```

___
### Transformando para Fator

```{r Class para fator e definindo labels}
# Variável 'class'
etl.full$class <- factor(etl.full$class, labels = c(Positive = "Positivo", Negative = "Negativo"))
```

```{r Gender para fator e definindo os labels}
# Variável 'Gender'
etl.full$Gender <- factor(etl.full$Gender, labels = c(Male = "Homem", Female = "Mulher"))
```

```{r Polyuria para fator e definindo labels}
# Variável 'Polyuria'
etl.full$Polyuria <- factor(etl.full$Polyuria, labels = c(No = "Não", Yes = "Sim"))
```

```{r Polydipsia para fator e definindo labels}
# Variável 'Polydipsia'
etl.full$Polydipsia <- factor(etl.full$Polydipsia, labels = c(No = "Não", Yes = "Sim"))
```

```{r sudden.weight.loss para fator e definindo labels}
# Variável 'sudden.weight.loss'
etl.full$sudden.weight.loss <- factor(etl.full$sudden.weight.loss, labels = c(No = "Não", Yes = "Sim"))
```

```{r weakness para fator e definindo labels}
# Variável 'weakness'
etl.full$weakness <- factor(etl.full$weakness, labels = c(No = "Não", Yes = "Sim"))
```

```{r Polyphagia para fator e definindo labels}
# Variável 'Polyphagia'
etl.full$Polyphagia <- factor(etl.full$Polyphagia, labels = c(No = "Não", Yes = "Sim"))
```

```{r Genital.thrush para fator e definindo labels}
# Variável 'Genital.thrush'
etl.full$Genital.thrush <- factor(etl.full$Genital.thrush, labels = c(No = "Não", Yes = "Sim"))
```

```{r visual.blurring para fator e definindo labels}
# Variável 'visual.blurring'
etl.full$visual.blurring <- factor(etl.full$visual.blurring, labels = c(No = "Não", Yes = "Sim"))
```

```{r Itching para fator e definindo labels}
# Variável 'Itching'
etl.full$Itching <- factor(etl.full$Itching, labels = c(No = "Não", Yes = "Sim"))
```

```{r Irritability para fator e definindo labels}
# Variável 'Irritability'
etl.full$Irritability <- factor(etl.full$Irritability, labels = c(No = "Não", Yes = "Sim"))
```

```{r delayed.healing para fator e definindo labels}
# Variável 'delayed.healing'
etl.full$delayed.healing <- factor(etl.full$delayed.healing, labels = c(No = "Não", Yes = "Sim"))
```

```{r partial.paresis para fator e definindo labels}
# Variável 'partial.paresis'
etl.full$partial.paresis <- factor(etl.full$partial.paresis, labels = c(No = "Não", Yes = "Sim"))
```

```{r muscle.stiffness para fator e definindo labels}
# Variável 'muscle.stiffness'
etl.full$muscle.stiffness <- factor(etl.full$muscle.stiffness, labels = c(No = "Não", Yes = "Sim"))
```

```{r Alopecia para fator e definindo labels}
# Variável 'Alopecia'
etl.full$Alopecia <- factor(etl.full$Alopecia, labels = c(No = "Não", Yes = "Sim"))
```

```{r Obesity para fator e definindo labels}
# Variável 'Obesity'
etl.full$Obesity <- factor(etl.full$Obesity, labels = c(No = "Não", Yes = "Sim"))
```

```{r Estrutura dos Dados - Antes}
str(diabetes) # Estrutura do dataset
```

```{r Estrutura dos Dados - Depois}
str(etl.full) # Estrutura do dataset
```

___
### Separando os Datasets

```{r Separando os datasets novamente}
# Dividindo os dados em treino e teste, 70% para dados de treino e 30% para dados de teste
set.seed(7)
rows <- sample(1:nrow(etl.full), 0.7 * nrow(etl.full))
etl.trn <- etl.full[rows,]
etl.tst <- etl.full[-rows,]
```
___
### Removendo Variáveis

```{r Removendo variaveis, eval=FALSE, include=FALSE}
# Removendo variável 'PassengerId'
etl.full$PassengerId <- NULL
etl.trn$PassengerId <- NULL
etl.tst$PassengerId <- NULL

# Removendo variável 'PassengerId'
etl.full$Name <- NULL
etl.trn$Name <- NULL
etl.tst$Name <- NULL

# Removendo a variável 'flag'
etl.full$Cabin <- NULL
etl.trn$Cabin <- NULL
etl.tst$Cabin <- NULL

# Removendo a variável 'flag'
etl.full$idx <- NULL
etl.trn$idx <- NULL
etl.tst$idx <- NULL
```

___
### Resultado da E.T.L.

```{r Exportando ETL}
# write.csv(treino.etl, "../datasets/treino_etl.csv", row.names = FALSE)
dput(etl.trn, file = "datasets/etl_trn.R")
dput(etl.tst, file = "datasets/etl_tst.R")
dput(etl.full, file = "datasets/etl_full.R")
```

___
# Análise Exploratória

A ideia de uma análise descritiva de dados é tentar responder as seguintes
questões:

1. Qual a ***frequência*** com que cada valor (ou intervalo de valores) aparece no conjunto de dados. Ou seja, qual a distribuição de frequências dos dados?

2. Quais são alguns ***valores típicos*** do conjunto de dados, como mínimo e máximo?

3. Qual seria um valor para representar a ***posição central*** do conjunto de dados?

4. Qual seria uma medida da ***variabilidade*** ou dispersão dos dados?

5. Existem ***valores atípicos*** ou discrepantes (outliers) no conjunto de dados?

6. Os dados são simétricos? Qual a ***simetria*** dos dados?

```{r Importando Treino ETL}
aed.trn <- dget(file = "datasets/etl_trn.R")
```
 
___
## Classificando as Variáveis

Uma boa forma de iniciar uma análise descritiva adequada é verificar os tipos de variáveis disponíveis.

```{r Variáveis Disponíveis}
# Qualitativas Nominais
var.fct <- c()
for(i in 1:ncol(aed.trn)){
    if(is.factor(aed.trn[,i]) && !is.ordered(aed.trn[,i]))
    {
        var.fct = c(var.fct, names(aed.trn)[i])
    }
}

# Qualitativas Ordinais
var.fct_ord <- c()
for(i in 1:ncol(aed.trn)){
  i = 1
    if(is.factor(aed.trn[,i]) && is.ordered(aed.trn[,i]))
    {
        var.fct_ord = c(var.fct_ord, names(aed.trn)[i])
    }
}

# Quantitativas Discretas
var.num <- c()
for(i in 1:ncol(aed.trn)){
    if(is.integer(aed.trn[,i]))
    {
        var.num <- c(var.num, names(aed.trn)[i])
    }
}

# Quantitativas Contínuas
var.num_con <- c()
for(i in 1:ncol(aed.trn)){
    if(typeof(aed.trn[,i]) == "double")
    {
        var.num_con <- c(var.num_con, names(aed.trn)[i])
    }
}

data.frame("Qualitativas_Nominais - " = var.fct)

data.frame("Qualitativas_Ordinais - " = var.fct_ord)

data.frame("Quantitativas_Discretas - " = var.num)

data.frame("Quantitativas_Contínuas - " = var.num_con)
```

___
## Análise Univariada

```{r message=FALSE, warning=FALSE}
describe(aed.trn )
```


___
### Distribuições de Frequências

A primeira tarefa de uma análise estatística de um conjunto de dados consiste em resumí-los. As técnicas disponíveis para essa finalidade dependem dos tipos de variáveis envolvidas.

___
#### Variáveis Qualitativas

As distribuições podem ser representadas por meio de:

  - Gráfico de Barras
  
  - Gráfico do tipo Pizza

___
> (@) **Distribuições de Frequências para as Variáveis Qualitativas Nominais**

```{r Dist. Freq. Nominais}
# Distribuição de Frequências para as Variáveis Qualitativas Nominais
for(i in var.fct){
      fdt_cat(aed.trn[,i]) %>%
          tibble() -> df
    names(df)[1] = c(colnames(aed.trn[i]))
    print(kable(df[order(df[2], decreasing = TRUE),]))
}
```

___
> (@) **Distribuições de Frequências para as Variáveis Qualitativas Ordinais**

```{r Dist. Freq. Ordinais}
# Distribuição de Frequências para as Variáveis Qualitativas Ordinais
for(i in var.fct_ord){
      fdt_cat(aed.trn[,i]) %>%
          tibble() -> df
    names(df)[1] = c(colnames(aed.trn[i]))
    print(kable(df[order(df[2], decreasing = TRUE),]))
}
```

___
> (@) **Gráfico de Barras: Variáveis Qualitativas**

```{r Barras: Variáveis Categóricas}
# Gráficos de barras para variáveis categóricas
par(mfrow=c(2,2), cex = 0.55)
for(i in c(var.fct, var.fct_ord)){
        bp <- barplot(table(aed.trn[i]), 
                      main = names(aed.trn[i]),
                      ylim = c(0, max(table(aed.trn[i]))*1.4),
                      col = "green")
        text(x = as.vector(bp),
            y = table(aed.trn[,i]) + 2,
            label =  round(table(aed.trn[i]), 1), 
            pos = 3,
            col = "black")
}
```

___
#### Variáveis Quantitativas

- Discretas

- Contínuas

  > Agrupam-se os valores das variáveis em classes e obtêm-se as frequências em cada classe.

As distribuições podem ser representadas por meio de:

- Gráfico de Dispersão Unidimensional (dotplot)

- Gráfico Ramo e Folhas (Steam and Leaf)

- Histograma

> (@) **Distribuição de Frequências: Variáveis Quantitativas**

```{r Dist. Freq. Continuas}
par(mfrow=c(2,1))
# Distribuição de Frequências: Variáveis Quantitativas
for(i in var.num){
        df <- fdt(aed.trn[,i])
        #names(df)[[1]] = c(colnames(aed.trn[i]))
        print(kable(df, caption = "Title of the table"))
}
```

___
##### Gráficos

```{r Dispersao Unidimensional}
par(mfrow=c(1,1))
# Gráfico de Dispersão Unidimensional: Variáveis Quantitativas
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        dotchart(aed.trn[,i], main = paste("Dispersão Unidimensional:", names(aed.trn)[i]))
    }
} 
```

```{r Ramo e Folhas}
# Gráfico Ramo e Folhas: Variáveis Quantitativas
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        stem(aed.trn[,i])
    }
}
```

```{r Histograma: Quantitativas}
# Gráfico Histograma: Varáveis Quantitativas
# par(mfrow=c(1,2))
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        
        hist(aed.trn[,i], 
             main = paste("Histograma\n", names(aed.trn)[i]),
             xlab = names(aed.trn)[i],
             #ylim = c(0, max(table(aed.trn[,"Age"]))*2.5),
             labels = TRUE,
             nclass = round(1+3.22*log10(nrow(as.array(aed.trn[,i])))) )
    }
} 
```

___
### Medidas de Posição

São as estatísticas que representam uma série de dados orientando-nos quanto à posição da distribuição em relação ao eixo horizontal (eixo "x") do gráfico da curva de freqüência. As medidas de posição mais importantes são as medidas de tendência central, no qual se verifica uma tendência dos dados observados a se agruparem em torno dos valores centrais.

```{r Sumario_Estatistico}
  stargazer(aed.trn, median = T, mean.sd = T, iqr = T, type = "text", title = "Sumário Estatístico")
```

___
***Moda***

Moda - A moda é especialmente útil para dados qualitativos. Não é possível analisar a média ou mediana de dados não ordenados, como cidade ou preferência musical. Então a moda entra em ação.
```{r Moda}
n = c()
m = c()
# Moda
for(i in 1:ncol(aed.trn)){
    if(is.factor(aed.trn[,i])){
        n = c(n, names(aed.trn)[i])
        m = c(m, moda(aed.trn[,i]))
    }
}
kable(data.frame("Variável" = n, "Moda" = m), align = "l")
```

### Medidas Separatrizes

___
> **Quartis**

```{r Quartis}
# Quartis
# par(mfrow=c(2,2))
#df <- data.frame()
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        q <- data.frame(
                quantile(aed.trn[,i])
              )
        names(q)[length(q)] <- names(aed.trn)[i]
        print(q)
    }
}
```

___
> **Quintis**

```{r Quintis}
# Quintis
par(mfrow=c(2,2))
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        quintis = seq(.2, .8, .2)
        q <- data.frame(quantile(aed.trn[,i], quintis))
        names(q) <- names(aed.trn)[i]
        print(q)
    }
}
```

___
> **Decis**

```{r Decis}
# Decis
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        decis = seq(.1, .9, .1)
        q <- data.frame(quantile(aed.trn[,i], decis))
        names(q) <- names(aed.trn)[i]
        print(q)
    }
}
```

___
> **Percentis**

```{r Percentis}
# Percentis
par(mfrow=c(2,2))
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        percentis = seq(.01, .99, .01)
        q <- data.frame(quantile(aed.trn[,i], percentis))
        names(q) <- names(aed.trn)[i]
        print(q)
    }
}
```

___
> **Boxplot**

O boxplot é um gráfico baseado nos quantis que serve como alternativa ao histograma para resumir a distribuição das dados.
Esse gráfico permite que identifiquemos a posição dos 50% centrais dos dados (entre o primeiro e terceiro quartis), a posição da mediana, os valores atípicos, se existirem, assim como permite uma avaliação da simetria da distribuição. Boxplots são úteis para a comparação de vários conjuntos de dados.

```{r Boxplot}
# Boxplot
# par(mfrow=c(2,2))
aed.trn$previsao = NULL
aed.trn$index = NULL
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        boxplot(summary(aed.trn[,i]), 
                ylab = names(aed.trn)[i], 
                main = paste("Boxplot", names(aed.trn)[i]))
    }
}
```

___
### Medidas de Dispersão

As medidas de dispersão são a amplitude total, a variância, o desvio-padrão e o coeficiente de variação.

___
> **Amplitude Total**

É a diferença entre o maior e menor dos valores da série.
A utilização da amplitude total como medida de dispersão é muito limitada, pois é uma medida que depende apenas dos valores extremos, não sendo afetada pela variabilidade interna dos valores da série.
```{r Amplitude Total}
# Amplitude Total
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
      amp <- diff(range(aed.trn[,i]))
      print(amp)
    }
}

```

___
> **Variância**

    A variância é a medida de dispersão mais empregada geralmente, pois leva em consideração a totalidade dos valores da variável em estudo. Baseia-se nos desvios em torno da média aritmética, sendo um indicador de variabilidade.
    
    Para medir o grau de variabilidade dos valores em torno da média, nada mais interessante do que estudarmos o comportamento dos desvios de cada valor individual da série em relação à média.
    
    Queremos calcular a média dos desvios, porém sua soma pode ser nula. Como solução a esse problema a variância considera o quadrado de cada desvio evitando com isso que o somatório seja nulo.

```{r Variancia}
# Variância
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        print(
            var(aed.trn[,i])
            )
    }
}
```

___
> **Desvio Padrão**

    Seguindo a mesma linha de raciocínio usado para o cálculo da variância, necessitamos, agora, aproximar a medida de dispersão da variável original. Para isso, calculamos o desvio padrão, que é a raiz quadrada da variância.
    
    Podemos representar o desvio padrão por uma distribuição normal:
    
    - 68,26% das ocorrências se concentrarão na área do gráfico demarcada por um
    desvio padrão à direita e um desvio padrão à esquerda da linha média;
    
    - 95,44% das ocorrências estão a dois desvios padrão, para a direita e a esquerda da média e, finalmente;
    
    - 99,72% das ocorrências ocorrem a três desvios padrão ao redor da média aritmética.

```{r Desvio Padrão}
# Desvio Padrão
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        print(
            sd(aed.trn[,i])
        )
    }
}
```

- **Coeficiente de Variação**

    Trata-se de uma medida relativa de dispersão, útil para a comparação em termos relativos do grau de concentração em torno da média de séries distintas.

    A importância de se estudar o coeficiente de variação se dá, pois o desvio-padrão é relativo à média. E como duas distribuições podem ter médias diferentes, o desvio destas distribuições não é comparável. Logo, o coeficiente de variação é muito utilizado para comparação entre amostras.

```{r Coeficiente de Variação}
# Coeficiente de Variação
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        cv <- 100*sd(aed.trn[,i]/mean(aed.trn[,i]))
        print(cv)
    }
}
```

___
### Medidas de Assimetria e Achatamento

- Assimetria

    Duas distribuições podem se diferenciar uma da outra em termos de assimetria ou achatamento, ou de ambas. A assimetria e o achatamento têm importância devido a hipótese de que populações são distribuídas normalmente.
    
    A assimetria de Pearson, é baseada nas relações entre a média, mediana e moda. Essas três medidas são idênticas em valor para uma distribuição unimodas simétria, mas, para uma distribuição assimétrica, a média distancia-se da moda situando-se a mediana em uma posição intermediária, à medida que aumenta a assimetria da distribuição. 
    
    Consequentemente, a distância entre a média e a moda poderia ser usada para medir a assimetria.

- Achatamento

    Curtose é o grau de achatamento em uma distribuição de frequência que têm apenas uma moda, ou seja, uma unimodal, em relação à normal. Mede o agrupamento de valores em torno do centro. Quanto maior esse agrupamento, maior será o valor da curtose.
    
    Os tipos de curtose são:
    
    - Leptocúrtica
    
    - Mesocúrtica
    
    - Platicúrtica
    
    A curtose analisa a curva de frequência de forma vertical, relacionando a sua característica com a característica de uma distribuição normal.

```{r Histograma}
#par(mfrow=c(2,2))
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        
        datasim <- data.frame(aed.trn[,i])
        g <- ggplot(datasim, aes(x = aed.trn[,i]), binwidth = 2) + 
          geom_histogram(aes(y = ..density..), fill = 'red', alpha = 0.5) + 
          geom_density(colour = 'blue') + xlab(expression(bold('Dados'))) + 
          ylab(expression(bold('Densidade')))
        
        print(g)
        
    }
}
```

```{r Coeficiente de Assimetria (Skew)}
# Coeficiente de Assimetria (Skew)
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        ca <- skewness(aed.trn[,i])
        print(ca)
    }
}
```

```{r Coeficiente de Curtose}
# Coeficiente de Curtose
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        ck <- kurtosis(aed.trn[,i])
        print(ck)
    }
}
```

___
## Análise Bivariada

O objetivo principal das análises  nessa  situação  é ***explorar  relações*** (similaridades) ***entre as colunas***, ou algumas vezes ***entre as linhas*** através da ***distribuição conjunta das frequências***.

Em  algumas  situações,  podem  ter  dois  ou  mais conjuntos  de  dados 
provenientes  da  observação  da  mesma  variável.

- Quando se considera duas variáveis ou dois conjuntos de dados, existem três situações:

  - As duas variáveis são qualitativas; 
  
  - As duas variáveis são quantitativas; e 
  
  - Uma variável é qualitativa e a outra é quantitativa.

___
### Duas Categóricas

Quando as variáveis são qualitativas, os dados são resumidos em ***tabelas de dupla entrada*** (ou de ***contingência***), onde aparecerão as frequências absolutas. Pode-se ainda, adicionar o total por linha e o total por coluna. As distribuições assim obtidas são chamadas tecnicamente de ***distribuições marginais***.

Em vez de se trabalhar com frequências absolutas, constrói-se tabelas com frequências relativas. Porém, existem três possibilidades de se expressar as frequências relativas de cada casela (célula):

1. Em relação ao total geral;

2. Em relação ao total de cada linha; e,

3. Em relação ao total de cada coluna.

___
#### Tabelas de Contingência

A função *CrossTable* implementa uma tabela de tabulação cruzada, com testes de qui-quadrado, Fisher e McNemar de independência de todos os fatores da tabela. Seu resultado já apresenta as três possibilidades de frequências relativas.

***Qui Quadrado*** é um teste de hipótese que encontra um ***valor de dispersão para duas variáveis nominais***. Compara proporções, divergências entre frequências observadas e esperadas para um certo evento.

```{r Tabulação Cruzada + Qui-Quadrado}
par(mfrow=c(1,2))
for(i in var.fct[-1]){
  CrossTable(aed.trn$class, 
             aed.trn[,i],
             prop.t = T,
             chisq = T,
             digits = 2,
             dnn = c("class", i))
  }
```

___
### Duas Numéricas

É útil identificar se existe uma *associação linear* entre duas variáveis ou entre mais de duas variáveis e, se apropriado, *quantificar a associação*.

Um dispositivo bastante útil para se verificar a *associação* entre duas variáveis quantitativas, ou entre dois conjuntos de dados, é o ***diagrama de dispersão***.

Sua associação pode ser *quantificada* utilizando-se uma medida estatística chamada ***coeficiente de correlação*** ou *grau de associação*. 

___
#### Diagramas de Dispersão

A ***relação*** entre as variáveis pode ser *fortemente linear*, *não linear* ou mesmo *inexistente*. Portanto, um diagrama de dispersão ***é uma primeira indicação*** útil da possível existência ***de uma associação entre duas variáveis***.

```{r Matriz de Correlação}
# Diagramas de Dispersão - Matriz de Correlação
plot(aed.trn[])
```

___
#### Coeficiente de Correlação

Mede a ***força*** de associação entre duas variáveis. Essa medição leva em consideração a dispersão entre os valores dados. Quanto menos dispersos estiverem os dados, mais forte será a dependência, isto é, a associação entre as variáveis.

O coeficiente de correlação “R” assume um valor entre [– 1 e + 1], isto é: 

  - Se r = 1, a correlação é positiva perfeita; 
  
  - Se r = -1, a correlação é negativa perfeita; 
  
  - Se r = 0, a correlação é nula. 


```{r Tabela dos Coeficientes de Correlação}
# Tabela dos Coeficientes de Correlação
cor(aed.trn[var.num])
```

```{r Gráfico dos Coeficientes de Correlação, eval=FALSE, include=FALSE}
# Gráfico dos Coeficientes de Correlação
cor.plot(aed.trn[,c(var.num, var.num_con)])
```

___
#### Covariância

```{r Tabela de Covariâncias, eval=FALSE, include=FALSE}
cov(aed.trn[,c(var.num, var.num_con)])
```

```{r Diagramas de Dispersão + Coeficientes de Correlação, eval=FALSE, include=FALSE}
# Diagramas de Dispersão + Coeficientes de Correlação
pairs.panels(aed.trn[,c(var.num, var.num_con)])
```

___
### Uma Númerica e outra Categórica

Em geral ***analisa-se o que acontece com a variável quantitativa dentro de cada categoria da variável qualitativa.***. Essa análise pode ser conduzida por meio de ***medidas-resumo*** ou ***box plot***.

As medidas-resumo são agrupadas por categoria da variável qualitativa. A partir daí, constroem-se boxplots baseados em cada medida-resumo. Então, os boxplots são comparados visualmente. 

___
#### Medidas-resumo

*As medidas-resumo são calculadas para a variável quantitativa, a variável que se quer observar o comportamento.*

___
#### Boxplot

```{r Gráficos de Caixa Age x Categóricas, fig.width = 12, fig.height = 5}
# Boxplots Idade conforme as Categóricas
par(mfrow=c(1,3), cex = 0.65)
for(i in var.fct){
  boxplot(aed.trn$Age ~ aed.trn[,i], beside = TRUE, xlab = names(aed.trn)[grep(i, names(aed.trn))])
}
```

```{r Gráficos de Caixa Numéricas x Survived, fig.width = 7, fig.height = 8}
# Boxplots Numéricas conforme as Survived
# par(mfrow=c(2,2))
for(i in 0:length(aed.trn)){
  if(is.numeric(aed.trn[,i])){
    boxplot(aed.trn[,i] ~ class, ylab = names(aed.trn)[i], data = aed.trn)
  }
}
```

- Intervalo Interquartil (IQ): Diferença entre o 3º e o 1º quartil. O comprimento do lado vertical.

- Limite Inferior (LI): *Q1 - 1,5IQ*

- Limite Superior (LS): *Q3 + 1,5IQ*

- Outliers: Um dado será considerado outlier se ele for *menor que o* ***LI*** ou *maior que o* ***LS***.

  - O que fazer com os outliers:
    
    1. Excluir - valores absurdos, p.ex. idade de 180 anos;
    2. Buscar possíveis causas;
    3. Verificar diferenças na análise com e sem o outlier;
    4. Categorizar a variável, p.ex. o outlier entraria em valores maiores que x.

- Informações que podem ser retiradas:

  1. Número de outliers;
  2. Valor mediano aproximado;
  3. Intervalo onde estão 50% dos valores;
  4. Simetria da variável baseada na distância dos quartis até a mediana.


```{r Exporta AED}
dput(aed.trn, file = "datasets/aed_trn.R")
```

___
# Pré-Processamento

___
## Escala e Normalização

Como o intervalo de valores dos dados brutos varia muito, em alguns algoritmos de aprendizado de máquina, as funções objetivas não funcionarão corretamente sem [normalização](https://en.wikipedia.org/wiki/Normalization_(statistics)). Outra razão pela qual o dimensionamento de recursos é aplicado é que a [descida do gradiente](https://en.wikipedia.org/wiki/Gradient_descent) converge muito mais rapidamente com o dimensionamento de recursos do que sem ele.

- Métodos: *[Feature Scaling](https://en.wikipedia.org/wiki/Feature_scaling)*
  
  (@) **Re-escalonar (Normalização Min-Max):** é o método mais simples e consiste em redimensionar o intervalo de recursos para dimensionar o intervalo em [0, 1] ou [-1, 1].
  
  (@) **Escore-Z (Padronização):** No aprendizado de máquina, podemos lidar com vários tipos de dados, por exemplo, sinais de áudio e valores de pixel para dados de imagem, e esses dados podem incluir várias dimensões. 
  *A padronização de recursos faz com que os valores de cada recurso nos dados tenham média zero* (ao subtrair a média no numerador) *e variação de unidade*. Esse método é amplamente utilizado para normalização em muitos algoritmos de aprendizado de máquina (por exemplo, máquinas de vetores de suporte, regressão logística e redes neurais artificiais).
  O método geral de cálculo é determinar a média de distribuição e o desvio padrão para cada recurso. Em seguida, subtraímos a média de cada recurso. Em seguida, dividimos os valores (a média já está subtraída) de cada recurso pelo seu desvio padrão.

### Normalização (Min-Max)

```{r Min-Max}
```

### Padronização (Z-Score)

```{r Z-Score, eval=FALSE, include=FALSE}
z.trn <- dget(file = "datasets/aed_trn.R")
# Padroniza
z.trn[c(var.num, var.num_con)] <- sapply(z.trn[c(var.num, var.num_con)], scale)

z.tst <- dget(file = "datasets/etl_tst.R")
# Padroniza
z.tst[c(var.num, var.num_con)] <- sapply(z.tst[c(var.num, var.num_con)], scale)
```

```{r Exporta Treino e Teste Padronizados, eval=FALSE, include=FALSE}
# Exporta
dput(z.trn, file = "datasets/z_trn.R")
dput(z.tst, file = "datasets/z_tst.R")
```

```{r Apresenta Padronização, eval=FALSE, include=FALSE}
stargazer(aed.trn, type = "text")
stargazer(z.trn, type = "text")
```

## Feature Selection
```{r Feature Selection}
```

# Modelagem

```{r Importa Dados Processados}
#massa <- z.trn
#mdl.tst <- z.tst
massa <- aed.trn
mdl.tst <- etl.tst

```

___
## Amostragem

> **Dividir os dados em Treinamento, Validação e Teste.**

  - ***Conjunto de dados de treinamento:*** a amostra de dados usada para ajustar o modelo.
  
  - ***Conjunto de dados de validação:*** é usado para avaliar um determinado modelo. Usamos esses dados para ajustar os hiperparâmetros do modelo. Portanto, o modelo ocasionalmente vê esses dados, mas nunca “aprende” com eles. Nós usamos os resultados do conjunto de validação e atualizamos os hiperparâmetros de nível superior. Portanto, a validação definida de uma maneira afeta um modelo, mas indiretamente.
  
  - ***Conjunto de dados de teste:*** a amostra de dados usada para fornecer uma avaliação imparcial de um ajuste final do modelo no conjunto de dados de treinamento. Só é usado quando um modelo é completamente treinado (usando os conjuntos de treino e validação). O conjunto de testes geralmente é o que é usado para avaliar modelos concorrentes (por exemplo, em muitas competições Kaggle, o conjunto de validação é lançado inicialmente juntamente com o conjunto de treinamento e o conjunto de testes real é lançado somente quando a competição está prestes a fechar e é o resultado do modelo no conjunto de teste que decide o vencedor).
  Muitas vezes, o conjunto de validação é usado como o conjunto de teste, mas não é uma boa prática. O conjunto de testes geralmente é bem organizado. Ele contém dados cuidadosamente amostrados que abrangem as várias classes que o modelo enfrentaria, quando usado no mundo real.
  
> ***Nota sobre validação cruzada:*** *Muitas vezes, as pessoas primeiro dividem seus conjuntos de dados em 2 - Treinar e Testar. Depois disso, eles deixam de lado o conjunto de testes e escolhem aleatoriamente X% do conjunto de dados de trem para ser o conjunto de trens real e o restante (100-X)% para o conjunto de validação , onde X é um número fixo (por exemplo, 80% ), o modelo é treinado e validado iterativamente nesses diferentes conjuntos. Existem várias maneiras de fazer isso e é comumente conhecido como Validação Cruzada. Basicamente, você usa seu conjunto de treinamento para gerar várias divisões dos conjuntos de treinamento e validação. A validação cruzada evita o ajuste e está se tornando cada vez mais popular, com a Validação Cruzada K-fold sendo o método mais popular de validação cruzada.*

**Fontes:** 

1. *[https://towardsdatascience.com/train-validation-and-test-sets-72cb40cba9e7](https://towardsdatascience.com/train-validation-and-test-sets-72cb40cba9e7)*

2. *[https://machinelearningmastery.com/difference-test-validation-datasets/](https://machinelearningmastery.com/difference-test-validation-datasets/)*

> ***Resampling***

A semente é um número criado a partir do horário atual e do ID do processo, para garantir a aleatoriedade dos resultados. Fixá-lo permite que esses resultados sejam reproduzíveis.

```{r Resampling}
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

___
## Treino
___
> Treinando um ***Modelo Linear Generalizado*** para classificação.

```{r Ajuste G.L.M.}
# Gerar Modelos de Classificação
ajt.trn <- glm(formula = class ~ .,
               family = binomial(link = "logit"), 
               data = mdl.trn)
```

**Resumo do modelo treinado:**

```{r Output G.L.M.}
summary(ajt.trn)
```
___	
## Validação
___
> Validando o modelo através de predições utilizando o *Conjunto de Validação*.

```{r Predição para Validação}
pred <- predict( ajt.trn, 
                     newdata = mdl.vld,
                     type = "response" )

# Criando um dataframe com os dados observados e os preditos.
previsoes <- data.frame(observado = mdl.vld$class,
                        previsto = pred %>% 
                                      round() %>% 
                                      factor(labels = c("Sim", "Não")))
```

```{r Tabela Cruzada - Validação}
CrossTable(previsoes$observado, 
           previsoes$previsto)
```

```{r Qui Quadrado - Validação}
chisq.test(previsoes$observado, 
           previsoes$previsto)
```

```{r, echo=FALSE}
hist(pred, 
     xlab = "Predições", 
     main = "Histograma de Predições")
```


### Avaliação

Para avaliar o desempenho do modelo nos dados de validação, foi construída uma *matriz de confusão*.

```{r Confusion Matrix usando Caret}
cm <- confusionMatrix(previsoes$observado, 
                      previsoes$previsto, 
                      positive = "Sim")
```

Matriz de Confusão - Usando o pacote Caret

```{r Imprime Confusion Matrix}
print(cm)
```  

Accuracy

"Através da *Matriz de Confusão* obtivemos a Acurácia de ***`r round((cm$table[1,1] + cm$table[2,2]) / (cm$table[1,1] + cm$table[1,2] + cm$table[2,1] + cm$table[2,2]) * 100, 2)`%***, ela corresponde a fração das premissas corretas em relação ao total de observações. Esta métrica também poderia ser calculada utilizando a função *table()* ou a função *CrossTable()*, pois ela corresponde a soma das predições corretas dividida pelo total de observações.  
Porém, não é suficiente (e nem segura) para avaliarmos a eficiência do modelo. Analisamos então, outras métricas obtidas pela Confusion Matrix, que nos informe não apenas o percentual de acertos, mas também a precisão e a sensibilidade."

***95% CI***

O ***Intervalo de Confiança***, denotado por *95% CI*, é ***[`r round(cm$overall["AccuracyLower"], 4)`, `r round(cm$overall["AccuracyUpper"], 4)`]***. Ele é uma *estimativa por intervalo* de um *parâmetro populacional*, que com dada frequência (*Nível de Confiança*), inclui o parâmetro de interesse. Nesse caso específico, significa dizer que 95% dos *intervalos de confiança* observados têm o valor real do parâmetro.

***No Information Rate***

***P-Value [Acc > NIR]***

***Kappa***

O ***Teste de Concordância Kappa*** foi igual a ***`r round(cm$overall["Kappa"], 4)`***. O coeficiente de Kappa tem a finalidade de medir o grau de concordância entre proporções. Ele demonstra se uma dada classificação pode ser considerada confiável.

O seu valor pode ser interpretado por meio de uma tabela.

Mcnemar's Test P-Value

Sensitivity

Specificity

Pos Pred Value

Neg Pred Value

Prevalence

Detection Rate

Detection Prevalence

Balanced Accuracy

```{r Plot Confusion Matrix}
fourfoldplot(cm$table)
```

___
O pacote Caret nos dá, já calculadas, as principais métricas estatísticas, baseadas nos tipos de acertos e, nos tipos de erros. Para a *Acurácia*, não havia diferenças, apenas acertos e erros.

Agora, os erros são classificados como ***"Erro do Tipo I"*** e ***"Erro do Tipo II"***. O primeiro é *baseado nos* ***Falsos Positivos (FP)***, o segundo é *baseado nos* ***Falsos Negativos (FN)***.

O **Erro do Tipo I** *(FP)*, é o resultado que tem *Significância Estatística*. A probabilidade de se cometer um *erro do tipo I* em um teste de hitótese é denominada ***Nível de Significância***, representado por ***\(\alpha\) (alfa)***. Sua interpretação é que em *\(\alpha\) vezes* (p.ex. 5%) rejeitaremos a *hipótese nula (H0)* quando ela é verdadeira.  
O **Erro do Tipo II** *(FN)*, é o ***Poder do Teste***, representado por ***\(\beta\) (beta)***. Ocorre quando a hipótese nula é falsa, mas erroneamente falhamos ao ser rejeitada.

Normalmente, ao se testar uma hipótese, é definido o *nível de significância* do *Erro do Tipo I*, chamado de α, tipicamente de 5%. Ou seja, com α = 0,05, existe 5% de chance de se rejeitar a hipótese nula, no caso dela ser verdadeira.   
- Ao se diminuir a probabilidade de ocorrer um *Erro do Tipo I*, ou seja, diminuindo o valor de α, aumenta-se a probabilidade de se ocorrer um *Erro do Tipo II*. A probabilidade da ocorrência do Erro do tipo II é chamada de β.

```{r Mosaico Confusion Matrix}
mosaicplot(cm$table, color = TRUE, shade = TRUE, main = "Mosaico para Confusion Matrix", )
```

A partir destas definições construímos as duas principais métricas, a ***Precisão*** e a ***Sensibilidade***.

A **Precisão** demonstra que de todos os casos previstos como **TP**, quantos realmente estavam certos.  
A **Sensibilidade** mostra que de todos os possíveis casos onde o target era 1, quanto o modelo conseguiu capturar.

Se *diminuírmos o erro do tipo I* (FP), tornaremos nosso modelo ***mais preciso***.  
Se *diminuírmos o erro do tipo II* (FN), tornaremos o modelo ***mais sensível***.  

Para manipular o *Erro Tipo I*...  
Para manipular o *Erro Tipo II* devemos utilizar modelos diferentes ou aumentar o tamanho da amostra.

Para alterarmos esses valores, devemos analizar as predições como uma distribuição de probabilidade, encontrando a probabilidade de cada observação pertencer a classe 1 ou 0. 

O ***Threshold*** é o percentual que definirá essa escolha, o corte que separará as classes.

A ***F1-Score*** é uma alternativa para não utilizarmos a Precisão e a Sensibilidade. Ela é uma média harmônica entre essas duas, o que torna a métrica mais sensível a desproporções.

Outra forma é a ***F-Beta*** onde podemos escolher o Peso entre Precisão e Sensibilidade através do parâmetro Beta.

___
#### Métricas de Avaliação

Avaliar um modelo de Classificação Binária:

*Acurácia (Accuracy)*, *Precisão (Precision)*, *Sensibilidade (Recall)* e *Pontuação F1 (F1-Score)*.

```{r Accuracy}
acuracia <- (cm$table[1,1] + cm$table[2,2])/(cm$table[1,1] + cm$table[1,2] + cm$table[2,1] + cm$table[2,2])

```

```{r Precision}
# Precision
precisao <- cm$table[1,1] / (cm$table[1,1] + cm$table[1,2])
```

```{r Recall}
sensibilidade <- cm$table[1,1] / (cm$table[1,1] + cm$table[2,1])
```

```{r F1-Score}
f1 <- (2 * sensibilidade * precisao) / (sensibilidade + precisao)
```

```{r Tabela de Métricas}
metricas <- kable(
              data.frame(
                "Acuracia" = acuracia, 
                "Precisao" = precisao, 
                "Sensibilidade" = sensibilidade,
                "F1-Score" = f1), format = "markdown", align = 'l')
metricas
```

#### Curva ROC

```{r ROC}
class1 <- predict(ajt.trn, newdata = mdl.vld, type = "response")
class2 <- mdl.vld$class

pred <- prediction(class1, class2)
perf <- performance(pred, "tpr", "fpr")
```

```{r Output ROC, fig.cap = 'Gerando uma curva ROC em R'}
# Gerando uma curva ROC em R
plot(perf, col = rainbow(10, alpha = NULL))
```

___
Realizando o ***Teste Chi-Quadrado*** (X²).
```{r Chi-Quadrado - Validação}
chisq.test(previsoes$observado, previsoes$previsto)
```

___
## Teste

Predição de teste utilizando novos dados.
```{r Predição para Teste}
predict(ajt.trn, 
        newdata = mdl.tst, 
        type = "response") %>% 
  round() %>% 
  factor(labels = c("Não", "Sim")) -> predicao
```

Tabela para as predições de teste.
```{r Tabela de Proporções - Teste}
CrossTable(predicao, prop.t = T, digits = 2)
```
