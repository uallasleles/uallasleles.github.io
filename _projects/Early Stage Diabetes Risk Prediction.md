---
name: Early_Stage_Diabetes_Risk_Prediction
tools: [Support, Author, VVG]
image:
description: Early_Stage_Diabetes_Risk_Prediction
---

Diretório de Trabalho:

    ## [1] "C:/Users/ulp/Documents/GitHub/proj_20210619-Early_Stage_Diabetes_Risk_Prediction"

# Dependências

## Pacotes

## Funções

``` r
source(file = "scripts/functions.R")
```

# Definição do Problema

------------------------------------------------------------------------

**Definição:** *Early stage diabetes risk prediction dataset.. (2020).
UCI Machine Learning Repository.*

**Dataset:** *This dataset contains the sign and symptpom data of newly
diabetic or would be diabetic patient.* *Early stage diabetes risk
prediction dataset.. (2020). UCI Machine Learning Repository.*

**Link:** [UCI: Early stage diabetes risk prediction
dataset.](https://archive-beta.ics.uci.edu/ml/datasets/529)

**Objetivo:** *Prever o risco de diabetes no estágio inicial.*

------------------------------------------------------------------------

# Planejamento

------------------------------------------------------------------------

1.  *Coleta de dados com StringAsFactor como false.*
2.  *Limpeza e transformação das variáveis.*
3.  *Dividir o conjunto de dados.*
4.  *Fazer a Análise Exploratória do conjunto de treino.*

------------------------------------------------------------------------

# Coleta

------------------------------------------------------------------------

## Extração

``` r
source(file = "scripts/ds_load.R")
```

------------------------------------------------------------------------

### Pré-visualização dos Dados

``` r
# TREINO: Pré-visualização das primeiras observações:
etl.full %>% 
    head() %>% 
    tibble(format = "markdown", caption = "Primeiras observações")
```

    ## # A tibble: 6 x 19
    ##     Age Gender Polyuria Polydipsia sudden.weight.loss weakness Polyphagia
    ##   <int> <chr>  <chr>    <chr>      <chr>              <chr>    <chr>     
    ## 1    40 Male   No       Yes        No                 Yes      No        
    ## 2    58 Male   No       No         No                 Yes      No        
    ## 3    41 Male   Yes      No         No                 Yes      Yes       
    ## 4    45 Male   No       No         Yes                Yes      Yes       
    ## 5    60 Male   Yes      Yes        Yes                Yes      Yes       
    ## 6    55 Male   Yes      Yes        No                 Yes      Yes       
    ## # ... with 12 more variables: Genital.thrush <chr>, visual.blurring <chr>,
    ## #   Itching <chr>, Irritability <chr>, delayed.healing <chr>,
    ## #   partial.paresis <chr>, muscle.stiffness <chr>, Alopecia <chr>,
    ## #   Obesity <chr>, class <chr>, format <chr>, caption <chr>

``` r
print(etl.full)
```

    ##     Age Gender Polyuria Polydipsia sudden.weight.loss weakness Polyphagia
    ## 1    40   Male       No        Yes                 No      Yes         No
    ## 2    58   Male       No         No                 No      Yes         No
    ## 3    41   Male      Yes         No                 No      Yes        Yes
    ## 4    45   Male       No         No                Yes      Yes        Yes
    ## 5    60   Male      Yes        Yes                Yes      Yes        Yes
    ## 6    55   Male      Yes        Yes                 No      Yes        Yes
    ## 7    57   Male      Yes        Yes                 No      Yes        Yes
    ## 8    66   Male      Yes        Yes                Yes      Yes         No
    ## 9    67   Male      Yes        Yes                 No      Yes        Yes
    ## 10   70   Male       No        Yes                Yes      Yes        Yes
    ## 11   44   Male      Yes        Yes                 No      Yes         No
    ## 12   38   Male      Yes        Yes                 No       No        Yes
    ## 13   35   Male      Yes         No                 No       No        Yes
    ## 14   61   Male      Yes        Yes                Yes      Yes        Yes
    ## 15   60   Male      Yes        Yes                 No      Yes        Yes
    ## 16   58   Male      Yes        Yes                 No      Yes        Yes
    ## 17   54   Male      Yes        Yes                Yes      Yes         No
    ## 18   67   Male       No        Yes                 No      Yes        Yes
    ## 19   66   Male      Yes        Yes                 No      Yes        Yes
    ## 20   43   Male      Yes        Yes                Yes      Yes         No
    ## 21   62   Male      Yes        Yes                 No      Yes        Yes
    ## 22   54   Male      Yes        Yes                Yes      Yes        Yes
    ## 23   39   Male      Yes         No                Yes       No         No
    ## 24   48   Male       No        Yes                Yes      Yes         No
    ## 25   58   Male      Yes        Yes                Yes      Yes        Yes
    ## 26   32   Male       No         No                 No       No         No
    ## 27   42   Male       No         No                 No      Yes        Yes
    ## 28   52   Male      Yes        Yes                Yes      Yes        Yes
    ## 29   38   Male       No        Yes                 No       No         No
    ## 30   53   Male      Yes        Yes                Yes      Yes        Yes
    ## 31   57   Male      Yes        Yes                Yes      Yes        Yes
    ## 32   41   Male      Yes        Yes                Yes      Yes        Yes
    ## 33   37   Male       No         No                 No      Yes         No
    ## 34   54   Male      Yes        Yes                Yes      Yes         No
    ## 35   49   Male      Yes        Yes                 No      Yes         No
    ## 36   48   Male      Yes        Yes                 No      Yes         No
    ## 37   60   Male      Yes         No                 No       No         No
    ## 38   63   Male      Yes        Yes                Yes      Yes        Yes
    ## 39   35   Male      Yes         No                Yes       No         No
    ## 40   30 Female      Yes         No                Yes      Yes        Yes
    ## 41   53 Female       No        Yes                Yes       No        Yes
    ## 42   50 Female      Yes        Yes                Yes       No        Yes
    ## 43   50 Female       No        Yes                 No      Yes        Yes
    ## 44   35 Female      Yes        Yes                Yes      Yes        Yes
    ## 45   40 Female      Yes        Yes                Yes      Yes         No
    ## 46   48 Female       No         No                Yes      Yes         No
    ## 47   60 Female      Yes        Yes                Yes      Yes        Yes
    ## 48   60 Female      Yes        Yes                Yes      Yes        Yes
    ## 49   35 Female      Yes        Yes                Yes      Yes        Yes
    ## 50   46 Female      Yes         No                Yes      Yes         No
    ## 51   36 Female      Yes        Yes                Yes       No        Yes
    ## 52   50 Female      Yes        Yes                Yes      Yes        Yes
    ## 53   60 Female       No        Yes                Yes      Yes        Yes
    ## 54   50 Female       No        Yes                Yes      Yes         No
    ## 55   51 Female       No         No                 No       No         No
    ## 56   38 Female       No         No                 No       No         No
    ## 57   66 Female      Yes        Yes                Yes      Yes        Yes
    ## 58   53 Female       No         No                 No       No        Yes
    ## 59   59 Female       No         No                 No       No         No
    ## 60   39 Female      Yes        Yes                 No      Yes        Yes
    ## 61   65 Female      Yes        Yes                 No      Yes        Yes
    ## 62   35 Female      Yes        Yes                Yes      Yes        Yes
    ## 63   55 Female      Yes        Yes                 No      Yes        Yes
    ## 64   60 Female      Yes         No                Yes      Yes         No
    ## 65   45 Female       No         No                 No       No         No
    ## 66   40 Female      Yes         No                Yes       No        Yes
    ## 67   30 Female      Yes        Yes                 No       No         No
    ## 68   35 Female       No        Yes                Yes      Yes        Yes
    ## 69   25 Female       No         No                 No      Yes        Yes
    ## 70   50 Female      Yes        Yes                Yes      Yes        Yes
    ## 71   40 Female       No         No                Yes      Yes         No
    ## 72   35 Female       No        Yes                Yes      Yes         No
    ## 73   65 Female       No         No                 No       No         No
    ## 74   38 Female      Yes        Yes                Yes      Yes        Yes
    ## 75   50 Female      Yes        Yes                Yes      Yes        Yes
    ## 76   55 Female      Yes        Yes                Yes      Yes         No
    ## 77   48 Female      Yes        Yes                Yes      Yes        Yes
    ## 78   55 Female      Yes        Yes                Yes       No        Yes
    ## 79   39 Female      Yes        Yes                Yes      Yes        Yes
    ## 80   43 Female      Yes        Yes                Yes      Yes        Yes
    ## 81   35 Female      Yes        Yes                 No      Yes         No
    ## 82   47 Female       No         No                Yes      Yes        Yes
    ## 83   50 Female      Yes        Yes                 No      Yes        Yes
    ## 84   48 Female      Yes        Yes                 No      Yes         No
    ## 85   35 Female      Yes        Yes                Yes      Yes        Yes
    ## 86   49 Female       No         No                Yes      Yes         No
    ## 87   38 Female      Yes        Yes                Yes      Yes        Yes
    ## 88   28 Female       No         No                 No       No         No
    ## 89   68 Female      Yes        Yes                 No      Yes        Yes
    ## 90   35 Female       No         No                 No       No         No
    ## 91   45 Female       No         No                 No       No        Yes
    ## 92   48 Female      Yes        Yes                Yes      Yes        Yes
    ## 93   40 Female      Yes        Yes                Yes      Yes         No
    ## 94   40 Female      Yes        Yes                 No      Yes        Yes
    ## 95   36 Female      Yes        Yes                 No      Yes        Yes
    ## 96   56 Female      Yes        Yes                 No      Yes         No
    ## 97   30 Female      Yes        Yes                Yes      Yes         No
    ## 98   31 Female      Yes        Yes                Yes      Yes        Yes
    ## 99   35 Female      Yes        Yes                 No       No         No
    ## 100  39 Female      Yes        Yes                 No       No        Yes
    ## 101  48 Female      Yes        Yes                Yes       No        Yes
    ## 102  85   Male      Yes        Yes                Yes      Yes        Yes
    ## 103  90 Female       No        Yes                Yes       No         No
    ## 104  72   Male      Yes         No                Yes      Yes        Yes
    ## 105  70   Male      Yes         No                Yes      Yes        Yes
    ## 106  69 Female      Yes         No                Yes      Yes        Yes
    ## 107  58   Male       No        Yes                Yes      Yes        Yes
    ## 108  47   Male      Yes         No                Yes      Yes        Yes
    ## 109  25   Male      Yes        Yes                 No       No        Yes
    ## 110  39 Female      Yes        Yes                 No       No        Yes
    ## 111  53 Female       No         No                Yes      Yes         No
    ## 112  52   Male      Yes        Yes                Yes       No        Yes
    ## 113  68 Female      Yes        Yes                Yes       No        Yes
    ## 114  79   Male       No        Yes                Yes      Yes        Yes
    ## 115  55 Female      Yes         No                Yes       No         No
    ## 116  45 Female      Yes        Yes                Yes      Yes        Yes
    ## 117  30 Female      Yes        Yes                Yes       No        Yes
    ## 118  45 Female      Yes        Yes                 No      Yes        Yes
    ## 119  65 Female      Yes        Yes                 No      Yes         No
    ## 120  34 Female      Yes        Yes                 No      Yes         No
    ## 121  48   Male      Yes        Yes                 No      Yes        Yes
    ## 122  35   Male      Yes        Yes                 No      Yes        Yes
    ## 123  40   Male       No        Yes                 No      Yes        Yes
    ## 124  47   Male       No        Yes                 No       No         No
    ## 125  38   Male      Yes         No                Yes      Yes         No
    ## 126  55   Male      Yes        Yes                Yes      Yes        Yes
    ## 127  66   Male      Yes        Yes                 No      Yes         No
    ## 128  57   Male      Yes         No                 No      Yes        Yes
    ## 129  32   Male       No        Yes                 No      Yes        Yes
    ## 130  48   Male      Yes        Yes                Yes      Yes         No
    ## 131  47   Male      Yes        Yes                Yes      Yes        Yes
    ## 132  43   Male      Yes         No                 No       No        Yes
    ## 133  30   Male      Yes        Yes                Yes      Yes         No
    ## 134  16   Male      Yes         No                Yes       No        Yes
    ## 135  35   Male      Yes        Yes                Yes      Yes         No
    ## 136  66   Male       No         No                 No      Yes         No
    ## 137  54   Male       No         No                 No       No         No
    ## 138  58   Male      Yes        Yes                Yes      Yes        Yes
    ## 139  51   Male      Yes        Yes                Yes      Yes         No
    ## 140  40   Male      Yes         No                 No       No         No
    ## 141  47   Male      Yes        Yes                 No       No         No
    ## 142  62   Male      Yes        Yes                 No       No        Yes
    ## 143  49   Male      Yes        Yes                Yes       No         No
    ## 144  53   Male      Yes         No                Yes       No         No
    ## 145  68   Male      Yes        Yes                 No       No        Yes
    ## 146  61   Male      Yes         No                 No      Yes        Yes
    ## 147  39   Male      Yes        Yes                 No       No         No
    ## 148  38   Male      Yes         No                 No       No         No
    ## 149  44   Male       No        Yes                 No       No         No
    ## 150  45   Male      Yes        Yes                 No      Yes         No
    ## 151  50   Male      Yes        Yes                Yes      Yes        Yes
    ## 152  42   Male      Yes        Yes                 No      Yes         No
    ## 153  55   Male       No        Yes                 No      Yes         No
    ## 154  57   Male       No        Yes                 No      Yes         No
    ## 155  62   Male      Yes         No                 No      Yes         No
    ## 156  33   Male       No        Yes                 No       No         No
    ## 157  55   Male      Yes        Yes                Yes      Yes         No
    ## 158  48   Male      Yes        Yes                Yes       No        Yes
    ## 159  56   Male      Yes         No                Yes      Yes         No
    ## 160  38 Female      Yes        Yes                Yes      Yes        Yes
    ## 161  28 Female       No         No                 No       No         No
    ## 162  68 Female      Yes        Yes                 No      Yes        Yes
    ## 163  35 Female       No         No                 No       No         No
    ## 164  45 Female       No         No                 No       No        Yes
    ## 165  48 Female      Yes        Yes                Yes      Yes        Yes
    ## 166  40 Female      Yes        Yes                Yes      Yes         No
    ## 167  57   Male      Yes        Yes                Yes      Yes        Yes
    ## 168  41   Male      Yes        Yes                Yes      Yes        Yes
    ## 169  37   Male       No         No                 No      Yes         No
    ## 170  54   Male      Yes        Yes                Yes      Yes         No
    ## 171  49   Male      Yes        Yes                 No      Yes         No
    ## 172  48   Male      Yes        Yes                 No      Yes         No
    ## 173  60   Male      Yes         No                 No       No         No
    ## 174  63   Male      Yes        Yes                Yes      Yes        Yes
    ## 175  35   Male      Yes         No                Yes       No         No
    ## 176  30 Female      Yes         No                Yes      Yes        Yes
    ## 177  53 Female       No        Yes                Yes       No        Yes
    ## 178  50 Female      Yes        Yes                Yes       No        Yes
    ## 179  50 Female       No        Yes                 No      Yes        Yes
    ## 180  35 Female      Yes        Yes                Yes      Yes        Yes
    ## 181  40 Female      Yes        Yes                Yes      Yes         No
    ## 182  31 Female      Yes        Yes                Yes      Yes        Yes
    ## 183  35 Female      Yes        Yes                 No       No         No
    ## 184  39 Female      Yes        Yes                 No       No        Yes
    ## 185  48 Female      Yes        Yes                Yes       No        Yes
    ## 186  85   Male      Yes        Yes                Yes      Yes        Yes
    ## 187  90 Female       No        Yes                Yes       No         No
    ## 188  72   Male      Yes         No                Yes      Yes        Yes
    ## 189  70   Male      Yes         No                Yes      Yes        Yes
    ## 190  69 Female      Yes         No                Yes      Yes        Yes
    ## 191  58   Male       No        Yes                Yes      Yes        Yes
    ## 192  54   Male      Yes        Yes                 No       No         No
    ## 193  64   Male       No        Yes                 No       No         No
    ## 194  36   Male      Yes         No                 No      Yes         No
    ## 195  43   Male       No         No                Yes      Yes        Yes
    ## 196  31   Male      Yes         No                 No       No        Yes
    ## 197  66   Male       No         No                 No       No        Yes
    ## 198  61 Female      Yes         No                 No       No        Yes
    ## 199  58 Female      Yes         No                Yes       No        Yes
    ## 200  69 Female      Yes        Yes                Yes      Yes         No
    ## 201  40   Male       No        Yes                Yes      Yes         No
    ## 202  28   Male       No         No                Yes       No         No
    ## 203  37   Male       No         No                 No       No         No
    ## 204  34   Male       No         No                 No       No         No
    ## 205  30   Male       No         No                 No       No         No
    ## 206  67   Male      Yes         No                 No      Yes        Yes
    ## 207  60   Male       No         No                 No      Yes         No
    ## 208  58   Male       No         No                 No       No        Yes
    ## 209  54   Male       No         No                Yes      Yes         No
    ## 210  43   Male       No         No                Yes       No         No
    ## 211  39   Male       No         No                 No      Yes         No
    ## 212  40   Male       No         No                 No      Yes         No
    ## 213  43   Male       No         No                 No       No         No
    ## 214  49   Male       No        Yes                 No       No         No
    ## 215  47   Male       No         No                 No       No         No
    ## 216  45   Male       No         No                 No       No        Yes
    ## 217  57   Male       No         No                 No       No        Yes
    ## 218  72   Male      Yes         No                 No       No        Yes
    ## 219  30   Male       No         No                 No       No         No
    ## 220  27   Male       No         No                 No       No         No
    ## 221  38   Male       No         No                 No       No         No
    ## 222  43   Male       No         No                 No      Yes         No
    ## 223  40   Male       No         No                Yes       No         No
    ## 224  55   Male       No         No                 No      Yes        Yes
    ## 225  68   Male       No         No                 No      Yes        Yes
    ## 226  29   Male       No         No                 No      Yes         No
    ## 227  37   Male       No         No                 No       No         No
    ## 228  30   Male       No         No                Yes      Yes         No
    ## 229  45   Male       No         No                 No      Yes         No
    ## 230  47   Male       No         No                 No       No         No
    ## 231  35   Male       No         No                Yes      Yes         No
    ## 232  32   Male       No         No                 No      Yes         No
    ## 233  56   Male       No        Yes                 No      Yes        Yes
    ## 234  50   Male       No         No                 No      Yes         No
    ## 235  52   Male       No         No                 No      Yes        Yes
    ## 236  26   Male       No         No                 No       No         No
    ## 237  60   Male       No         No                 No      Yes        Yes
    ## 238  65   Male       No         No                 No      Yes         No
    ## 239  72   Male       No         No                 No       No        Yes
    ## 240  30   Male       No         No                 No      Yes         No
    ## 241  45   Male       No         No                 No      Yes         No
    ## 242  65   Male      Yes         No                 No       No         No
    ## 243  70   Male       No         No                 No      Yes         No
    ## 244  35   Male       No         No                 No      Yes         No
    ## 245  54   Male       No         No                 No      Yes         No
    ## 246  30   Male       No         No                 No       No         No
    ## 247  46   Male       No         No                 No      Yes         No
    ## 248  53   Male       No         No                 No      Yes        Yes
    ## 249  42   Male       No         No                 No       No         No
    ## 250  55 Female      Yes        Yes                Yes      Yes         No
    ## 251  48 Female      Yes        Yes                Yes      Yes        Yes
    ## 252  55 Female      Yes        Yes                Yes       No        Yes
    ## 253  39 Female      Yes        Yes                Yes      Yes        Yes
    ## 254  43 Female      Yes        Yes                Yes      Yes        Yes
    ## 255  35 Female      Yes        Yes                 No      Yes         No
    ## 256  47 Female       No         No                Yes      Yes        Yes
    ## 257  50 Female      Yes        Yes                 No      Yes        Yes
    ## 258  48 Female      Yes        Yes                 No      Yes         No
    ## 259  35 Female      Yes        Yes                Yes      Yes        Yes
    ## 260  62   Male      Yes         No                 No      Yes         No
    ## 261  33   Male       No        Yes                 No       No         No
    ## 262  55   Male      Yes        Yes                Yes      Yes         No
    ## 263  48   Male      Yes        Yes                Yes       No        Yes
    ## 264  56   Male      Yes         No                Yes      Yes         No
    ## 265  38 Female      Yes        Yes                Yes      Yes        Yes
    ## 266  28 Female       No         No                 No       No         No
    ## 267  68 Female      Yes        Yes                 No      Yes        Yes
    ## 268  35 Female       No         No                 No       No         No
    ## 269  45 Female       No         No                 No       No        Yes
    ## 270  48 Female      Yes        Yes                Yes      Yes        Yes
    ## 271  40 Female      Yes        Yes                Yes      Yes         No
    ## 272  57   Male      Yes        Yes                Yes      Yes        Yes
    ## 273  47   Male       No         No                 No       No         No
    ## 274  45   Male       No         No                 No       No        Yes
    ## 275  57   Male       No         No                 No       No        Yes
    ## 276  72   Male      Yes         No                 No       No        Yes
    ## 277  30   Male       No         No                 No       No         No
    ## 278  27   Male       No         No                 No       No         No
    ## 279  38   Male       No         No                 No       No         No
    ## 280  43   Male       No         No                 No      Yes         No
    ## 281  40   Male       No         No                Yes       No         No
    ## 282  47   Male       No         No                 No       No         No
    ## 283  45   Male       No         No                 No       No        Yes
    ## 284  57   Male       No         No                 No       No        Yes
    ## 285  72   Male      Yes         No                 No       No        Yes
    ## 286  30   Male       No         No                 No       No         No
    ## 287  27   Male       No         No                 No       No         No
    ## 288  38   Male       No         No                 No       No         No
    ## 289  43   Male       No         No                 No      Yes         No
    ## 290  40   Male       No         No                Yes       No         No
    ## 291  54   Male       No         No                 No      Yes         No
    ## 292  30   Male       No         No                 No       No         No
    ## 293  46   Male       No         No                 No      Yes         No
    ## 294  53   Male       No         No                 No      Yes        Yes
    ## 295  42   Male       No         No                 No       No         No
    ## 296  55 Female      Yes        Yes                Yes      Yes         No
    ## 297  48 Female      Yes        Yes                Yes      Yes        Yes
    ## 298  55 Female      Yes        Yes                Yes       No        Yes
    ## 299  39 Female      Yes        Yes                Yes      Yes        Yes
    ## 300  43 Female      Yes        Yes                Yes      Yes        Yes
    ## 301  35 Female      Yes        Yes                 No      Yes         No
    ## 302  47 Female       No         No                Yes      Yes        Yes
    ## 303  61 Female      Yes         No                 No       No        Yes
    ## 304  58 Female      Yes         No                Yes       No        Yes
    ## 305  69 Female      Yes        Yes                Yes      Yes         No
    ## 306  40   Male       No        Yes                Yes      Yes         No
    ## 307  28   Male       No         No                Yes       No         No
    ## 308  37   Male       No         No                 No       No         No
    ## 309  34   Male       No         No                 No       No         No
    ## 310  30   Male       No         No                 No       No         No
    ## 311  67   Male      Yes         No                 No      Yes        Yes
    ## 312  60   Male       No         No                 No      Yes         No
    ## 313  58   Male       No         No                 No       No        Yes
    ## 314  54   Male       No         No                Yes      Yes         No
    ## 315  43   Male       No         No                Yes       No         No
    ## 316  33 Female       No         No                 No       No         No
    ## 317  55 Female       No         No                 No      Yes         No
    ## 318  36 Female       No         No                 No      Yes         No
    ## 319  28 Female       No         No                 No       No        Yes
    ## 320  34 Female       No         No                Yes       No         No
    ## 321  65 Female       No         No                 No      Yes         No
    ## 322  34 Female       No         No                 No       No         No
    ## 323  64   Male       No         No                 No      Yes        Yes
    ## 324  44   Male      Yes         No                Yes      Yes         No
    ## 325  36   Male       No         No                 No       No         No
    ## 326  43   Male       No         No                 No      Yes         No
    ## 327  53   Male       No         No                 No      Yes         No
    ## 328  47   Male       No         No                 No       No         No
    ## 329  58   Male       No        Yes                 No       No         No
    ## 330  56   Male       No         No                Yes      Yes        Yes
    ## 331  51 Female       No         No                 No       No         No
    ## 332  59 Female       No         No                 No      Yes         No
    ## 333  50 Female       No         No                 No      Yes         No
    ## 334  30   Male       No         No                 No       No         No
    ## 335  46   Male       No         No                 No      Yes         No
    ## 336  53   Male       No         No                 No      Yes        Yes
    ## 337  42   Male       No         No                 No       No         No
    ## 338  55 Female      Yes        Yes                Yes      Yes         No
    ## 339  48 Female      Yes        Yes                Yes      Yes        Yes
    ## 340  55 Female      Yes        Yes                Yes       No        Yes
    ## 341  39 Female      Yes        Yes                Yes      Yes        Yes
    ## 342  43 Female      Yes        Yes                Yes      Yes        Yes
    ## 343  35 Female      Yes        Yes                 No      Yes         No
    ## 344  47 Female       No         No                Yes      Yes        Yes
    ## 345  61 Female      Yes         No                 No       No        Yes
    ## 346  58 Female      Yes         No                Yes       No        Yes
    ## 347  69 Female      Yes        Yes                Yes      Yes         No
    ## 348  40   Male       No        Yes                Yes      Yes         No
    ## 349  28   Male       No         No                Yes       No         No
    ## 350  37   Male       No         No                 No       No         No
    ## 351  34   Male       No         No                 No       No         No
    ## 352  30   Male       No         No                 No       No         No
    ## 353  67   Male      Yes         No                 No      Yes        Yes
    ## 354  60   Male       No         No                 No      Yes         No
    ## 355  58   Male       No         No                 No       No        Yes
    ## 356  54   Male       No         No                Yes      Yes         No
    ## 357  43   Male       No         No                Yes       No         No
    ## 358  33 Female       No         No                 No       No         No
    ## 359  55   Male      Yes        Yes                Yes      Yes         No
    ## 360  48   Male      Yes        Yes                Yes       No        Yes
    ## 361  56   Male      Yes         No                Yes      Yes         No
    ## 362  38 Female      Yes        Yes                Yes      Yes        Yes
    ## 363  28 Female       No         No                 No       No         No
    ## 364  68 Female      Yes        Yes                 No      Yes        Yes
    ## 365  35 Female       No         No                 No       No         No
    ## 366  45 Female       No         No                 No       No        Yes
    ## 367  48 Female      Yes        Yes                Yes      Yes        Yes
    ## 368  40 Female      Yes        Yes                Yes      Yes         No
    ## 369  57   Male      Yes        Yes                Yes      Yes        Yes
    ## 370  47   Male       No         No                 No       No         No
    ## 371  45   Male       No         No                 No       No        Yes
    ## 372  57   Male       No         No                 No       No        Yes
    ## 373  72   Male      Yes         No                 No       No        Yes
    ## 374  30   Male       No         No                 No       No         No
    ## 375  27   Male       No         No                 No       No         No
    ## 376  38   Male       No         No                 No       No         No
    ## 377  43   Male       No         No                 No      Yes         No
    ## 378  40   Male       No         No                Yes       No         No
    ## 379  47   Male      Yes        Yes                 No       No         No
    ## 380  62   Male      Yes        Yes                 No       No        Yes
    ## 381  49   Male      Yes        Yes                Yes       No         No
    ## 382  53   Male      Yes         No                Yes       No         No
    ## 383  68   Male      Yes        Yes                 No       No        Yes
    ## 384  61   Male      Yes         No                 No      Yes        Yes
    ## 385  39   Male      Yes        Yes                 No       No         No
    ## 386  38   Male      Yes         No                 No       No         No
    ## 387  44   Male      Yes         No                Yes      Yes         No
    ## 388  36   Male       No         No                 No       No         No
    ## 389  43   Male       No         No                 No      Yes         No
    ## 390  53   Male       No         No                 No      Yes         No
    ## 391  47   Male       No         No                 No       No         No
    ## 392  58   Male       No        Yes                 No       No         No
    ## 393  56   Male       No         No                Yes      Yes        Yes
    ## 394  51 Female       No         No                 No       No         No
    ## 395  59 Female       No         No                 No      Yes         No
    ## 396  50 Female       No         No                 No      Yes         No
    ## 397  30   Male       No         No                 No       No         No
    ## 398  46   Male       No         No                 No      Yes         No
    ## 399  53   Male       No         No                 No      Yes        Yes
    ## 400  64   Male       No         No                 No      Yes        Yes
    ## 401  44   Male      Yes         No                Yes      Yes         No
    ## 402  36   Male       No         No                 No       No         No
    ## 403  43   Male       No         No                 No      Yes         No
    ## 404  53   Male       No         No                 No      Yes         No
    ## 405  47   Male       No         No                 No       No         No
    ## 406  58   Male       No        Yes                 No       No         No
    ## 407  56   Male       No         No                Yes      Yes        Yes
    ## 408  51 Female       No         No                 No       No         No
    ## 409  59 Female       No         No                 No      Yes         No
    ## 410  50 Female       No         No                 No      Yes         No
    ## 411  30   Male       No         No                 No       No         No
    ## 412  46   Male       No         No                 No      Yes         No
    ## 413  53   Male       No         No                 No      Yes        Yes
    ## 414  42   Male       No         No                 No       No         No
    ## 415  55 Female      Yes        Yes                Yes      Yes         No
    ## 416  48 Female      Yes        Yes                Yes      Yes        Yes
    ## 417  55 Female      Yes        Yes                Yes       No        Yes
    ## 418  39 Female      Yes        Yes                Yes      Yes        Yes
    ## 419  43 Female      Yes        Yes                Yes      Yes        Yes
    ## 420  35 Female      Yes        Yes                 No      Yes         No
    ## 421  47 Female       No         No                Yes      Yes        Yes
    ## 422  61 Female      Yes         No                 No       No        Yes
    ## 423  67   Male       No        Yes                 No      Yes        Yes
    ## 424  66   Male      Yes        Yes                 No      Yes        Yes
    ## 425  43   Male      Yes        Yes                Yes      Yes         No
    ## 426  62   Male      Yes        Yes                 No      Yes        Yes
    ## 427  54   Male      Yes        Yes                Yes      Yes        Yes
    ## 428  39   Male      Yes         No                Yes       No         No
    ## 429  48   Male       No        Yes                Yes      Yes         No
    ## 430  58   Male      Yes        Yes                Yes      Yes        Yes
    ## 431  32   Male       No         No                 No       No         No
    ## 432  42   Male       No         No                 No      Yes        Yes
    ## 433  52   Male      Yes        Yes                Yes      Yes        Yes
    ## 434  38   Male       No        Yes                 No       No         No
    ## 435  53   Male      Yes        Yes                Yes      Yes        Yes
    ## 436  57   Male      Yes        Yes                Yes      Yes        Yes
    ## 437  41   Male      Yes        Yes                Yes      Yes        Yes
    ## 438  37   Male       No         No                 No      Yes         No
    ## 439  54   Male      Yes        Yes                Yes      Yes         No
    ## 440  49   Male      Yes        Yes                 No      Yes         No
    ## 441  48   Male      Yes        Yes                 No      Yes         No
    ## 442  60   Male      Yes         No                 No       No         No
    ## 443  63   Male      Yes        Yes                Yes      Yes        Yes
    ## 444  35   Male      Yes         No                Yes       No         No
    ## 445  30 Female      Yes         No                Yes      Yes        Yes
    ## 446  53 Female       No        Yes                Yes       No        Yes
    ## 447  50 Female      Yes        Yes                Yes       No        Yes
    ## 448  50 Female       No        Yes                 No      Yes        Yes
    ## 449  35 Female      Yes        Yes                Yes      Yes        Yes
    ## 450  40 Female      Yes        Yes                Yes      Yes         No
    ## 451  48 Female       No         No                Yes      Yes         No
    ## 452  60 Female      Yes        Yes                Yes      Yes        Yes
    ## 453  38 Female      Yes        Yes                Yes      Yes        Yes
    ## 454  28 Female       No         No                 No       No         No
    ## 455  68 Female      Yes        Yes                 No      Yes        Yes
    ## 456  35 Female       No         No                 No       No         No
    ## 457  45 Female       No         No                 No       No        Yes
    ## 458  48 Female      Yes        Yes                Yes      Yes        Yes
    ## 459  40 Female      Yes        Yes                Yes      Yes         No
    ## 460  57   Male      Yes        Yes                Yes      Yes        Yes
    ## 461  47   Male       No         No                 No       No         No
    ## 462  45   Male       No         No                 No       No        Yes
    ## 463  57   Male       No         No                 No       No        Yes
    ## 464  72   Male      Yes         No                 No       No        Yes
    ## 465  30   Male       No         No                 No       No         No
    ## 466  27   Male       No         No                 No       No         No
    ## 467  38   Male       No         No                 No       No         No
    ## 468  43   Male       No         No                 No      Yes         No
    ## 469  40   Male       No         No                Yes       No         No
    ## 470  47   Male       No         No                 No       No         No
    ## 471  45   Male       No         No                 No       No        Yes
    ## 472  57   Male       No         No                 No       No        Yes
    ## 473  72   Male      Yes         No                 No       No        Yes
    ## 474  30   Male       No         No                 No       No         No
    ## 475  27   Male       No         No                 No       No         No
    ## 476  38   Male       No         No                 No       No         No
    ## 477  43   Male       No         No                 No      Yes         No
    ## 478  40   Male       No         No                Yes       No         No
    ## 479  54   Male       No         No                 No      Yes         No
    ## 480  30   Male       No         No                 No       No         No
    ## 481  46   Male       No         No                 No      Yes         No
    ## 482  53   Male       No         No                 No      Yes        Yes
    ## 483  42   Male       No         No                 No       No         No
    ## 484  55 Female      Yes        Yes                Yes      Yes         No
    ## 485  48 Female      Yes        Yes                Yes      Yes        Yes
    ## 486  55 Female      Yes        Yes                Yes       No        Yes
    ## 487  39 Female      Yes        Yes                Yes      Yes        Yes
    ## 488  43 Female      Yes        Yes                Yes      Yes        Yes
    ## 489  50 Female       No         No                 No      Yes         No
    ## 490  30   Male       No         No                 No       No         No
    ## 491  46   Male       No         No                 No      Yes         No
    ## 492  53   Male       No         No                 No      Yes        Yes
    ## 493  64   Male       No         No                 No      Yes        Yes
    ## 494  44   Male      Yes         No                Yes      Yes         No
    ## 495  36   Male       No         No                 No       No         No
    ## 496  43   Male       No         No                 No      Yes         No
    ## 497  53   Male       No         No                 No      Yes         No
    ## 498  47   Male       No         No                 No       No         No
    ## 499  68 Female      Yes        Yes                 No      Yes        Yes
    ## 500  64   Male       No         No                 No      Yes        Yes
    ## 501  66   Male      Yes         No                Yes       No         No
    ## 502  67   Male       No         No                 No       No        Yes
    ## 503  70   Male      Yes         No                 No       No        Yes
    ## 504  44   Male       No         No                 No       No         No
    ## 505  38   Male       No         No                 No       No         No
    ## 506  35   Male       No         No                 No       No         No
    ## 507  61   Male       No         No                 No      Yes         No
    ## 508  60   Male       No         No                Yes       No         No
    ## 509  58   Male       No         No                 No      Yes         No
    ## 510  54   Male       No         No                 No       No         No
    ## 511  67   Male       No         No                 No      Yes         No
    ## 512  66   Male       No         No                 No      Yes        Yes
    ## 513  43   Male       No         No                 No       No         No
    ## 514  62 Female      Yes        Yes                Yes      Yes         No
    ## 515  54 Female      Yes        Yes                Yes      Yes        Yes
    ## 516  39 Female      Yes        Yes                Yes       No        Yes
    ## 517  48 Female      Yes        Yes                Yes      Yes        Yes
    ## 518  58 Female      Yes        Yes                Yes      Yes        Yes
    ## 519  32 Female       No         No                 No      Yes         No
    ## 520  42   Male       No         No                 No       No         No
    ##     Genital.thrush visual.blurring Itching Irritability delayed.healing
    ## 1               No              No     Yes           No             Yes
    ## 2               No             Yes      No           No              No
    ## 3               No              No     Yes           No             Yes
    ## 4              Yes              No     Yes           No             Yes
    ## 5               No             Yes     Yes          Yes             Yes
    ## 6               No             Yes     Yes           No             Yes
    ## 7              Yes              No      No           No             Yes
    ## 8               No             Yes     Yes          Yes              No
    ## 9              Yes              No     Yes          Yes              No
    ## 10              No             Yes     Yes          Yes              No
    ## 11             Yes              No      No          Yes             Yes
    ## 12             Yes              No     Yes           No             Yes
    ## 13             Yes              No      No          Yes             Yes
    ## 14             Yes             Yes     Yes           No              No
    ## 15              No             Yes     Yes           No             Yes
    ## 16              No              No      No           No             Yes
    ## 17             Yes              No      No           No             Yes
    ## 18              No             Yes      No          Yes             Yes
    ## 19              No             Yes      No           No              No
    ## 20             Yes              No      No           No              No
    ## 21              No             Yes      No          Yes              No
    ## 22             Yes             Yes     Yes           No             Yes
    ## 23             Yes              No     Yes          Yes              No
    ## 24              No             Yes     Yes          Yes             Yes
    ## 25              No             Yes      No           No             Yes
    ## 26             Yes              No      No          Yes             Yes
    ## 27              No              No      No          Yes              No
    ## 28              No             Yes     Yes           No             Yes
    ## 29             Yes              No      No           No              No
    ## 30              No             Yes      No          Yes              No
    ## 31              No             Yes      No           No              No
    ## 32             Yes             Yes     Yes          Yes              No
    ## 33              No              No      No           No             Yes
    ## 34              No             Yes     Yes          Yes             Yes
    ## 35              No             Yes     Yes           No              No
    ## 36             Yes             Yes     Yes           No              No
    ## 37              No             Yes      No           No              No
    ## 38              No             Yes      No           No              No
    ## 39              No              No      No           No              No
    ## 40              No              No      No           No             Yes
    ## 41             Yes             Yes     Yes          Yes             Yes
    ## 42              No              No      No           No             Yes
    ## 43              No             Yes     Yes          Yes             Yes
    ## 44              No              No     Yes           No             Yes
    ## 45              No             Yes     Yes           No              No
    ## 46              No             Yes     Yes           No             Yes
    ## 47              No             Yes     Yes           No             Yes
    ## 48              No             Yes     Yes           No              No
    ## 49              No             Yes     Yes           No             Yes
    ## 50              No             Yes      No           No             Yes
    ## 51              No             Yes      No          Yes             Yes
    ## 52              No             Yes      No           No              No
    ## 53             Yes             Yes      No          Yes             Yes
    ## 54              No             Yes      No           No             Yes
    ## 55              No             Yes      No           No              No
    ## 56              No              No      No           No              No
    ## 57             Yes             Yes     Yes           No             Yes
    ## 58              No             Yes      No           No              No
    ## 59              No              No      No           No              No
    ## 60              No             Yes      No          Yes             Yes
    ## 61              No              No     Yes           No              No
    ## 62              No             Yes     Yes           No             Yes
    ## 63              No             Yes     Yes           No             Yes
    ## 64             Yes             Yes     Yes           No             Yes
    ## 65              No             Yes     Yes           No              No
    ## 66              No              No      No           No              No
    ## 67              No              No     Yes           No             Yes
    ## 68              No              No     Yes           No              No
    ## 69              No             Yes      No           No              No
    ## 70              No             Yes      No           No              No
    ## 71              No              No      No           No              No
    ## 72              No              No     Yes           No             Yes
    ## 73             Yes              No      No           No              No
    ## 74              No              No      No           No              No
    ## 75              No              No     Yes           No             Yes
    ## 76              No             Yes      No           No              No
    ## 77              No              No      No           No              No
    ## 78              No              No     Yes           No             Yes
    ## 79              No              No     Yes          Yes             Yes
    ## 80              No             Yes      No           No              No
    ## 81              No             Yes      No           No              No
    ## 82              No              No      No           No              No
    ## 83              No              No      No           No             Yes
    ## 84              No             Yes     Yes           No             Yes
    ## 85              No             Yes     Yes           No             Yes
    ## 86              No              No      No           No              No
    ## 87              No             Yes     Yes          Yes             Yes
    ## 88              No             Yes      No           No              No
    ## 89              No             Yes     Yes           No             Yes
    ## 90              No              No      No           No              No
    ## 91              No             Yes     Yes           No              No
    ## 92              No             Yes     Yes          Yes             Yes
    ## 93              No             Yes      No           No             Yes
    ## 94              No              No     Yes           No              No
    ## 95              No             Yes     Yes           No             Yes
    ## 96              No             Yes     Yes           No              No
    ## 97              No              No     Yes           No             Yes
    ## 98              No              No     Yes          Yes              No
    ## 99              No             Yes      No           No              No
    ## 100            Yes             Yes     Yes           No             Yes
    ## 101            Yes              No      No          Yes             Yes
    ## 102            Yes             Yes     Yes           No             Yes
    ## 103            Yes             Yes     Yes           No              No
    ## 104             No              No      No          Yes             Yes
    ## 105            Yes              No      No          Yes             Yes
    ## 106            Yes             Yes     Yes           No             Yes
    ## 107             No             Yes     Yes           No              No
    ## 108             No              No      No           No             Yes
    ## 109            Yes             Yes     Yes           No             Yes
    ## 110             No             Yes      No          Yes             Yes
    ## 111             No             Yes     Yes           No             Yes
    ## 112            Yes             Yes      No          Yes              No
    ## 113            Yes             Yes      No          Yes             Yes
    ## 114            Yes              No     Yes          Yes              No
    ## 115            Yes             Yes     Yes           No             Yes
    ## 116             No             Yes     Yes          Yes             Yes
    ## 117             No              No      No          Yes              No
    ## 118             No             Yes     Yes          Yes             Yes
    ## 119             No             Yes     Yes          Yes             Yes
    ## 120             No              No      No          Yes              No
    ## 121             No              No      No           No             Yes
    ## 122            Yes              No     Yes           No             Yes
    ## 123             No              No     Yes          Yes             Yes
    ## 124             No             Yes     Yes           No              No
    ## 125             No              No      No           No              No
    ## 126             No             Yes      No           No             Yes
    ## 127            Yes              No     Yes          Yes             Yes
    ## 128             No              No     Yes          Yes              No
    ## 129            Yes              No     Yes          Yes              No
    ## 130             No              No     Yes           No              No
    ## 131            Yes             Yes     Yes          Yes              No
    ## 132             No              No      No          Yes              No
    ## 133            Yes              No      No           No             Yes
    ## 134             No              No      No           No              No
    ## 135             No              No      No           No              No
    ## 136            Yes             Yes      No          Yes             Yes
    ## 137            Yes              No     Yes          Yes              No
    ## 138            Yes              No     Yes          Yes             Yes
    ## 139            Yes              No      No          Yes             Yes
    ## 140            Yes              No     Yes           No             Yes
    ## 141             No              No      No           No              No
    ## 142             No             Yes      No          Yes             Yes
    ## 143             No              No     Yes           No              No
    ## 144             No              No      No           No             Yes
    ## 145             No             Yes     Yes          Yes              No
    ## 146            Yes             Yes     Yes          Yes             Yes
    ## 147            Yes              No     Yes           No              No
    ## 148            Yes              No     Yes           No              No
    ## 149            Yes              No      No           No             Yes
    ## 150            Yes              No      No           No             Yes
    ## 151             No              No     Yes          Yes             Yes
    ## 152            Yes              No      No          Yes              No
    ## 153            Yes              No      No          Yes             Yes
    ## 154            Yes             Yes      No          Yes             Yes
    ## 155            Yes             Yes     Yes          Yes              No
    ## 156             No              No      No           No              No
    ## 157            Yes              No      No          Yes              No
    ## 158            Yes              No      No           No             Yes
    ## 159            Yes              No     Yes          Yes              No
    ## 160             No             Yes     Yes          Yes             Yes
    ## 161             No             Yes      No           No              No
    ## 162             No             Yes     Yes           No             Yes
    ## 163             No              No      No           No              No
    ## 164             No             Yes     Yes           No              No
    ## 165             No             Yes     Yes          Yes             Yes
    ## 166             No             Yes      No           No             Yes
    ## 167             No             Yes      No           No              No
    ## 168            Yes             Yes     Yes          Yes              No
    ## 169             No              No      No           No             Yes
    ## 170             No             Yes     Yes          Yes             Yes
    ## 171             No             Yes     Yes           No              No
    ## 172            Yes             Yes     Yes           No              No
    ## 173             No             Yes      No           No              No
    ## 174             No             Yes      No           No              No
    ## 175             No              No      No           No              No
    ## 176             No              No      No           No             Yes
    ## 177            Yes             Yes     Yes          Yes             Yes
    ## 178             No              No      No           No             Yes
    ## 179             No             Yes     Yes          Yes             Yes
    ## 180             No              No     Yes           No             Yes
    ## 181             No             Yes     Yes           No              No
    ## 182             No              No     Yes          Yes              No
    ## 183             No             Yes      No           No              No
    ## 184            Yes             Yes     Yes           No             Yes
    ## 185            Yes              No      No          Yes             Yes
    ## 186            Yes             Yes     Yes           No             Yes
    ## 187            Yes             Yes     Yes           No              No
    ## 188             No              No      No          Yes             Yes
    ## 189            Yes              No      No          Yes             Yes
    ## 190            Yes             Yes     Yes           No             Yes
    ## 191             No             Yes     Yes           No              No
    ## 192             No             Yes      No           No              No
    ## 193             No              No      No          Yes             Yes
    ## 194            Yes             Yes     Yes           No             Yes
    ## 195            Yes             Yes     Yes          Yes             Yes
    ## 196             No             Yes      No           No              No
    ## 197             No             Yes      No           No              No
    ## 198             No              No      No          Yes              No
    ## 199             No              No      No          Yes              No
    ## 200             No             Yes     Yes          Yes              No
    ## 201             No             Yes     Yes           No              No
    ## 202             No              No      No           No              No
    ## 203             No              No      No           No              No
    ## 204             No              No      No           No              No
    ## 205             No              No      No           No              No
    ## 206             No             Yes     Yes          Yes             Yes
    ## 207             No              No      No           No              No
    ## 208             No              No     Yes           No             Yes
    ## 209            Yes              No      No           No             Yes
    ## 210            Yes              No      No           No             Yes
    ## 211            Yes              No      No           No              No
    ## 212             No              No      No          Yes              No
    ## 213             No              No      No          Yes              No
    ## 214             No              No     Yes           No              No
    ## 215             No              No     Yes           No              No
    ## 216            Yes              No      No           No              No
    ## 217             No             Yes      No           No              No
    ## 218             No             Yes     Yes           No             Yes
    ## 219             No              No      No           No              No
    ## 220             No              No      No           No              No
    ## 221             No              No      No           No              No
    ## 222            Yes              No     Yes           No             Yes
    ## 223             No              No      No           No              No
    ## 224             No             Yes     Yes           No             Yes
    ## 225             No             Yes     Yes           No             Yes
    ## 226             No              No      No           No              No
    ## 227             No              No     Yes           No              No
    ## 228             No              No      No           No              No
    ## 229             No              No     Yes          Yes             Yes
    ## 230             No              No     Yes           No             Yes
    ## 231            Yes              No      No           No              No
    ## 232             No              No      No           No              No
    ## 233             No             Yes     Yes           No             Yes
    ## 234             No              No     Yes           No             Yes
    ## 235             No              No     Yes           No             Yes
    ## 236             No              No      No           No              No
    ## 237             No             Yes     Yes           No             Yes
    ## 238             No             Yes     Yes           No             Yes
    ## 239             No             Yes     Yes           No             Yes
    ## 240             No              No      No           No              No
    ## 241             No              No     Yes           No              No
    ## 242             No             Yes     Yes          Yes             Yes
    ## 243             No             Yes     Yes           No             Yes
    ## 244             No              No      No           No              No
    ## 245             No              No     Yes           No             Yes
    ## 246             No              No      No           No              No
    ## 247             No              No     Yes           No             Yes
    ## 248             No             Yes     Yes           No             Yes
    ## 249             No              No      No           No              No
    ## 250             No             Yes      No           No              No
    ## 251             No              No      No           No              No
    ## 252             No              No     Yes           No             Yes
    ## 253             No              No     Yes          Yes             Yes
    ## 254             No             Yes      No           No              No
    ## 255             No             Yes      No           No              No
    ## 256             No              No      No           No              No
    ## 257             No              No      No           No             Yes
    ## 258             No             Yes     Yes           No             Yes
    ## 259             No             Yes     Yes           No             Yes
    ## 260            Yes             Yes     Yes          Yes              No
    ## 261             No              No      No           No              No
    ## 262            Yes              No      No          Yes              No
    ## 263            Yes              No      No           No             Yes
    ## 264            Yes              No     Yes          Yes              No
    ## 265             No             Yes     Yes          Yes             Yes
    ## 266             No             Yes      No           No              No
    ## 267             No             Yes     Yes           No             Yes
    ## 268             No              No      No           No              No
    ## 269             No             Yes     Yes           No              No
    ## 270             No             Yes     Yes          Yes             Yes
    ## 271             No             Yes      No           No             Yes
    ## 272             No             Yes      No           No              No
    ## 273             No              No     Yes           No              No
    ## 274            Yes              No      No           No              No
    ## 275             No             Yes      No           No              No
    ## 276             No             Yes     Yes           No             Yes
    ## 277             No              No      No           No              No
    ## 278             No              No      No           No              No
    ## 279             No              No      No           No              No
    ## 280            Yes              No     Yes           No             Yes
    ## 281             No              No      No           No              No
    ## 282             No              No     Yes           No              No
    ## 283            Yes              No      No           No              No
    ## 284             No             Yes      No           No              No
    ## 285             No             Yes     Yes           No             Yes
    ## 286             No              No      No           No              No
    ## 287             No              No      No           No              No
    ## 288             No              No      No           No              No
    ## 289            Yes              No     Yes           No             Yes
    ## 290             No              No      No           No              No
    ## 291             No              No     Yes           No             Yes
    ## 292             No              No      No           No              No
    ## 293             No              No     Yes           No             Yes
    ## 294             No             Yes     Yes           No             Yes
    ## 295             No              No      No           No              No
    ## 296             No             Yes      No           No              No
    ## 297             No              No      No           No              No
    ## 298             No              No     Yes           No             Yes
    ## 299             No              No     Yes          Yes             Yes
    ## 300             No             Yes      No           No              No
    ## 301             No             Yes      No           No              No
    ## 302             No              No      No           No              No
    ## 303             No              No      No          Yes              No
    ## 304             No              No      No          Yes              No
    ## 305             No             Yes     Yes          Yes              No
    ## 306             No             Yes     Yes           No              No
    ## 307             No              No      No           No              No
    ## 308             No              No      No           No              No
    ## 309             No              No      No           No              No
    ## 310             No              No      No           No              No
    ## 311             No             Yes     Yes          Yes             Yes
    ## 312             No              No      No           No              No
    ## 313             No              No     Yes           No             Yes
    ## 314            Yes              No      No           No             Yes
    ## 315            Yes              No      No           No             Yes
    ## 316             No              No      No           No              No
    ## 317            Yes              No     Yes           No             Yes
    ## 318             No              No      No          Yes              No
    ## 319             No              No      No           No              No
    ## 320             No              No      No           No              No
    ## 321             No              No     Yes           No             Yes
    ## 322             No              No      No           No              No
    ## 323             No             Yes     Yes          Yes             Yes
    ## 324            Yes              No     Yes           No             Yes
    ## 325             No              No      No           No              No
    ## 326            Yes              No     Yes           No              No
    ## 327             No             Yes     Yes           No             Yes
    ## 328             No              No      No          Yes              No
    ## 329             No             Yes     Yes           No              No
    ## 330             No             Yes     Yes           No             Yes
    ## 331            Yes              No     Yes           No             Yes
    ## 332             No             Yes     Yes           No             Yes
    ## 333             No             Yes     Yes           No             Yes
    ## 334             No              No      No           No              No
    ## 335             No              No     Yes           No             Yes
    ## 336             No             Yes     Yes           No             Yes
    ## 337             No              No      No           No              No
    ## 338             No             Yes      No           No              No
    ## 339             No              No      No           No              No
    ## 340             No              No     Yes           No             Yes
    ## 341             No              No     Yes          Yes             Yes
    ## 342             No             Yes      No           No              No
    ## 343             No             Yes      No           No              No
    ## 344             No              No      No           No              No
    ## 345             No              No      No          Yes              No
    ## 346             No              No      No          Yes              No
    ## 347             No             Yes     Yes          Yes              No
    ## 348             No             Yes     Yes           No              No
    ## 349             No              No      No           No              No
    ## 350             No              No      No           No              No
    ## 351             No              No      No           No              No
    ## 352             No              No      No           No              No
    ## 353             No             Yes     Yes          Yes             Yes
    ## 354             No              No      No           No              No
    ## 355             No              No     Yes           No             Yes
    ## 356            Yes              No      No           No             Yes
    ## 357            Yes              No      No           No             Yes
    ## 358             No              No      No           No              No
    ## 359            Yes              No      No          Yes              No
    ## 360            Yes              No      No           No             Yes
    ## 361            Yes              No     Yes          Yes              No
    ## 362             No             Yes     Yes          Yes             Yes
    ## 363             No             Yes      No           No              No
    ## 364             No             Yes     Yes           No             Yes
    ## 365             No              No      No           No              No
    ## 366             No             Yes     Yes           No              No
    ## 367             No             Yes     Yes          Yes             Yes
    ## 368             No             Yes      No           No             Yes
    ## 369             No             Yes      No           No              No
    ## 370             No              No     Yes           No              No
    ## 371            Yes              No      No           No              No
    ## 372             No             Yes      No           No              No
    ## 373             No             Yes     Yes           No             Yes
    ## 374             No              No      No           No              No
    ## 375             No              No      No           No              No
    ## 376             No              No      No           No              No
    ## 377            Yes              No     Yes           No             Yes
    ## 378             No              No      No           No              No
    ## 379             No              No      No           No              No
    ## 380             No             Yes      No          Yes             Yes
    ## 381             No              No     Yes           No              No
    ## 382             No              No      No           No             Yes
    ## 383             No             Yes     Yes          Yes              No
    ## 384            Yes             Yes     Yes          Yes             Yes
    ## 385            Yes              No     Yes           No              No
    ## 386            Yes              No     Yes           No              No
    ## 387            Yes              No     Yes           No             Yes
    ## 388             No              No      No           No              No
    ## 389            Yes              No     Yes           No              No
    ## 390             No             Yes     Yes           No             Yes
    ## 391             No              No      No          Yes              No
    ## 392             No             Yes     Yes           No              No
    ## 393             No             Yes     Yes           No             Yes
    ## 394            Yes              No     Yes           No             Yes
    ## 395             No             Yes     Yes           No             Yes
    ## 396             No             Yes     Yes           No             Yes
    ## 397             No              No      No           No              No
    ## 398             No              No     Yes           No             Yes
    ## 399             No             Yes     Yes           No             Yes
    ## 400             No             Yes     Yes          Yes             Yes
    ## 401            Yes              No     Yes           No             Yes
    ## 402             No              No      No           No              No
    ## 403            Yes              No     Yes           No              No
    ## 404             No             Yes     Yes           No             Yes
    ## 405             No              No      No          Yes              No
    ## 406             No             Yes     Yes           No              No
    ## 407             No             Yes     Yes           No             Yes
    ## 408            Yes              No     Yes           No             Yes
    ## 409             No             Yes     Yes           No             Yes
    ## 410             No             Yes     Yes           No             Yes
    ## 411             No              No      No           No              No
    ## 412             No              No     Yes           No             Yes
    ## 413             No             Yes     Yes           No             Yes
    ## 414             No              No      No           No              No
    ## 415             No             Yes      No           No              No
    ## 416             No              No      No           No              No
    ## 417             No              No     Yes           No             Yes
    ## 418             No              No     Yes          Yes             Yes
    ## 419             No             Yes      No           No              No
    ## 420             No             Yes      No           No              No
    ## 421             No              No      No           No              No
    ## 422             No              No      No          Yes              No
    ## 423             No             Yes      No          Yes             Yes
    ## 424             No             Yes      No           No              No
    ## 425            Yes              No      No           No              No
    ## 426             No             Yes      No          Yes              No
    ## 427            Yes             Yes     Yes           No             Yes
    ## 428            Yes              No     Yes          Yes              No
    ## 429             No             Yes     Yes          Yes             Yes
    ## 430             No             Yes      No           No             Yes
    ## 431            Yes              No      No          Yes             Yes
    ## 432             No              No      No          Yes              No
    ## 433             No             Yes     Yes           No             Yes
    ## 434            Yes              No      No           No              No
    ## 435             No             Yes      No          Yes              No
    ## 436             No             Yes      No           No              No
    ## 437            Yes             Yes     Yes          Yes              No
    ## 438             No              No      No           No             Yes
    ## 439             No             Yes     Yes          Yes             Yes
    ## 440             No             Yes     Yes           No              No
    ## 441            Yes             Yes     Yes           No              No
    ## 442             No             Yes      No           No              No
    ## 443             No             Yes      No           No              No
    ## 444             No              No      No           No              No
    ## 445             No              No      No           No             Yes
    ## 446            Yes             Yes     Yes          Yes             Yes
    ## 447             No              No      No           No             Yes
    ## 448             No             Yes     Yes          Yes             Yes
    ## 449             No              No     Yes           No             Yes
    ## 450             No             Yes     Yes           No              No
    ## 451             No             Yes     Yes           No             Yes
    ## 452             No             Yes     Yes           No             Yes
    ## 453             No             Yes     Yes          Yes             Yes
    ## 454             No             Yes      No           No              No
    ## 455             No             Yes     Yes           No             Yes
    ## 456             No              No      No           No              No
    ## 457             No             Yes     Yes           No              No
    ## 458             No             Yes     Yes          Yes             Yes
    ## 459             No             Yes      No           No             Yes
    ## 460             No             Yes      No           No              No
    ## 461             No              No     Yes           No              No
    ## 462            Yes              No      No           No              No
    ## 463             No             Yes      No           No              No
    ## 464             No             Yes     Yes           No             Yes
    ## 465             No              No      No           No              No
    ## 466             No              No      No           No              No
    ## 467             No              No      No           No              No
    ## 468            Yes              No     Yes           No             Yes
    ## 469             No              No      No           No              No
    ## 470             No              No     Yes           No              No
    ## 471            Yes              No      No           No              No
    ## 472             No             Yes      No           No              No
    ## 473             No             Yes     Yes           No             Yes
    ## 474             No              No      No           No              No
    ## 475             No              No      No           No              No
    ## 476             No              No      No           No              No
    ## 477            Yes              No     Yes           No             Yes
    ## 478             No              No      No           No              No
    ## 479             No              No     Yes           No             Yes
    ## 480             No              No      No           No              No
    ## 481             No              No     Yes           No             Yes
    ## 482             No             Yes     Yes           No             Yes
    ## 483             No              No      No           No              No
    ## 484             No             Yes      No           No              No
    ## 485             No              No      No           No              No
    ## 486             No              No     Yes           No             Yes
    ## 487             No              No     Yes          Yes             Yes
    ## 488             No             Yes      No           No              No
    ## 489             No             Yes     Yes           No             Yes
    ## 490             No              No      No           No              No
    ## 491             No              No     Yes           No             Yes
    ## 492             No             Yes     Yes           No             Yes
    ## 493             No             Yes     Yes          Yes             Yes
    ## 494            Yes              No     Yes           No             Yes
    ## 495             No              No      No           No              No
    ## 496            Yes              No     Yes           No              No
    ## 497             No             Yes     Yes           No             Yes
    ## 498             No              No      No          Yes              No
    ## 499             No             Yes     Yes           No             Yes
    ## 500             No             Yes     Yes          Yes             Yes
    ## 501            Yes              No     Yes          Yes              No
    ## 502             No             Yes      No           No              No
    ## 503             No             Yes     Yes           No             Yes
    ## 504             No              No      No           No              No
    ## 505             No              No      No           No              No
    ## 506             No              No      No           No              No
    ## 507            Yes              No     Yes           No             Yes
    ## 508             No              No      No           No              No
    ## 509             No              No     Yes           No             Yes
    ## 510             No              No      No           No              No
    ## 511             No              No     Yes           No             Yes
    ## 512             No             Yes     Yes           No             Yes
    ## 513             No              No      No           No              No
    ## 514             No             Yes      No           No              No
    ## 515             No              No      No           No              No
    ## 516             No              No     Yes           No             Yes
    ## 517             No              No     Yes          Yes             Yes
    ## 518             No             Yes      No           No              No
    ## 519             No             Yes     Yes           No             Yes
    ## 520             No              No      No           No              No
    ##     partial.paresis muscle.stiffness Alopecia Obesity    class
    ## 1                No              Yes      Yes     Yes Positive
    ## 2               Yes               No      Yes      No Positive
    ## 3                No              Yes      Yes      No Positive
    ## 4                No               No       No      No Positive
    ## 5               Yes              Yes      Yes     Yes Positive
    ## 6                No              Yes      Yes     Yes Positive
    ## 7               Yes               No       No      No Positive
    ## 8               Yes              Yes       No      No Positive
    ## 9               Yes              Yes       No     Yes Positive
    ## 10               No               No      Yes      No Positive
    ## 11               No              Yes      Yes      No Positive
    ## 12               No              Yes       No      No Positive
    ## 13               No               No      Yes      No Positive
    ## 14               No               No      Yes     Yes Positive
    ## 15              Yes               No       No      No Positive
    ## 16              Yes              Yes       No      No Positive
    ## 17               No              Yes       No      No Positive
    ## 18              Yes              Yes      Yes     Yes Positive
    ## 19              Yes              Yes       No      No Positive
    ## 20               No               No       No      No Positive
    ## 21              Yes              Yes       No      No Positive
    ## 22               No              Yes      Yes      No Positive
    ## 23               No               No      Yes      No Positive
    ## 24               No               No       No      No Positive
    ## 25              Yes              Yes       No     Yes Positive
    ## 26               No               No       No     Yes Positive
    ## 27               No              Yes       No      No Positive
    ## 28              Yes              Yes       No      No Positive
    ## 29               No               No      Yes      No Positive
    ## 30              Yes               No      Yes      No Positive
    ## 31              Yes               No       No      No Positive
    ## 32               No               No       No     Yes Positive
    ## 33               No               No      Yes      No Positive
    ## 34              Yes              Yes       No      No Positive
    ## 35               No               No       No      No Positive
    ## 36               No               No       No      No Positive
    ## 37              Yes               No      Yes      No Positive
    ## 38               No              Yes      Yes     Yes Positive
    ## 39              Yes               No      Yes      No Positive
    ## 40               No               No       No      No Positive
    ## 41               No              Yes       No      No Positive
    ## 42              Yes               No       No      No Positive
    ## 43              Yes              Yes       No      No Positive
    ## 44              Yes              Yes       No      No Positive
    ## 45              Yes              Yes       No      No Positive
    ## 46              Yes               No       No      No Positive
    ## 47              Yes              Yes       No     Yes Positive
    ## 48              Yes              Yes       No      No Positive
    ## 49               No              Yes       No     Yes Positive
    ## 50              Yes              Yes       No      No Positive
    ## 51              Yes              Yes       No      No Positive
    ## 52              Yes               No       No      No Positive
    ## 53              Yes              Yes       No      No Positive
    ## 54               No               No       No      No Positive
    ## 55              Yes              Yes       No      No Positive
    ## 56               No               No       No     Yes Positive
    ## 57              Yes              Yes       No      No Positive
    ## 58              Yes               No       No      No Positive
    ## 59               No               No       No      No Positive
    ## 60              Yes              Yes       No     Yes Positive
    ## 61              Yes              Yes       No      No Positive
    ## 62              Yes              Yes       No      No Positive
    ## 63              Yes              Yes       No      No Positive
    ## 64              Yes              Yes       No     Yes Positive
    ## 65              Yes               No       No      No Positive
    ## 66               No               No       No      No Positive
    ## 67              Yes               No       No      No Positive
    ## 68              Yes               No       No      No Positive
    ## 69               No               No      Yes      No Positive
    ## 70              Yes               No       No     Yes Positive
    ## 71               No               No       No      No Positive
    ## 72              Yes              Yes       No      No Positive
    ## 73               No               No       No      No Positive
    ## 74              Yes               No       No      No Positive
    ## 75              Yes              Yes       No      No Positive
    ## 76              Yes               No       No     Yes Positive
    ## 77              Yes               No       No      No Positive
    ## 78              Yes               No       No      No Positive
    ## 79              Yes               No       No      No Positive
    ## 80              Yes              Yes       No     Yes Positive
    ## 81               No               No       No      No Positive
    ## 82               No              Yes       No      No Positive
    ## 83              Yes               No       No      No Positive
    ## 84              Yes               No       No      No Positive
    ## 85              Yes              Yes       No      No Positive
    ## 86               No               No       No     Yes Positive
    ## 87              Yes              Yes       No      No Positive
    ## 88              Yes              Yes       No      No Positive
    ## 89              Yes               No       No      No Positive
    ## 90               No               No       No      No Positive
    ## 91              Yes               No       No      No Positive
    ## 92              Yes              Yes       No      No Positive
    ## 93              Yes              Yes       No      No Positive
    ## 94              Yes               No       No      No Positive
    ## 95              Yes               No       No      No Positive
    ## 96              Yes               No       No      No Positive
    ## 97              Yes              Yes       No      No Positive
    ## 98              Yes              Yes       No      No Positive
    ## 99               No               No      Yes      No Positive
    ## 100             Yes               No       No      No Positive
    ## 101              No              Yes      Yes     Yes Positive
    ## 102             Yes              Yes       No      No Positive
    ## 103              No              Yes      Yes      No Positive
    ## 104             Yes              Yes      Yes      No Positive
    ## 105             Yes              Yes      Yes      No Positive
    ## 106              No               No      Yes      No Positive
    ## 107             Yes               No      Yes     Yes Positive
    ## 108             Yes              Yes       No      No Positive
    ## 109              No               No      Yes      No Positive
    ## 110             Yes              Yes       No      No Positive
    ## 111              No               No       No      No Positive
    ## 112             Yes               No      Yes     Yes Positive
    ## 113              No              Yes      Yes      No Positive
    ## 114             Yes               No       No      No Positive
    ## 115             Yes               No       No      No Positive
    ## 116             Yes               No       No      No Positive
    ## 117             Yes              Yes       No      No Positive
    ## 118             Yes              Yes       No      No Positive
    ## 119             Yes               No       No      No Positive
    ## 120              No              Yes       No      No Positive
    ## 121              No              Yes      Yes      No Positive
    ## 122             Yes              Yes       No      No Positive
    ## 123             Yes              Yes       No     Yes Positive
    ## 124              No               No      Yes     Yes Positive
    ## 125              No               No       No      No Positive
    ## 126             Yes               No       No      No Positive
    ## 127             Yes               No       No      No Positive
    ## 128              No              Yes      Yes      No Positive
    ## 129              No               No      Yes      No Positive
    ## 130             Yes               No      Yes      No Positive
    ## 131              No              Yes       No      No Positive
    ## 132              No              Yes       No      No Positive
    ## 133              No               No       No      No Positive
    ## 134              No               No       No      No Positive
    ## 135              No               No       No      No Positive
    ## 136             Yes              Yes      Yes      No Positive
    ## 137              No              Yes      Yes     Yes Positive
    ## 138             Yes              Yes      Yes     Yes Positive
    ## 139             Yes               No      Yes      No Positive
    ## 140              No               No      Yes      No Positive
    ## 141              No              Yes       No      No Positive
    ## 142             Yes               No      Yes      No Positive
    ## 143              No               No       No      No Positive
    ## 144             Yes               No       No      No Positive
    ## 145             Yes               No       No      No Positive
    ## 146             Yes               No       No      No Positive
    ## 147              No              Yes       No     Yes Positive
    ## 148              No              Yes       No     Yes Positive
    ## 149              No               No       No     Yes Positive
    ## 150              No               No      Yes      No Positive
    ## 151              No               No      Yes      No Positive
    ## 152             Yes               No      Yes      No Positive
    ## 153              No               No      Yes      No Positive
    ## 154             Yes              Yes      Yes      No Positive
    ## 155             Yes              Yes      Yes     Yes Positive
    ## 156              No               No       No      No Positive
    ## 157             Yes               No       No      No Positive
    ## 158              No               No       No      No Positive
    ## 159              No               No      Yes      No Positive
    ## 160             Yes              Yes       No      No Positive
    ## 161             Yes              Yes       No      No Positive
    ## 162             Yes               No       No      No Positive
    ## 163              No               No       No      No Positive
    ## 164             Yes               No       No      No Positive
    ## 165             Yes              Yes       No      No Positive
    ## 166             Yes              Yes       No      No Positive
    ## 167             Yes               No       No      No Positive
    ## 168              No               No       No     Yes Positive
    ## 169              No               No      Yes      No Positive
    ## 170             Yes              Yes       No      No Positive
    ## 171              No               No       No      No Positive
    ## 172              No               No       No      No Positive
    ## 173             Yes               No      Yes      No Positive
    ## 174              No              Yes      Yes     Yes Positive
    ## 175             Yes               No      Yes      No Positive
    ## 176              No               No       No      No Positive
    ## 177              No              Yes       No      No Positive
    ## 178             Yes               No       No      No Positive
    ## 179             Yes              Yes       No      No Positive
    ## 180             Yes              Yes       No      No Positive
    ## 181             Yes              Yes       No      No Positive
    ## 182             Yes              Yes       No      No Positive
    ## 183              No               No      Yes      No Positive
    ## 184             Yes               No       No      No Positive
    ## 185              No              Yes      Yes     Yes Positive
    ## 186             Yes              Yes       No      No Positive
    ## 187              No              Yes      Yes      No Positive
    ## 188             Yes              Yes      Yes      No Positive
    ## 189             Yes              Yes      Yes      No Positive
    ## 190              No               No      Yes      No Positive
    ## 191             Yes               No      Yes     Yes Positive
    ## 192              No               No       No      No Positive
    ## 193              No               No       No      No Positive
    ## 194              No               No       No      No Positive
    ## 195              No               No       No      No Positive
    ## 196             Yes               No      Yes      No Positive
    ## 197             Yes               No      Yes      No Positive
    ## 198              No               No      Yes      No Positive
    ## 199              No              Yes       No     Yes Positive
    ## 200              No              Yes       No     Yes Positive
    ## 201             Yes              Yes       No      No Negative
    ## 202              No               No       No      No Negative
    ## 203              No               No       No      No Negative
    ## 204              No               No       No      No Negative
    ## 205              No               No       No      No Negative
    ## 206              No              Yes      Yes     Yes Negative
    ## 207             Yes              Yes       No      No Negative
    ## 208              No               No      Yes     Yes Negative
    ## 209              No               No      Yes      No Negative
    ## 210              No               No      Yes      No Negative
    ## 211              No               No       No      No Negative
    ## 212              No               No       No      No Negative
    ## 213              No               No       No      No Negative
    ## 214             Yes               No       No     Yes Negative
    ## 215              No               No      Yes      No Negative
    ## 216              No               No       No      No Negative
    ## 217              No              Yes       No      No Negative
    ## 218             Yes              Yes      Yes      No Negative
    ## 219              No               No       No      No Negative
    ## 220              No               No       No      No Negative
    ## 221              No               No       No      No Negative
    ## 222              No               No      Yes      No Negative
    ## 223              No               No       No     Yes Negative
    ## 224              No              Yes       No     Yes Negative
    ## 225              No               No      Yes      No Negative
    ## 226              No               No       No      No Negative
    ## 227              No               No       No      No Negative
    ## 228              No               No       No      No Negative
    ## 229             Yes               No       No      No Negative
    ## 230              No               No      Yes      No Negative
    ## 231              No               No       No      No Negative
    ## 232              No               No       No      No Negative
    ## 233              No              Yes      Yes      No Negative
    ## 234             Yes              Yes      Yes      No Negative
    ## 235              No               No      Yes      No Negative
    ## 236              No               No       No      No Negative
    ## 237              No              Yes      Yes      No Negative
    ## 238             Yes              Yes      Yes      No Negative
    ## 239              No              Yes      Yes      No Negative
    ## 240              No               No       No      No Negative
    ## 241             Yes               No      Yes      No Negative
    ## 242              No              Yes      Yes     Yes Negative
    ## 243              No              Yes      Yes      No Negative
    ## 244              No               No       No      No Negative
    ## 245              No              Yes      Yes      No Negative
    ## 246              No               No       No      No Negative
    ## 247              No               No      Yes      No Negative
    ## 248             Yes              Yes      Yes      No Negative
    ## 249              No               No      Yes      No Negative
    ## 250             Yes               No       No     Yes Positive
    ## 251             Yes               No       No      No Positive
    ## 252             Yes               No       No      No Positive
    ## 253             Yes               No       No      No Positive
    ## 254             Yes              Yes       No     Yes Positive
    ## 255              No               No       No      No Positive
    ## 256              No              Yes       No      No Positive
    ## 257             Yes               No       No      No Positive
    ## 258             Yes               No       No      No Positive
    ## 259             Yes              Yes       No      No Positive
    ## 260             Yes              Yes      Yes     Yes Positive
    ## 261              No               No       No      No Positive
    ## 262             Yes               No       No      No Positive
    ## 263              No               No       No      No Positive
    ## 264              No               No      Yes      No Positive
    ## 265             Yes              Yes       No      No Positive
    ## 266             Yes              Yes       No      No Positive
    ## 267             Yes               No       No      No Positive
    ## 268              No               No       No      No Positive
    ## 269             Yes               No       No      No Positive
    ## 270             Yes              Yes       No      No Positive
    ## 271             Yes              Yes       No      No Positive
    ## 272             Yes               No       No      No Positive
    ## 273              No               No      Yes      No Negative
    ## 274              No               No       No      No Negative
    ## 275              No              Yes       No      No Negative
    ## 276             Yes              Yes      Yes      No Negative
    ## 277              No               No       No      No Negative
    ## 278              No               No       No      No Negative
    ## 279              No               No       No      No Negative
    ## 280              No               No      Yes      No Negative
    ## 281              No               No       No     Yes Negative
    ## 282              No               No      Yes      No Negative
    ## 283              No               No       No      No Negative
    ## 284              No              Yes       No      No Negative
    ## 285             Yes              Yes      Yes      No Negative
    ## 286              No               No       No      No Negative
    ## 287              No               No       No      No Negative
    ## 288              No               No       No      No Negative
    ## 289              No               No      Yes      No Negative
    ## 290              No               No       No     Yes Negative
    ## 291              No              Yes      Yes      No Negative
    ## 292              No               No       No      No Negative
    ## 293              No               No      Yes      No Negative
    ## 294             Yes              Yes      Yes      No Negative
    ## 295              No               No      Yes      No Negative
    ## 296             Yes               No       No     Yes Positive
    ## 297             Yes               No       No      No Positive
    ## 298             Yes               No       No      No Positive
    ## 299             Yes               No       No      No Positive
    ## 300             Yes              Yes       No     Yes Positive
    ## 301              No               No       No      No Positive
    ## 302              No              Yes       No      No Positive
    ## 303              No               No      Yes      No Positive
    ## 304              No              Yes       No     Yes Positive
    ## 305              No              Yes       No     Yes Positive
    ## 306             Yes              Yes       No      No Negative
    ## 307              No               No       No      No Negative
    ## 308              No               No       No      No Negative
    ## 309              No               No       No      No Negative
    ## 310              No               No       No      No Negative
    ## 311              No              Yes      Yes     Yes Negative
    ## 312             Yes              Yes       No      No Negative
    ## 313              No               No      Yes     Yes Negative
    ## 314              No               No      Yes      No Negative
    ## 315              No               No      Yes      No Negative
    ## 316              No               No       No      No Negative
    ## 317             Yes               No      Yes      No Negative
    ## 318              No               No       No      No Negative
    ## 319             Yes               No       No      No Negative
    ## 320              No               No       No      No Negative
    ## 321              No               No      Yes      No Negative
    ## 322              No               No       No      No Negative
    ## 323              No              Yes      Yes      No Negative
    ## 324              No               No      Yes     Yes Negative
    ## 325              No               No       No      No Negative
    ## 326              No               No      Yes      No Negative
    ## 327              No              Yes      Yes      No Negative
    ## 328             Yes               No       No     Yes Negative
    ## 329              No              Yes       No      No Negative
    ## 330              No              Yes      Yes      No Negative
    ## 331              No               No      Yes      No Negative
    ## 332              No              Yes      Yes     Yes Negative
    ## 333              No               No      Yes      No Negative
    ## 334              No               No       No      No Negative
    ## 335              No               No      Yes      No Negative
    ## 336             Yes              Yes      Yes      No Negative
    ## 337              No               No      Yes      No Negative
    ## 338             Yes               No       No     Yes Positive
    ## 339             Yes               No       No      No Positive
    ## 340             Yes               No       No      No Positive
    ## 341             Yes               No       No      No Positive
    ## 342             Yes              Yes       No     Yes Positive
    ## 343              No               No       No      No Positive
    ## 344              No              Yes       No      No Positive
    ## 345              No               No      Yes      No Positive
    ## 346              No              Yes       No     Yes Positive
    ## 347              No              Yes       No     Yes Positive
    ## 348             Yes              Yes       No      No Negative
    ## 349              No               No       No      No Negative
    ## 350              No               No       No      No Negative
    ## 351              No               No       No      No Negative
    ## 352              No               No       No      No Negative
    ## 353              No              Yes      Yes     Yes Negative
    ## 354             Yes              Yes       No      No Negative
    ## 355              No               No      Yes     Yes Negative
    ## 356              No               No      Yes      No Negative
    ## 357              No               No      Yes      No Negative
    ## 358              No               No       No      No Negative
    ## 359             Yes               No       No      No Positive
    ## 360              No               No       No      No Positive
    ## 361              No               No      Yes      No Positive
    ## 362             Yes              Yes       No      No Positive
    ## 363             Yes              Yes       No      No Positive
    ## 364             Yes               No       No      No Positive
    ## 365              No               No       No      No Positive
    ## 366             Yes               No       No      No Positive
    ## 367             Yes              Yes       No      No Positive
    ## 368             Yes              Yes       No      No Positive
    ## 369             Yes               No       No      No Positive
    ## 370              No               No      Yes      No Negative
    ## 371              No               No       No      No Negative
    ## 372              No              Yes       No      No Negative
    ## 373             Yes              Yes      Yes      No Negative
    ## 374              No               No       No      No Negative
    ## 375              No               No       No      No Negative
    ## 376              No               No       No      No Negative
    ## 377              No               No      Yes      No Negative
    ## 378              No               No       No     Yes Negative
    ## 379              No              Yes       No      No Positive
    ## 380             Yes               No      Yes      No Positive
    ## 381              No               No       No      No Positive
    ## 382             Yes               No       No      No Positive
    ## 383             Yes               No       No      No Positive
    ## 384             Yes               No       No      No Positive
    ## 385              No              Yes       No     Yes Positive
    ## 386              No              Yes       No     Yes Positive
    ## 387              No               No      Yes     Yes Negative
    ## 388              No               No       No      No Negative
    ## 389              No               No      Yes      No Negative
    ## 390              No              Yes      Yes      No Negative
    ## 391             Yes               No       No     Yes Negative
    ## 392              No              Yes       No      No Negative
    ## 393              No              Yes      Yes      No Negative
    ## 394              No               No      Yes      No Negative
    ## 395              No              Yes      Yes     Yes Negative
    ## 396              No               No      Yes      No Negative
    ## 397              No               No       No      No Negative
    ## 398              No               No      Yes      No Negative
    ## 399             Yes              Yes      Yes      No Negative
    ## 400              No              Yes      Yes      No Negative
    ## 401              No               No      Yes     Yes Negative
    ## 402              No               No       No      No Negative
    ## 403              No               No      Yes      No Negative
    ## 404              No              Yes      Yes      No Negative
    ## 405             Yes               No       No     Yes Negative
    ## 406              No              Yes       No      No Negative
    ## 407              No              Yes      Yes      No Negative
    ## 408              No               No      Yes      No Negative
    ## 409              No              Yes      Yes     Yes Negative
    ## 410              No               No      Yes      No Negative
    ## 411              No               No       No      No Negative
    ## 412              No               No      Yes      No Negative
    ## 413             Yes              Yes      Yes      No Negative
    ## 414              No               No      Yes      No Negative
    ## 415             Yes               No       No     Yes Positive
    ## 416             Yes               No       No      No Positive
    ## 417             Yes               No       No      No Positive
    ## 418             Yes               No       No      No Positive
    ## 419             Yes              Yes       No     Yes Positive
    ## 420              No               No       No      No Positive
    ## 421              No              Yes       No      No Positive
    ## 422              No               No      Yes      No Positive
    ## 423             Yes              Yes      Yes     Yes Positive
    ## 424             Yes              Yes       No      No Positive
    ## 425              No               No       No      No Positive
    ## 426             Yes              Yes       No      No Positive
    ## 427              No              Yes      Yes      No Positive
    ## 428              No               No      Yes      No Positive
    ## 429              No               No       No      No Positive
    ## 430             Yes              Yes       No     Yes Positive
    ## 431              No               No       No     Yes Positive
    ## 432              No              Yes       No      No Positive
    ## 433             Yes              Yes       No      No Positive
    ## 434              No               No      Yes      No Positive
    ## 435             Yes               No      Yes      No Positive
    ## 436             Yes               No       No      No Positive
    ## 437              No               No       No     Yes Positive
    ## 438              No               No      Yes      No Positive
    ## 439             Yes              Yes       No      No Positive
    ## 440              No               No       No      No Positive
    ## 441              No               No       No      No Positive
    ## 442             Yes               No      Yes      No Positive
    ## 443              No              Yes      Yes     Yes Positive
    ## 444             Yes               No      Yes      No Positive
    ## 445              No               No       No      No Positive
    ## 446              No              Yes       No      No Positive
    ## 447             Yes               No       No      No Positive
    ## 448             Yes              Yes       No      No Positive
    ## 449             Yes              Yes       No      No Positive
    ## 450             Yes              Yes       No      No Positive
    ## 451             Yes               No       No      No Positive
    ## 452             Yes              Yes       No     Yes Positive
    ## 453             Yes              Yes       No      No Positive
    ## 454             Yes              Yes       No      No Positive
    ## 455             Yes               No       No      No Positive
    ## 456              No               No       No      No Positive
    ## 457             Yes               No       No      No Positive
    ## 458             Yes              Yes       No      No Positive
    ## 459             Yes              Yes       No      No Positive
    ## 460             Yes               No       No      No Positive
    ## 461              No               No      Yes      No Negative
    ## 462              No               No       No      No Negative
    ## 463              No              Yes       No      No Negative
    ## 464             Yes              Yes      Yes      No Negative
    ## 465              No               No       No      No Negative
    ## 466              No               No       No      No Negative
    ## 467              No               No       No      No Negative
    ## 468              No               No      Yes      No Negative
    ## 469              No               No       No     Yes Negative
    ## 470              No               No      Yes      No Negative
    ## 471              No               No       No      No Negative
    ## 472              No              Yes       No      No Negative
    ## 473             Yes              Yes      Yes      No Negative
    ## 474              No               No       No      No Negative
    ## 475              No               No       No      No Negative
    ## 476              No               No       No      No Negative
    ## 477              No               No      Yes      No Negative
    ## 478              No               No       No     Yes Negative
    ## 479              No              Yes      Yes      No Negative
    ## 480              No               No       No      No Negative
    ## 481              No               No      Yes      No Negative
    ## 482             Yes              Yes      Yes      No Negative
    ## 483              No               No      Yes      No Negative
    ## 484             Yes               No       No     Yes Positive
    ## 485             Yes               No       No      No Positive
    ## 486             Yes               No       No      No Positive
    ## 487             Yes               No       No      No Positive
    ## 488             Yes              Yes       No     Yes Positive
    ## 489              No               No      Yes      No Negative
    ## 490              No               No       No      No Negative
    ## 491              No               No      Yes      No Negative
    ## 492             Yes              Yes      Yes      No Negative
    ## 493              No              Yes      Yes      No Negative
    ## 494              No               No      Yes     Yes Negative
    ## 495              No               No       No      No Negative
    ## 496              No               No      Yes      No Negative
    ## 497              No              Yes      Yes      No Negative
    ## 498             Yes               No       No     Yes Negative
    ## 499             Yes               No       No      No Positive
    ## 500              No              Yes      Yes      No Negative
    ## 501              No               No      Yes      No Positive
    ## 502              No              Yes       No      No Negative
    ## 503             Yes              Yes      Yes      No Negative
    ## 504              No               No       No      No Negative
    ## 505              No               No       No      No Negative
    ## 506              No               No       No      No Negative
    ## 507              No               No      Yes      No Negative
    ## 508              No               No       No     Yes Negative
    ## 509              No              Yes      Yes      No Negative
    ## 510              No               No       No      No Negative
    ## 511              No               No      Yes      No Negative
    ## 512             Yes              Yes      Yes      No Negative
    ## 513              No               No      Yes      No Negative
    ## 514             Yes               No       No     Yes Positive
    ## 515             Yes               No       No      No Positive
    ## 516             Yes               No       No      No Positive
    ## 517             Yes               No       No      No Positive
    ## 518             Yes              Yes       No     Yes Positive
    ## 519              No               No      Yes      No Negative
    ## 520              No               No       No      No Negative

------------------------------------------------------------------------

## Resumo da Importação

------------------------------------------------------------------------

### Dimensões

``` r
dim(etl.full) # Dimensões do dataset
```

    ## [1] 520  17

------------------------------------------------------------------------

### Estrutura

``` r
str(etl.full) # Estrutura do dataset
```

    ## 'data.frame':    520 obs. of  17 variables:
    ##  $ Age               : int  40 58 41 45 60 55 57 66 67 70 ...
    ##  $ Gender            : chr  "Male" "Male" "Male" "Male" ...
    ##  $ Polyuria          : chr  "No" "No" "Yes" "No" ...
    ##  $ Polydipsia        : chr  "Yes" "No" "No" "No" ...
    ##  $ sudden.weight.loss: chr  "No" "No" "No" "Yes" ...
    ##  $ weakness          : chr  "Yes" "Yes" "Yes" "Yes" ...
    ##  $ Polyphagia        : chr  "No" "No" "Yes" "Yes" ...
    ##  $ Genital.thrush    : chr  "No" "No" "No" "Yes" ...
    ##  $ visual.blurring   : chr  "No" "Yes" "No" "No" ...
    ##  $ Itching           : chr  "Yes" "No" "Yes" "Yes" ...
    ##  $ Irritability      : chr  "No" "No" "No" "No" ...
    ##  $ delayed.healing   : chr  "Yes" "No" "Yes" "Yes" ...
    ##  $ partial.paresis   : chr  "No" "Yes" "No" "No" ...
    ##  $ muscle.stiffness  : chr  "Yes" "No" "Yes" "No" ...
    ##  $ Alopecia          : chr  "Yes" "Yes" "Yes" "No" ...
    ##  $ Obesity           : chr  "Yes" "No" "No" "No" ...
    ##  $ class             : chr  "Positive" "Positive" "Positive" "Positive" ...

------------------------------------------------------------------------

## Dicionário de Atributos

------------------------------------------------------------------------

## Limpeza e Transformação

Esta etapa corresponde a um pré-processamento para então iniciar a
análise exploratória de dados.

------------------------------------------------------------------------

### Dados Faltantes

``` r
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

------------------------------------------------------------------------

### Imputação de Dados

------------------------------------------------------------------------

### Transformando para Fator

``` r
# Variável 'class'
etl.full$class <- factor(etl.full$class, labels = c(Positive = "Positivo", Negative = "Negativo"))
```

``` r
# Variável 'Gender'
etl.full$Gender <- factor(etl.full$Gender, labels = c(Male = "Homem", Female = "Mulher"))
```

``` r
# Variável 'Polyuria'
etl.full$Polyuria <- factor(etl.full$Polyuria, labels = c(No = "Não", Yes = "Sim"))
```

``` r
# Variável 'Polydipsia'
etl.full$Polydipsia <- factor(etl.full$Polydipsia, labels = c(No = "Não", Yes = "Sim"))
```

``` r
# Variável 'sudden.weight.loss'
etl.full$sudden.weight.loss <- factor(etl.full$sudden.weight.loss, labels = c(No = "Não", Yes = "Sim"))
```

``` r
# Variável 'weakness'
etl.full$weakness <- factor(etl.full$weakness, labels = c(No = "Não", Yes = "Sim"))
```

``` r
# Variável 'Polyphagia'
etl.full$Polyphagia <- factor(etl.full$Polyphagia, labels = c(No = "Não", Yes = "Sim"))
```

``` r
# Variável 'Genital.thrush'
etl.full$Genital.thrush <- factor(etl.full$Genital.thrush, labels = c(No = "Não", Yes = "Sim"))
```

``` r
# Variável 'visual.blurring'
etl.full$visual.blurring <- factor(etl.full$visual.blurring, labels = c(No = "Não", Yes = "Sim"))
```

``` r
# Variável 'Itching'
etl.full$Itching <- factor(etl.full$Itching, labels = c(No = "Não", Yes = "Sim"))
```

``` r
# Variável 'Irritability'
etl.full$Irritability <- factor(etl.full$Irritability, labels = c(No = "Não", Yes = "Sim"))
```

``` r
# Variável 'delayed.healing'
etl.full$delayed.healing <- factor(etl.full$delayed.healing, labels = c(No = "Não", Yes = "Sim"))
```

``` r
# Variável 'partial.paresis'
etl.full$partial.paresis <- factor(etl.full$partial.paresis, labels = c(No = "Não", Yes = "Sim"))
```

``` r
# Variável 'muscle.stiffness'
etl.full$muscle.stiffness <- factor(etl.full$muscle.stiffness, labels = c(No = "Não", Yes = "Sim"))
```

``` r
# Variável 'Alopecia'
etl.full$Alopecia <- factor(etl.full$Alopecia, labels = c(No = "Não", Yes = "Sim"))
```

``` r
# Variável 'Obesity'
etl.full$Obesity <- factor(etl.full$Obesity, labels = c(No = "Não", Yes = "Sim"))
```

``` r
str(diabetes) # Estrutura do dataset
```

    ## 'data.frame':    520 obs. of  17 variables:
    ##  $ Age               : int  40 58 41 45 60 55 57 66 67 70 ...
    ##  $ Gender            : chr  "Male" "Male" "Male" "Male" ...
    ##  $ Polyuria          : chr  "No" "No" "Yes" "No" ...
    ##  $ Polydipsia        : chr  "Yes" "No" "No" "No" ...
    ##  $ sudden.weight.loss: chr  "No" "No" "No" "Yes" ...
    ##  $ weakness          : chr  "Yes" "Yes" "Yes" "Yes" ...
    ##  $ Polyphagia        : chr  "No" "No" "Yes" "Yes" ...
    ##  $ Genital.thrush    : chr  "No" "No" "No" "Yes" ...
    ##  $ visual.blurring   : chr  "No" "Yes" "No" "No" ...
    ##  $ Itching           : chr  "Yes" "No" "Yes" "Yes" ...
    ##  $ Irritability      : chr  "No" "No" "No" "No" ...
    ##  $ delayed.healing   : chr  "Yes" "No" "Yes" "Yes" ...
    ##  $ partial.paresis   : chr  "No" "Yes" "No" "No" ...
    ##  $ muscle.stiffness  : chr  "Yes" "No" "Yes" "No" ...
    ##  $ Alopecia          : chr  "Yes" "Yes" "Yes" "No" ...
    ##  $ Obesity           : chr  "Yes" "No" "No" "No" ...
    ##  $ class             : chr  "Positive" "Positive" "Positive" "Positive" ...

``` r
str(etl.full) # Estrutura do dataset
```

    ## 'data.frame':    520 obs. of  17 variables:
    ##  $ Age               : int  40 58 41 45 60 55 57 66 67 70 ...
    ##  $ Gender            : Factor w/ 2 levels "Homem","Mulher": 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ Polyuria          : Factor w/ 2 levels "Não","Sim": 1 1 2 1 2 2 2 2 2 1 ...
    ##  $ Polydipsia        : Factor w/ 2 levels "Não","Sim": 2 1 1 1 2 2 2 2 2 2 ...
    ##  $ sudden.weight.loss: Factor w/ 2 levels "Não","Sim": 1 1 1 2 2 1 1 2 1 2 ...
    ##  $ weakness          : Factor w/ 2 levels "Não","Sim": 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ Polyphagia        : Factor w/ 2 levels "Não","Sim": 1 1 2 2 2 2 2 1 2 2 ...
    ##  $ Genital.thrush    : Factor w/ 2 levels "Não","Sim": 1 1 1 2 1 1 2 1 2 1 ...
    ##  $ visual.blurring   : Factor w/ 2 levels "Não","Sim": 1 2 1 1 2 2 1 2 1 2 ...
    ##  $ Itching           : Factor w/ 2 levels "Não","Sim": 2 1 2 2 2 2 1 2 2 2 ...
    ##  $ Irritability      : Factor w/ 2 levels "Não","Sim": 1 1 1 1 2 1 1 2 2 2 ...
    ##  $ delayed.healing   : Factor w/ 2 levels "Não","Sim": 2 1 2 2 2 2 2 1 1 1 ...
    ##  $ partial.paresis   : Factor w/ 2 levels "Não","Sim": 1 2 1 1 2 1 2 2 2 1 ...
    ##  $ muscle.stiffness  : Factor w/ 2 levels "Não","Sim": 2 1 2 1 2 2 1 2 2 1 ...
    ##  $ Alopecia          : Factor w/ 2 levels "Não","Sim": 2 2 2 1 2 2 1 1 1 2 ...
    ##  $ Obesity           : Factor w/ 2 levels "Não","Sim": 2 1 1 1 2 2 1 1 2 1 ...
    ##  $ class             : Factor w/ 2 levels "Positivo","Negativo": 2 2 2 2 2 2 2 2 2 2 ...

------------------------------------------------------------------------

### Separando os Datasets

``` r
# Dividindo os dados em treino e teste, 70% para dados de treino e 30% para dados de teste
set.seed(7)
rows <- sample(1:nrow(etl.full), 0.7 * nrow(etl.full))
etl.trn <- etl.full[rows,]
etl.tst <- etl.full[-rows,]
```

------------------------------------------------------------------------

### Removendo Variáveis

------------------------------------------------------------------------

### Resultado da E.T.L.

``` r
# write.csv(treino.etl, "../datasets/treino_etl.csv", row.names = FALSE)
dput(etl.trn, file = "datasets/etl_trn.R")
dput(etl.tst, file = "datasets/etl_tst.R")
dput(etl.full, file = "datasets/etl_full.R")
```

------------------------------------------------------------------------

# Análise Exploratória

A ideia de uma análise descritiva de dados é tentar responder as
seguintes questões:

1.  Qual a ***frequência*** com que cada valor (ou intervalo de valores)
    aparece no conjunto de dados. Ou seja, qual a distribuição de
    frequências dos dados?

2.  Quais são alguns ***valores típicos*** do conjunto de dados, como
    mínimo e máximo?

3.  Qual seria um valor para representar a ***posição central*** do
    conjunto de dados?

4.  Qual seria uma medida da ***variabilidade*** ou dispersão dos dados?

5.  Existem ***valores atípicos*** ou discrepantes (outliers) no
    conjunto de dados?

6.  Os dados são simétricos? Qual a ***simetria*** dos dados?

``` r
aed.trn <- dget(file = "datasets/etl_trn.R")
```

------------------------------------------------------------------------

## Classificando as Variáveis

Uma boa forma de iniciar uma análise descritiva adequada é verificar os
tipos de variáveis disponíveis.

``` r
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

``` r
data.frame("Qualitativas_Ordinais - " = var.fct_ord)
```

    ## data frame with 0 columns and 0 rows

``` r
data.frame("Quantitativas_Discretas - " = var.num)
```

    ##   Quantitativas_Discretas...
    ## 1                        Age

``` r
data.frame("Quantitativas_Contínuas - " = var.num_con)
```

    ## data frame with 0 columns and 0 rows

------------------------------------------------------------------------

## Análise Univariada

``` r
describe(aed.trn )
```

    ##                     vars   n  mean    sd median trimmed   mad min max range
    ## Age                    1 364 48,51 12,19     48   48,24 13,34  16  90    74
    ## Gender*                2 364  1,64  0,48      2    1,68  0,00   1   2     1
    ## Polyuria*              3 364  1,47  0,50      1    1,46  0,00   1   2     1
    ## Polydipsia*            4 364  1,45  0,50      1    1,44  0,00   1   2     1
    ## sudden.weight.loss*    5 364  1,39  0,49      1    1,36  0,00   1   2     1
    ## weakness*              6 364  1,57  0,50      2    1,59  0,00   1   2     1
    ## Polyphagia*            7 364  1,44  0,50      1    1,43  0,00   1   2     1
    ## Genital.thrush*        8 364  1,21  0,41      1    1,14  0,00   1   2     1
    ## visual.blurring*       9 364  1,45  0,50      1    1,44  0,00   1   2     1
    ## Itching*              10 364  1,47  0,50      1    1,46  0,00   1   2     1
    ## Irritability*         11 364  1,24  0,43      1    1,17  0,00   1   2     1
    ## delayed.healing*      12 364  1,43  0,50      1    1,41  0,00   1   2     1
    ## partial.paresis*      13 364  1,42  0,49      1    1,40  0,00   1   2     1
    ## muscle.stiffness*     14 364  1,36  0,48      1    1,32  0,00   1   2     1
    ## Alopecia*             15 364  1,33  0,47      1    1,28  0,00   1   2     1
    ## Obesity*              16 364  1,17  0,38      1    1,09  0,00   1   2     1
    ## class*                17 364  1,61  0,49      2    1,64  0,00   1   2     1
    ##                      skew kurtosis   se
    ## Age                  0,28    -0,10 0,64
    ## Gender*             -0,59    -1,65 0,03
    ## Polyuria*            0,13    -1,99 0,03
    ## Polydipsia*          0,19    -1,97 0,03
    ## sudden.weight.loss*  0,45    -1,80 0,03
    ## weakness*           -0,30    -1,92 0,03
    ## Polyphagia*          0,23    -1,95 0,03
    ## Genital.thrush*      1,41    -0,02 0,02
    ## visual.blurring*     0,19    -1,97 0,03
    ## Itching*             0,12    -1,99 0,03
    ## Irritability*        1,22    -0,52 0,02
    ## delayed.healing*     0,30    -1,92 0,03
    ## partial.paresis*     0,31    -1,91 0,03
    ## muscle.stiffness*    0,59    -1,65 0,03
    ## Alopecia*            0,73    -1,46 0,02
    ## Obesity*             1,75     1,05 0,02
    ## class*              -0,46    -1,79 0,03

------------------------------------------------------------------------

### Distribuições de Frequências

A primeira tarefa de uma análise estatística de um conjunto de dados
consiste em resumí-los. As técnicas disponíveis para essa finalidade
dependem dos tipos de variáveis envolvidas.

------------------------------------------------------------------------

#### Variáveis Qualitativas

As distribuições podem ser representadas por meio de:

-   Gráfico de Barras

-   Gráfico do tipo Pizza

------------------------------------------------------------------------

> 1.  **Distribuições de Frequências para as Variáveis Qualitativas
>     Nominais**

``` r
# Distribuição de Frequências para as Variáveis Qualitativas Nominais
for(i in var.fct){
      fdt_cat(aed.trn[,i]) %>%
          tibble() -> df
    names(df)[1] = c(colnames(aed.trn[i]))
    print(kable(df[order(df[2], decreasing = TRUE),]))
}
```

    ## 
    ## 
    ## |Gender |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:------|---:|---------:|--------:|---:|---------:|
    ## |Mulher | 234| 0,6428571| 64,28571| 234|  64,28571|
    ## |Homem  | 130| 0,3571429| 35,71429| 364| 100,00000|
    ## 
    ## 
    ## |Polyuria |   f|       rf|   rf(%)|  cf|    cf(%)|
    ## |:--------|---:|--------:|-------:|---:|--------:|
    ## |Não      | 194| 0,532967| 53,2967| 194|  53,2967|
    ## |Sim      | 170| 0,467033| 46,7033| 364| 100,0000|
    ## 
    ## 
    ## |Polydipsia |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:----------|---:|---------:|--------:|---:|---------:|
    ## |Não        | 199| 0,5467033| 54,67033| 199|  54,67033|
    ## |Sim        | 165| 0,4532967| 45,32967| 364| 100,00000|
    ## 
    ## 
    ## |sudden.weight.loss |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:------------------|---:|---------:|--------:|---:|---------:|
    ## |Não                | 222| 0,6098901| 60,98901| 222|  60,98901|
    ## |Sim                | 142| 0,3901099| 39,01099| 364| 100,00000|
    ## 
    ## 
    ## |weakness |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:--------|---:|---------:|--------:|---:|---------:|
    ## |Sim      | 209| 0,5741758| 57,41758| 209|  57,41758|
    ## |Não      | 155| 0,4258242| 42,58242| 364| 100,00000|
    ## 
    ## 
    ## |Polyphagia |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:----------|---:|---------:|--------:|---:|---------:|
    ## |Não        | 203| 0,5576923| 55,76923| 203|  55,76923|
    ## |Sim        | 161| 0,4423077| 44,23077| 364| 100,00000|
    ## 
    ## 
    ## |Genital.thrush |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:--------------|---:|---------:|--------:|---:|---------:|
    ## |Não            | 287| 0,7884615| 78,84615| 287|  78,84615|
    ## |Sim            |  77| 0,2115385| 21,15385| 364| 100,00000|
    ## 
    ## 
    ## |visual.blurring |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:---------------|---:|---------:|--------:|---:|---------:|
    ## |Não             | 199| 0,5467033| 54,67033| 199|  54,67033|
    ## |Sim             | 165| 0,4532967| 45,32967| 364| 100,00000|
    ## 
    ## 
    ## |Itching |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:-------|---:|---------:|--------:|---:|---------:|
    ## |Não     | 193| 0,5302198| 53,02198| 193|  53,02198|
    ## |Sim     | 171| 0,4697802| 46,97802| 364| 100,00000|
    ## 
    ## 
    ## |Irritability |   f|       rf|   rf(%)|  cf|    cf(%)|
    ## |:------------|---:|--------:|-------:|---:|--------:|
    ## |Não          | 277| 0,760989| 76,0989| 277|  76,0989|
    ## |Sim          |  87| 0,239011| 23,9011| 364| 100,0000|
    ## 
    ## 
    ## |delayed.healing |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:---------------|---:|---------:|--------:|---:|---------:|
    ## |Não             | 209| 0,5741758| 57,41758| 209|  57,41758|
    ## |Sim             | 155| 0,4258242| 42,58242| 364| 100,00000|
    ## 
    ## 
    ## |partial.paresis |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:---------------|---:|---------:|--------:|---:|---------:|
    ## |Não             | 210| 0,5769231| 57,69231| 210|  57,69231|
    ## |Sim             | 154| 0,4230769| 42,30769| 364| 100,00000|
    ## 
    ## 
    ## |muscle.stiffness |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:----------------|---:|---------:|--------:|---:|---------:|
    ## |Não              | 234| 0,6428571| 64,28571| 234|  64,28571|
    ## |Sim              | 130| 0,3571429| 35,71429| 364| 100,00000|
    ## 
    ## 
    ## |Alopecia |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:--------|---:|---------:|--------:|---:|---------:|
    ## |Não      | 245| 0,6730769| 67,30769| 245|  67,30769|
    ## |Sim      | 119| 0,3269231| 32,69231| 364| 100,00000|
    ## 
    ## 
    ## |Obesity |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:-------|---:|---------:|--------:|---:|---------:|
    ## |Não     | 302| 0,8296703| 82,96703| 302|  82,96703|
    ## |Sim     |  62| 0,1703297| 17,03297| 364| 100,00000|
    ## 
    ## 
    ## |class    |   f|        rf|    rf(%)|  cf|     cf(%)|
    ## |:--------|---:|---------:|--------:|---:|---------:|
    ## |Negativo | 223| 0,6126374| 61,26374| 223|  61,26374|
    ## |Positivo | 141| 0,3873626| 38,73626| 364| 100,00000|

------------------------------------------------------------------------

> 1.  **Distribuições de Frequências para as Variáveis Qualitativas
>     Ordinais**

``` r
# Distribuição de Frequências para as Variáveis Qualitativas Ordinais
for(i in var.fct_ord){
      fdt_cat(aed.trn[,i]) %>%
          tibble() -> df
    names(df)[1] = c(colnames(aed.trn[i]))
    print(kable(df[order(df[2], decreasing = TRUE),]))
}
```

------------------------------------------------------------------------

> 1.  **Gráfico de Barras: Variáveis Qualitativas**

``` r
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

![](project_files/figure-markdown_github/Barras:%20Variáveis%20Categóricas-1.png)![](project_files/figure-markdown_github/Barras:%20Variáveis%20Categóricas-2.png)![](project_files/figure-markdown_github/Barras:%20Variáveis%20Categóricas-3.png)![](project_files/figure-markdown_github/Barras:%20Variáveis%20Categóricas-4.png)

------------------------------------------------------------------------

#### Variáveis Quantitativas

-   Discretas

-   Contínuas

    > Agrupam-se os valores das variáveis em classes e obtêm-se as
    > frequências em cada classe.

As distribuições podem ser representadas por meio de:

-   Gráfico de Dispersão Unidimensional (dotplot)

-   Gráfico Ramo e Folhas (Steam and Leaf)

-   Histograma

> 1.  **Distribuição de Frequências: Variáveis Quantitativas**

``` r
par(mfrow=c(2,1))
# Distribuição de Frequências: Variáveis Quantitativas
for(i in var.num){
        df <- fdt(aed.trn[,i])
        #names(df)[[1]] = c(colnames(aed.trn[i]))
        print(kable(df, caption = "Title of the table"))
}
```

    ## 
    ## 
    ## <table class="kable_wrapper">
    ## <caption>Title of the table</caption>
    ## <tbody>
    ##   <tr>
    ##    <td> 
    ## 
    ## |Class limits  |  f|        rf|      rf(%)|  cf|       cf(%)|
    ## |:-------------|--:|---------:|----------:|---:|-----------:|
    ## |[15,84,23,35) |  1| 0,0027473|  0,2747253|   1|   0,2747253|
    ## |[23,35,30,85) | 31| 0,0851648|  8,5164835|  32|   8,7912088|
    ## |[30,85,38,36) | 50| 0,1373626| 13,7362637|  82|  22,5274725|
    ## |[38,36,45,86) | 70| 0,1923077| 19,2307692| 152|  41,7582418|
    ## |[45,86,53,37) | 82| 0,2252747| 22,5274725| 234|  64,2857143|
    ## |[53,37,60,88) | 72| 0,1978022| 19,7802198| 306|  84,0659341|
    ## |[60,88,68,38) | 43| 0,1181319| 11,8131868| 349|  95,8791209|
    ## |[68,38,75,89) | 11| 0,0302198|  3,0219780| 360|  98,9010989|
    ## |[75,89,83,39) |  1| 0,0027473|  0,2747253| 361|  99,1758242|
    ## |[83,39,90,9)  |  3| 0,0082418|  0,8241758| 364| 100,0000000|
    ## 
    ##  </td>
    ##    <td> 
    ## 
    ## |      |      x|
    ## |:-----|------:|
    ## |start | 15,840|
    ## |end   | 90,900|
    ## |h     |  7,506|
    ## |right |  0,000|
    ## 
    ##  </td>
    ##   </tr>
    ## </tbody>
    ## </table>

------------------------------------------------------------------------

##### Gráficos

``` r
par(mfrow=c(1,1))
# Gráfico de Dispersão Unidimensional: Variáveis Quantitativas
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        dotchart(aed.trn[,i], main = paste("Dispersão Unidimensional:", names(aed.trn)[i]))
    }
} 
```

![](project_files/figure-markdown_github/Dispersao%20Unidimensional-1.png)

``` r
# Gráfico Ramo e Folhas: Variáveis Quantitativas
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        stem(aed.trn[,i])
    }
}
```

    ## 
    ##   The decimal point is 1 digit(s) to the right of the |
    ## 
    ##   1 | 6
    ##   2 | 
    ##   2 | 5777777888888889
    ##   3 | 00000000000000012223344
    ##   3 | 5555555555555555555566666677777888888888889999999999
    ##   4 | 000000000000000111122222223333333333333333334444
    ##   4 | 55555555555566666777777777777777778888888888888888888889999999
    ##   5 | 000000000000111112223333333333334444444444
    ##   5 | 555555555555555555666666777777777777778888888889999
    ##   6 | 000000000001111112222234444
    ##   6 | 5555566666777777778888888889
    ##   7 | 0000222222
    ##   7 | 9
    ##   8 | 
    ##   8 | 5
    ##   9 | 00

``` r
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

![](project_files/figure-markdown_github/Histograma:%20Quantitativas-1.png)

------------------------------------------------------------------------

### Medidas de Posição

São as estatísticas que representam uma série de dados orientando-nos
quanto à posição da distribuição em relação ao eixo horizontal (eixo
“x”) do gráfico da curva de freqüência. As medidas de posição mais
importantes são as medidas de tendência central, no qual se verifica uma
tendência dos dados observados a se agruparem em torno dos valores
centrais.

``` r
  stargazer(aed.trn, median = T, mean.sd = T, iqr = T, type = "text", title = "Sumário Estatístico")
```

    ## 
    ## Sumário Estatístico
    ## ======================================================================
    ## Statistic  N     Mean      St. Dev.   Min Pctl(25) Median Pctl(75) Max
    ## ----------------------------------------------------------------------
    ## Age       364 48,,505.000 12,,187.000 16     39      48      57    90 
    ## ----------------------------------------------------------------------

------------------------------------------------------------------------

***Moda***

Moda - A moda é especialmente útil para dados qualitativos. Não é
possível analisar a média ou mediana de dados não ordenados, como cidade
ou preferência musical. Então a moda entra em ação.

``` r
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

| Variável           | Moda     |
|:-------------------|:---------|
| Gender             | Mulher   |
| Polyuria           | Não      |
| Polydipsia         | Não      |
| sudden.weight.loss | Não      |
| weakness           | Sim      |
| Polyphagia         | Não      |
| Genital.thrush     | Não      |
| visual.blurring    | Não      |
| Itching            | Não      |
| Irritability       | Não      |
| delayed.healing    | Não      |
| partial.paresis    | Não      |
| muscle.stiffness   | Não      |
| Alopecia           | Não      |
| Obesity            | Não      |
| class              | Negativo |

### Medidas Separatrizes

------------------------------------------------------------------------

> **Quartis**

``` r
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

    ##      Age
    ## 0%    16
    ## 25%   39
    ## 50%   48
    ## 75%   57
    ## 100%  90

------------------------------------------------------------------------

> **Quintis**

``` r
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

    ##      Age
    ## 20% 38,0
    ## 40% 45,0
    ## 60% 51,0
    ## 80% 58,4

------------------------------------------------------------------------

> **Decis**

``` r
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

    ##      Age
    ## 10% 33,0
    ## 20% 38,0
    ## 30% 41,0
    ## 40% 45,0
    ## 50% 48,0
    ## 60% 51,0
    ## 70% 55,0
    ## 80% 58,4
    ## 90% 65,7

------------------------------------------------------------------------

> **Percentis**

``` r
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

    ##       Age
    ## 1%  27,00
    ## 2%  27,26
    ## 3%  28,00
    ## 4%  28,00
    ## 5%  30,00
    ## 6%  30,00
    ## 7%  30,00
    ## 8%  30,00
    ## 9%  31,67
    ## 10% 33,00
    ## 11% 34,93
    ## 12% 35,00
    ## 13% 35,00
    ## 14% 35,00
    ## 15% 35,00
    ## 16% 35,00
    ## 17% 36,00
    ## 18% 36,34
    ## 19% 37,00
    ## 20% 38,00
    ## 21% 38,00
    ## 22% 38,00
    ## 23% 39,00
    ## 24% 39,00
    ## 25% 39,00
    ## 26% 40,00
    ## 27% 40,00
    ## 28% 40,00
    ## 29% 40,00
    ## 30% 41,00
    ## 31% 42,00
    ## 32% 42,00
    ## 33% 43,00
    ## 34% 43,00
    ## 35% 43,00
    ## 36% 43,00
    ## 37% 43,00
    ## 38% 44,00
    ## 39% 45,00
    ## 40% 45,00
    ## 41% 45,00
    ## 42% 46,00
    ## 43% 46,09
    ## 44% 47,00
    ## 45% 47,00
    ## 46% 47,00
    ## 47% 47,00
    ## 48% 48,00
    ## 49% 48,00
    ## 50% 48,00
    ## 51% 48,00
    ## 52% 48,00
    ## 53% 48,00
    ## 54% 49,00
    ## 55% 49,00
    ## 56% 50,00
    ## 57% 50,00
    ## 58% 50,00
    ## 59% 51,00
    ## 60% 51,00
    ## 61% 52,43
    ## 62% 53,00
    ## 63% 53,00
    ## 64% 53,00
    ## 65% 54,00
    ## 66% 54,00
    ## 67% 54,21
    ## 68% 55,00
    ## 69% 55,00
    ## 70% 55,00
    ## 71% 55,00
    ## 72% 55,36
    ## 73% 56,00
    ## 74% 57,00
    ## 75% 57,00
    ## 76% 57,00
    ## 77% 57,00
    ## 78% 58,00
    ## 79% 58,00
    ## 80% 58,40
    ## 81% 59,03
    ## 82% 60,00
    ## 83% 60,00
    ## 84% 60,00
    ## 85% 61,00
    ## 86% 62,00
    ## 87% 62,00
    ## 88% 64,00
    ## 89% 65,00
    ## 90% 65,70
    ## 91% 66,00
    ## 92% 67,00
    ## 93% 67,00
    ## 94% 68,00
    ## 95% 68,00
    ## 96% 68,48
    ## 97% 70,00
    ## 98% 72,00
    ## 99% 74,59

------------------------------------------------------------------------

> **Boxplot**

O boxplot é um gráfico baseado nos quantis que serve como alternativa ao
histograma para resumir a distribuição das dados. Esse gráfico permite
que identifiquemos a posição dos 50% centrais dos dados (entre o
primeiro e terceiro quartis), a posição da mediana, os valores atípicos,
se existirem, assim como permite uma avaliação da simetria da
distribuição. Boxplots são úteis para a comparação de vários conjuntos
de dados.

``` r
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

![](project_files/figure-markdown_github/Boxplot-1.png)

------------------------------------------------------------------------

### Medidas de Dispersão

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
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
      amp <- diff(range(aed.trn[,i]))
      print(amp)
    }
}
```

    ## [1] 74

------------------------------------------------------------------------

> **Variância**

    A variância é a medida de dispersão mais empregada geralmente, pois leva em consideração a totalidade dos valores da variável em estudo. Baseia-se nos desvios em torno da média aritmética, sendo um indicador de variabilidade.

    Para medir o grau de variabilidade dos valores em torno da média, nada mais interessante do que estudarmos o comportamento dos desvios de cada valor individual da série em relação à média.

    Queremos calcular a média dos desvios, porém sua soma pode ser nula. Como solução a esse problema a variância considera o quadrado de cada desvio evitando com isso que o somatório seja nulo.

``` r
# Variância
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        print(
            var(aed.trn[,i])
            )
    }
}
```

    ## [1] 148,5261

------------------------------------------------------------------------

> **Desvio Padrão**

    Seguindo a mesma linha de raciocínio usado para o cálculo da variância, necessitamos, agora, aproximar a medida de dispersão da variável original. Para isso, calculamos o desvio padrão, que é a raiz quadrada da variância.

    Podemos representar o desvio padrão por uma distribuição normal:

    - 68,26% das ocorrências se concentrarão na área do gráfico demarcada por um
    desvio padrão à direita e um desvio padrão à esquerda da linha média;

    - 95,44% das ocorrências estão a dois desvios padrão, para a direita e a esquerda da média e, finalmente;

    - 99,72% das ocorrências ocorrem a três desvios padrão ao redor da média aritmética.

``` r
# Desvio Padrão
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        print(
            sd(aed.trn[,i])
        )
    }
}
```

    ## [1] 12,18713

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
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        cv <- 100*sd(aed.trn[,i]/mean(aed.trn[,i]))
        print(cv)
    }
}
```

    ## [1] 25,12526

------------------------------------------------------------------------

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

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](project_files/figure-markdown_github/Histograma-1.png)

``` r
# Coeficiente de Assimetria (Skew)
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        ca <- skewness(aed.trn[,i])
        print(ca)
    }
}
```

    ## [1] 0,2804123

``` r
# Coeficiente de Curtose
for(i in 1:ncol(aed.trn)){
    if(is.numeric(aed.trn[,i])){
        ck <- kurtosis(aed.trn[,i])
        print(ck)
    }
}
```

    ## [1] -0,09548968

------------------------------------------------------------------------

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

------------------------------------------------------------------------

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

------------------------------------------------------------------------

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
  CrossTable(aed.trn$class, 
             aed.trn[,i],
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
    ## Total Observations in Table:  364 
    ## 
    ##  
    ##              | Polyuria 
    ##        class |       Não |       Sim | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Positivo |       130 |        11 |       141 | 
    ##              |     40,04 |     45,69 |           | 
    ##              |      0,92 |      0,08 |      0,39 | 
    ##              |      0,67 |      0,06 |           | 
    ##              |      0,36 |      0,03 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Negativo |        64 |       159 |       223 | 
    ##              |     25,31 |     28,89 |           | 
    ##              |      0,29 |      0,71 |      0,61 | 
    ##              |      0,33 |      0,94 |           | 
    ##              |      0,18 |      0,44 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       194 |       170 |       364 | 
    ##              |      0,53 |      0,47 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  139,9294     d.f. =  1     p =  2,758406e-32 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  137,3899     d.f. =  1     p =  9,908517e-32 
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
    ## Total Observations in Table:  364 
    ## 
    ##  
    ##              | Polydipsia 
    ##        class |       Não |       Sim | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Positivo |       134 |         7 |       141 | 
    ##              |     42,02 |     50,68 |           | 
    ##              |      0,95 |      0,05 |      0,39 | 
    ##              |      0,67 |      0,04 |           | 
    ##              |      0,37 |      0,02 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Negativo |        65 |       158 |       223 | 
    ##              |     26,57 |     32,05 |           | 
    ##              |      0,29 |      0,71 |      0,61 | 
    ##              |      0,33 |      0,96 |           | 
    ##              |      0,18 |      0,43 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       199 |       165 |       364 | 
    ##              |      0,55 |      0,45 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  151,3192     d.f. =  1     p =  8,925156e-35 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  148,6722     d.f. =  1     p =  3,382136e-34 
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
    ## Total Observations in Table:  364 
    ## 
    ##  
    ##              | sudden.weight.loss 
    ##        class |       Não |       Sim | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Positivo |       124 |        17 |       141 | 
    ##              |     16,80 |     26,26 |           | 
    ##              |      0,88 |      0,12 |      0,39 | 
    ##              |      0,56 |      0,12 |           | 
    ##              |      0,34 |      0,05 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Negativo |        98 |       125 |       223 | 
    ##              |     10,62 |     16,60 |           | 
    ##              |      0,44 |      0,56 |      0,61 | 
    ##              |      0,44 |      0,88 |           | 
    ##              |      0,27 |      0,34 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       222 |       142 |       364 | 
    ##              |      0,61 |      0,39 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  70,27998     d.f. =  1     p =  5,145726e-17 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  68,44294     d.f. =  1     p =  1,306026e-16 
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
    ## Total Observations in Table:  364 
    ## 
    ##  
    ##              | weakness 
    ##        class |       Não |       Sim | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Positivo |        82 |        59 |       141 | 
    ##              |      8,03 |      5,96 |           | 
    ##              |      0,58 |      0,42 |      0,39 | 
    ##              |      0,53 |      0,28 |           | 
    ##              |      0,23 |      0,16 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Negativo |        73 |       150 |       223 | 
    ##              |      5,08 |      3,77 |           | 
    ##              |      0,33 |      0,67 |      0,61 | 
    ##              |      0,47 |      0,72 |           | 
    ##              |      0,20 |      0,41 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       155 |       209 |       364 | 
    ##              |      0,43 |      0,57 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  22,83069     d.f. =  1     p =  1,769178e-06 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  21,80282     d.f. =  1     p =  3,021555e-06 
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
    ## Total Observations in Table:  364 
    ## 
    ##  
    ##              | Polyphagia 
    ##        class |       Não |       Sim | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Positivo |       110 |        31 |       141 | 
    ##              |     12,51 |     15,77 |           | 
    ##              |      0,78 |      0,22 |      0,39 | 
    ##              |      0,54 |      0,19 |           | 
    ##              |      0,30 |      0,09 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Negativo |        93 |       130 |       223 | 
    ##              |      7,91 |      9,97 |           | 
    ##              |      0,42 |      0,58 |      0,61 | 
    ##              |      0,46 |      0,81 |           | 
    ##              |      0,26 |      0,36 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       203 |       161 |       364 | 
    ##              |      0,56 |      0,44 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  46,16996     d.f. =  1     p =  1,084268e-11 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  44,70969     d.f. =  1     p =  2,285237e-11 
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
    ## Total Observations in Table:  364 
    ## 
    ##  
    ##              | Genital.thrush 
    ##        class |       Não |       Sim | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Positivo |       117 |        24 |       141 | 
    ##              |      0,31 |      1,14 |           | 
    ##              |      0,83 |      0,17 |      0,39 | 
    ##              |      0,41 |      0,31 |           | 
    ##              |      0,32 |      0,07 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Negativo |       170 |        53 |       223 | 
    ##              |      0,19 |      0,72 |           | 
    ##              |      0,76 |      0,24 |      0,61 | 
    ##              |      0,59 |      0,69 |           | 
    ##              |      0,47 |      0,15 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       287 |        77 |       364 | 
    ##              |      0,79 |      0,21 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  2,356601     d.f. =  1     p =  0,1247537 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  1,96952     d.f. =  1     p =  0,1604988 
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
    ## Total Observations in Table:  364 
    ## 
    ##  
    ##              | visual.blurring 
    ##        class |       Não |       Sim | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Positivo |       100 |        41 |       141 | 
    ##              |      6,81 |      8,22 |           | 
    ##              |      0,71 |      0,29 |      0,39 | 
    ##              |      0,50 |      0,25 |           | 
    ##              |      0,27 |      0,11 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Negativo |        99 |       124 |       223 | 
    ##              |      4,31 |      5,19 |           | 
    ##              |      0,44 |      0,56 |      0,61 | 
    ##              |      0,50 |      0,75 |           | 
    ##              |      0,27 |      0,34 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       199 |       165 |       364 | 
    ##              |      0,55 |      0,45 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  24,52882     d.f. =  1     p =  7,320668e-07 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  23,47006     d.f. =  1     p =  1,268727e-06 
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
    ## Total Observations in Table:  364 
    ## 
    ##  
    ##              | Itching 
    ##        class |       Não |       Sim | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Positivo |        74 |        67 |       141 | 
    ##              |      0,01 |      0,01 |           | 
    ##              |      0,52 |      0,48 |      0,39 | 
    ##              |      0,38 |      0,39 |           | 
    ##              |      0,20 |      0,18 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Negativo |       119 |       104 |       223 | 
    ##              |      0,00 |      0,01 |           | 
    ##              |      0,53 |      0,47 |      0,61 | 
    ##              |      0,62 |      0,61 |           | 
    ##              |      0,33 |      0,29 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       193 |       171 |       364 | 
    ##              |      0,53 |      0,47 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  0,02691433     d.f. =  1     p =  0,8696872 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  0,003165711     d.f. =  1     p =  0,955131 
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
    ## Total Observations in Table:  364 
    ## 
    ##  
    ##              | Irritability 
    ##        class |       Não |       Sim | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Positivo |       128 |        13 |       141 | 
    ##              |      3,99 |     12,72 |           | 
    ##              |      0,91 |      0,09 |      0,39 | 
    ##              |      0,46 |      0,15 |           | 
    ##              |      0,35 |      0,04 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Negativo |       149 |        74 |       223 | 
    ##              |      2,53 |      8,04 |           | 
    ##              |      0,67 |      0,33 |      0,61 | 
    ##              |      0,54 |      0,85 |           | 
    ##              |      0,41 |      0,20 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       277 |        87 |       364 | 
    ##              |      0,76 |      0,24 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  27,27375     d.f. =  1     p =  1,765924e-07 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  25,97213     d.f. =  1     p =  3,463824e-07 
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
    ## Total Observations in Table:  364 
    ## 
    ##  
    ##              | delayed.healing 
    ##        class |       Não |       Sim | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Positivo |        85 |        56 |       141 | 
    ##              |      0,20 |      0,27 |           | 
    ##              |      0,60 |      0,40 |      0,39 | 
    ##              |      0,41 |      0,36 |           | 
    ##              |      0,23 |      0,15 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Negativo |       124 |        99 |       223 | 
    ##              |      0,13 |      0,17 |           | 
    ##              |      0,56 |      0,44 |      0,61 | 
    ##              |      0,59 |      0,64 |           | 
    ##              |      0,34 |      0,27 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       209 |       155 |       364 | 
    ##              |      0,57 |      0,43 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  0,7732586     d.f. =  1     p =  0,3792109 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  0,5937522     d.f. =  1     p =  0,4409718 
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
    ## Total Observations in Table:  364 
    ## 
    ##  
    ##              | partial.paresis 
    ##        class |       Não |       Sim | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Positivo |       121 |        20 |       141 | 
    ##              |     19,33 |     26,36 |           | 
    ##              |      0,86 |      0,14 |      0,39 | 
    ##              |      0,58 |      0,13 |           | 
    ##              |      0,33 |      0,05 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Negativo |        89 |       134 |       223 | 
    ##              |     12,22 |     16,67 |           | 
    ##              |      0,40 |      0,60 |      0,61 | 
    ##              |      0,42 |      0,87 |           | 
    ##              |      0,24 |      0,37 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       210 |       154 |       364 | 
    ##              |      0,58 |      0,42 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  74,57801     d.f. =  1     p =  5,828885e-18 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  72,70914     d.f. =  1     p =  1,502365e-17 
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
    ## Total Observations in Table:  364 
    ## 
    ##  
    ##              | muscle.stiffness 
    ##        class |       Não |       Sim | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Positivo |        98 |        43 |       141 | 
    ##              |      0,60 |      1,07 |           | 
    ##              |      0,70 |      0,30 |      0,39 | 
    ##              |      0,42 |      0,33 |           | 
    ##              |      0,27 |      0,12 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Negativo |       136 |        87 |       223 | 
    ##              |      0,38 |      0,68 |           | 
    ##              |      0,61 |      0,39 |      0,61 | 
    ##              |      0,58 |      0,67 |           | 
    ##              |      0,37 |      0,24 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       234 |       130 |       364 | 
    ##              |      0,64 |      0,36 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  2,729225     d.f. =  1     p =  0,09852708 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  2,370868     d.f. =  1     p =  0,1236183 
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
    ## Total Observations in Table:  364 
    ## 
    ##  
    ##              | Alopecia 
    ##        class |       Não |       Sim | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Positivo |        75 |        66 |       141 | 
    ##              |      4,17 |      8,59 |           | 
    ##              |      0,53 |      0,47 |      0,39 | 
    ##              |      0,31 |      0,55 |           | 
    ##              |      0,21 |      0,18 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Negativo |       170 |        53 |       223 | 
    ##              |      2,64 |      5,43 |           | 
    ##              |      0,76 |      0,24 |      0,61 | 
    ##              |      0,69 |      0,45 |           | 
    ##              |      0,47 |      0,15 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       245 |       119 |       364 | 
    ##              |      0,67 |      0,33 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  20,84208     d.f. =  1     p =  4,987493e-06 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  19,8081     d.f. =  1     p =  8,561917e-06 
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
    ## Total Observations in Table:  364 
    ## 
    ##  
    ##              | Obesity 
    ##        class |       Não |       Sim | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Positivo |       120 |        21 |       141 | 
    ##              |      0,08 |      0,38 |           | 
    ##              |      0,85 |      0,15 |      0,39 | 
    ##              |      0,40 |      0,34 |           | 
    ##              |      0,33 |      0,06 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Negativo |       182 |        41 |       223 | 
    ##              |      0,05 |      0,24 |           | 
    ##              |      0,82 |      0,18 |      0,61 | 
    ##              |      0,60 |      0,66 |           | 
    ##              |      0,50 |      0,11 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       302 |        62 |       364 | 
    ##              |      0,83 |      0,17 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  0,7453898     d.f. =  1     p =  0,3879398 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  0,5187639     d.f. =  1     p =  0,4713695 
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
    ## Total Observations in Table:  364 
    ## 
    ##  
    ##              | class 
    ##        class |  Positivo |  Negativo | Row Total | 
    ## -------------|-----------|-----------|-----------|
    ##     Positivo |       141 |         0 |       141 | 
    ##              |    136,62 |     86,38 |           | 
    ##              |      1,00 |      0,00 |      0,39 | 
    ##              |      1,00 |      0,00 |           | 
    ##              |      0,39 |      0,00 |           | 
    ## -------------|-----------|-----------|-----------|
    ##     Negativo |         0 |       223 |       223 | 
    ##              |     86,38 |     54,62 |           | 
    ##              |      0,00 |      1,00 |      0,61 | 
    ##              |      0,00 |      1,00 |           | 
    ##              |      0,00 |      0,61 |           | 
    ## -------------|-----------|-----------|-----------|
    ## Column Total |       141 |       223 |       364 | 
    ##              |      0,39 |      0,61 |           | 
    ## -------------|-----------|-----------|-----------|
    ## 
    ##  
    ## Statistics for All Table Factors
    ## 
    ## 
    ## Pearson's Chi-squared test 
    ## ------------------------------------------------------------
    ## Chi^2 =  364     d.f. =  1     p =  3,789733e-81 
    ## 
    ## Pearson's Chi-squared test with Yates' continuity correction 
    ## ------------------------------------------------------------
    ## Chi^2 =  359,7983     d.f. =  1     p =  3,115251e-80 
    ## 
    ## 

------------------------------------------------------------------------

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

------------------------------------------------------------------------

#### Diagramas de Dispersão

A ***relação*** entre as variáveis pode ser *fortemente linear*, *não
linear* ou mesmo *inexistente*. Portanto, um diagrama de dispersão ***é
uma primeira indicação*** útil da possível existência ***de uma
associação entre duas variáveis***.

``` r
# Diagramas de Dispersão - Matriz de Correlação
plot(aed.trn[])
```

![](project_files/figure-markdown_github/Matriz%20de%20Correlação-1.png)

------------------------------------------------------------------------

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
# Tabela dos Coeficientes de Correlação
cor(aed.trn[var.num])
```

    ##     Age
    ## Age   1

------------------------------------------------------------------------

#### Covariância

------------------------------------------------------------------------

### Uma Númerica e outra Categórica

Em geral ***analisa-se o que acontece com a variável quantitativa dentro
de cada categoria da variável qualitativa.***. Essa análise pode ser
conduzida por meio de ***medidas-resumo*** ou ***box plot***.

As medidas-resumo são agrupadas por categoria da variável qualitativa. A
partir daí, constroem-se boxplots baseados em cada medida-resumo. Então,
os boxplots são comparados visualmente.

------------------------------------------------------------------------

#### Medidas-resumo

*As medidas-resumo são calculadas para a variável quantitativa, a
variável que se quer observar o comportamento.*

------------------------------------------------------------------------

#### Boxplot

``` r
# Boxplots Idade conforme as Categóricas
par(mfrow=c(1,3), cex = 0.65)
for(i in var.fct){
  boxplot(aed.trn$Age ~ aed.trn[,i], beside = TRUE, xlab = names(aed.trn)[grep(i, names(aed.trn))])
}
```

![](project_files/figure-markdown_github/Gráficos%20de%20Caixa%20Age%20x%20Categóricas-1.png)![](project_files/figure-markdown_github/Gráficos%20de%20Caixa%20Age%20x%20Categóricas-2.png)![](project_files/figure-markdown_github/Gráficos%20de%20Caixa%20Age%20x%20Categóricas-3.png)![](project_files/figure-markdown_github/Gráficos%20de%20Caixa%20Age%20x%20Categóricas-4.png)![](project_files/figure-markdown_github/Gráficos%20de%20Caixa%20Age%20x%20Categóricas-5.png)![](project_files/figure-markdown_github/Gráficos%20de%20Caixa%20Age%20x%20Categóricas-6.png)

``` r
# Boxplots Numéricas conforme as Survived
# par(mfrow=c(2,2))
for(i in 0:length(aed.trn)){
  if(is.numeric(aed.trn[,i])){
    boxplot(aed.trn[,i] ~ class, ylab = names(aed.trn)[i], data = aed.trn)
  }
}
```

![](project_files/figure-markdown_github/Gráficos%20de%20Caixa%20Numéricas%20x%20Survived-1.png)

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

``` r
dput(aed.trn, file = "datasets/aed_trn.R")
```

------------------------------------------------------------------------

# Pré-Processamento

------------------------------------------------------------------------

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

    1.  **Re-escalonar (Normalização Min-Max):** é o método mais simples
        e consiste em redimensionar o intervalo de recursos para
        dimensionar o intervalo em \[0, 1\] ou \[-1, 1\].

    2.  **Escore-Z (Padronização):** No aprendizado de máquina, podemos
        lidar com vários tipos de dados, por exemplo, sinais de áudio e
        valores de pixel para dados de imagem, e esses dados podem
        incluir várias dimensões. *A padronização de recursos faz com
        que os valores de cada recurso nos dados tenham média zero* (ao
        subtrair a média no numerador) *e variação de unidade*. Esse
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

## Feature Selection

# Modelagem

``` r
#massa <- z.trn
#mdl.tst <- z.tst
massa <- aed.trn
mdl.tst <- etl.tst
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
               family = binomial(link = "logit"), 
               data = mdl.trn)
```

**Resumo do modelo treinado:**

``` r
summary(ajt.trn)
```

    ## 
    ## Call:
    ## glm(formula = class ~ ., family = binomial(link = "logit"), data = mdl.trn)
    ## 
    ## Deviance Residuals: 
    ##      Min        1Q    Median        3Q       Max  
    ## -2,68067  -0,13619   0,00011   0,01543   2,86335  
    ## 
    ## Coefficients:
    ##                       Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)            7,72194    2,44800   3,154 0,001608 ** 
    ## Age                   -0,12597    0,04820  -2,613 0,008966 ** 
    ## GenderMulher          -7,34476    1,39188  -5,277 1,31e-07 ***
    ## PolyuriaSim            6,63816    1,56514   4,241 2,22e-05 ***
    ## PolydipsiaSim          8,51908    1,81503   4,694 2,68e-06 ***
    ## sudden.weight.lossSim  1,18343    1,03481   1,144 0,252782    
    ## weaknessSim           -0,05447    0,99314  -0,055 0,956258    
    ## PolyphagiaSim          2,62843    1,05133   2,500 0,012416 *  
    ## Genital.thrushSim      1,34283    1,00844   1,332 0,182995    
    ## visual.blurringSim     0,60005    1,05789   0,567 0,570569    
    ## ItchingSim            -4,27563    1,39418  -3,067 0,002164 ** 
    ## IrritabilitySim        3,56113    1,06187   3,354 0,000798 ***
    ## delayed.healingSim    -0,71266    0,99647  -0,715 0,474496    
    ## partial.paresisSim     1,33057    0,86951   1,530 0,125953    
    ## muscle.stiffnessSim   -1,10156    1,21540  -0,906 0,364758    
    ## AlopeciaSim            0,96801    1,17464   0,824 0,409889    
    ## ObesitySim            -0,61650    0,88646  -0,695 0,486768    
    ## ---
    ## Signif. codes:  0 '***' 0,001 '**' 0,01 '*' 0,05 '.' 0,1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 418,574  on 312  degrees of freedom
    ## Residual deviance:  70,048  on 296  degrees of freedom
    ## AIC: 104,05
    ## 
    ## Number of Fisher Scoring iterations: 9

------------------------------------------------------------------------

## Validação

------------------------------------------------------------------------

> Validando o modelo através de predições utilizando o *Conjunto de
> Validação*.

``` r
pred <- predict( ajt.trn, 
                     newdata = mdl.vld,
                     type = "response" )

# Criando um dataframe com os dados observados e os preditos.
previsoes <- data.frame(observado = mdl.vld$class,
                        previsto = pred %>% 
                                      round() %>% 
                                      factor(labels = c("Positivo", "Negativo")))
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
    ## Total Observations in Table:  51 
    ## 
    ##  
    ##                     | previsoes$previsto 
    ## previsoes$observado |  Positivo |  Negativo | Row Total | 
    ## --------------------|-----------|-----------|-----------|
    ##            Positivo |        17 |         2 |        19 | 
    ##                     |     8,296 |     6,815 |           | 
    ##                     |     0,895 |     0,105 |     0,373 | 
    ##                     |     0,739 |     0,071 |           | 
    ##                     |     0,333 |     0,039 |           | 
    ## --------------------|-----------|-----------|-----------|
    ##            Negativo |         6 |        26 |        32 | 
    ##                     |     4,926 |     4,046 |           | 
    ##                     |     0,188 |     0,812 |     0,627 | 
    ##                     |     0,261 |     0,929 |           | 
    ##                     |     0,118 |     0,510 |           | 
    ## --------------------|-----------|-----------|-----------|
    ##        Column Total |        23 |        28 |        51 | 
    ##                     |     0,451 |     0,549 |           | 
    ## --------------------|-----------|-----------|-----------|
    ## 
    ## 

``` r
chisq.test(previsoes$observado, 
           previsoes$previsto)
```

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  previsoes$observado and previsoes$previsto
    ## X-squared = 21,312, df = 1, p-value = 3,903e-06

![](project_files/figure-markdown_github/unnamed-chunk-2-1.png)

### Avaliação

Para avaliar o desempenho do modelo nos dados de validação, foi
construída uma *matriz de confusão*.

``` r
cm <- confusionMatrix(previsoes$observado, 
                      previsoes$previsto, 
                      positive = 'Positivo')
```

Matriz de Confusão - Usando o pacote Caret

``` r
print(cm)
```

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction Positivo Negativo
    ##   Positivo       17        2
    ##   Negativo        6       26
    ##                                           
    ##                Accuracy : 0,8431          
    ##                  95% CI : (0,7141, 0,9298)
    ##     No Information Rate : 0,549           
    ##     P-Value [Acc > NIR] : 8,783e-06       
    ##                                           
    ##                   Kappa : 0,6782          
    ##                                           
    ##  Mcnemar's Test P-Value : 0,2888          
    ##                                           
    ##             Sensitivity : 0,7391          
    ##             Specificity : 0,9286          
    ##          Pos Pred Value : 0,8947          
    ##          Neg Pred Value : 0,8125          
    ##              Prevalence : 0,4510          
    ##          Detection Rate : 0,3333          
    ##    Detection Prevalence : 0,3725          
    ##       Balanced Accuracy : 0,8339          
    ##                                           
    ##        'Positive' Class : Positivo        
    ## 

Accuracy

“Através da *Matriz de Confusão* obtivemos a Acurácia de ***84,31%***,
ela corresponde a fração das premissas corretas em relação ao total de
observações. Esta métrica também poderia ser calculada utilizando a
função *table()* ou a função *CrossTable()*, pois ela corresponde a soma
das predições corretas dividida pelo total de observações.  
Porém, não é suficiente (e nem segura) para avaliarmos a eficiência do
modelo. Analisamos então, outras métricas obtidas pela Confusion Matrix,
que nos informe não apenas o percentual de acertos, mas também a
precisão e a sensibilidade.”

***95% CI***

O ***Intervalo de Confiança***, denotado por *95% CI*, é ***\[0,7141,
0,9298\]***. Ele é uma *estimativa por intervalo* de um *parâmetro
populacional*, que com dada frequência (*Nível de Confiança*), inclui o
parâmetro de interesse. Nesse caso específico, significa dizer que 95%
dos *intervalos de confiança* observados têm o valor real do parâmetro.

***No Information Rate***

***P-Value \[Acc \> NIR\]***

***Kappa***

O ***Teste de Concordância Kappa*** foi igual a ***0,6782***. O
coeficiente de Kappa tem a finalidade de medir o grau de concordância
entre proporções. Ele demonstra se uma dada classificação pode ser
considerada confiável.

O seu valor pode ser interpretado por meio de uma tabela.

Mcnemar’s Test P-Value

Sensitivity

Specificity

Pos Pred Value

Neg Pred Value

Prevalence

Detection Rate

Detection Prevalence

Balanced Accuracy

``` r
fourfoldplot(cm$table)
```

![](project_files/figure-markdown_github/Plot%20Confusion%20Matrix-1.png)

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

![](project_files/figure-markdown_github/Mosaico%20Confusion%20Matrix-1.png)

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
metricas <- kable(
              data.frame(
                "Acuracia" = acuracia, 
                "Precisao" = precisao, 
                "Sensibilidade" = sensibilidade,
                "F1-Score" = f1), format = "markdown", align = 'l')
metricas
```

| Acuracia  | Precisao  | Sensibilidade | F1.Score  |
|:----------|:----------|:--------------|:----------|
| 0,8431373 | 0,8947368 | 0,7391304     | 0,8095238 |

#### Curva ROC

``` r
class1 <- predict(ajt.trn, newdata = mdl.vld, type = "response")
class2 <- mdl.vld$class

pred <- prediction(class1, class2)
perf <- performance(pred, "tpr", "fpr")
```

``` r
# Gerando uma curva ROC em R
plot(perf, col = rainbow(10, alpha = NULL))
```

![Gerando uma curva ROC em
R](project_files/figure-markdown_github/Output%20ROC-1.png)

------------------------------------------------------------------------

Realizando o ***Teste Chi-Quadrado*** (X²).

``` r
chisq.test(previsoes$observado, previsoes$previsto)
```

    ## 
    ##  Pearson's Chi-squared test with Yates' continuity correction
    ## 
    ## data:  previsoes$observado and previsoes$previsto
    ## X-squared = 21,312, df = 1, p-value = 3,903e-06

------------------------------------------------------------------------

## Teste

Predição de teste utilizando novos dados.

``` r
predict(ajt.trn, 
        newdata = mdl.tst, 
        type = "response") %>% 
  round() %>% 
  factor(labels = c("Não", "Sim")) -> predicao
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
    ## Total Observations in Table:  156 
    ## 
    ##  
    ##           |       Não |       Sim | 
    ##           |-----------|-----------|
    ##           |        60 |        96 | 
    ##           |      0,38 |      0,62 | 
    ##           |-----------|-----------|
    ## 
    ## 
    ## 
    ## 