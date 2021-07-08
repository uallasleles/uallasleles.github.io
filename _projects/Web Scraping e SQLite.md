---
name: Web Scraping e SQLite
tools: [Python, SQLite, JupyterLab]
image:
description: Obtendo dados de páginas web e salvando em um banco de dados.

---


# Web Scraping e SQLite
---


<i>Author: Uallas Leles</i><br>
<i>Data: 23/06/2021</i>

### Obtendo dados de páginas web e salvando em um banco de dados

O objetivo deste projeto é obter dados de páginas web através de web scraping e salvá-los em um banco de dados SQLite.<br>
Neste trabalho, especificamente, não há preocupação na relevância dos dados ou na exploração de insights. A finalidade é construir um workflow básico que faça a extração e estruturação dos dados para etapas seguintes.

#### Definição do Problema
- Vamos obter dados de listas de repositórios no GitHub através de web scraping, e então salvá-los em um banco de dados.

## Mão na Massa

### Importação dos Pacotes


```python
import requests
from lxml import html
import pandas as pd
import sqlite3
```

### Obtendo os Dados


```python
# VARIÁVEIS
OVERWRITE = False

# Caso deseje acumular ou não as listas de reposiórios de diferentes usuários
```


```python
# URL's
URL_LIST = []
URL_LIST.append('https://github.com/uallasleles?tab=repositories')
URL_LIST.append('https://github.com/vitorfs?tab=repositories')
URL_LIST.append('https://github.com/franklin390?tab=repositories')
URL_LIST.append('https://github.com/VictorSRocha?tab=repositories')

# Aqui colocamos a url da aba repositórios da página de cada usuário desejado
```

- Criando a função para extrair os dados


```python
# Esta é a função que realiza a extração dos dados da página web.
def scraping_page(html):
    global user_repositories_list
    
    # Obtem um fragmento do html
    e = html.get_element_by_id("user-repositories-list")
    
    # Extraio os elementos
    for i in e.getchildren()[0]:
        for v in i.getchildren()[0:3]:

            try: repo_user = URL
            except: repo_user = ''

            try: repo_name = v.getchildren()[0].getchildren()[0].text_content().strip().replace('\n', '')
            except: repo_name = ''

            if len(repo_name) != 0:
                try: repo_fork = v.getchildren()[0].getchildren()[1].text_content().strip().replace('\n', '')
                except: repo_fork = ''

                try: repo_desc = v.getchildren()[1].getchildren()[0].text_content().strip().replace('\n', '')
                except: repo_desc = ''

                user_repositories_list = user_repositories_list.append(
                    {
                        'user': repo_user,
                        'name': repo_name,
                        'fork': repo_fork,
                        'desc': repo_desc
                    },
                    ignore_index = True)
                
    return user_repositories_list
```

- Extraindo os Dados


```python
# Dicionário para receber os dados
user_repositories_list = pd.DataFrame(columns=['user', 'name', 'fork', 'desc'])

# Neste loop executamos o scraping para cada url da lista e acumulamos os resultados em um dataframe.
for URL in URL_LIST:
    
    r = requests.get(URL)
    assert r.status_code == 200, f"Falha no request."

    # Retorna document_fromstring ou fragment_fromstring, com base em se a string parece um documento completo ou apenas um fragmento.
    HTML = html.fromstring(r.text)
    
    # Chama a função para o scraping
    user_repositories_list = scraping_page(HTML)
```


```python
# Conferindo os dados
user_repositories_list.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>user</th>
      <th>name</th>
      <th>fork</th>
      <th>desc</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>https://github.com/uallasleles?tab=repositories</td>
      <td>proj_20210623-Scraping_SQLite</td>
      <td></td>
      <td>Fazendo scraping e salvando os dados em um ban...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>https://github.com/uallasleles?tab=repositories</td>
      <td>uallasleles.github.io</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>https://github.com/uallasleles?tab=repositories</td>
      <td>proj_20210619-Early_Stage_Diabetes_Risk_Predic...</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>https://github.com/uallasleles?tab=repositories</td>
      <td>portfolio</td>
      <td></td>
      <td>Relação de projetos, ideias e habilidades dese...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>https://github.com/uallasleles?tab=repositories</td>
      <td>odoo</td>
      <td>Forked from odoo/odoo</td>
      <td>Odoo. Open Source Apps To Grow Your Business.</td>
    </tr>
    <tr>
      <th>5</th>
      <td>https://github.com/uallasleles?tab=repositories</td>
      <td>maintainer-tools</td>
      <td>Forked from OCA/maintainer-tools</td>
      <td>Odoo Maintainers Tools &amp; conventions for OCA m...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>https://github.com/uallasleles?tab=repositories</td>
      <td>unimed_bi</td>
      <td>Forked from dudanogueira/unimed_bi</td>
      <td>Sistema de Business Intelligence e Data Analyt...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>https://github.com/uallasleles?tab=repositories</td>
      <td>orun</td>
      <td>Forked from katrid/orun</td>
      <td>Orun. Build Your Own Custom Python ERP/CRM Sof...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>https://github.com/uallasleles?tab=repositories</td>
      <td>Odoo-14-Development-Cookbook-Fourth-Edition</td>
      <td>Forked from PacktPublishing/Odoo-14-Developmen...</td>
      <td>Odoo 14 Development Cookbook - Fourth Edition,...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>https://github.com/uallasleles?tab=repositories</td>
      <td>Odoo-12-Development-Essentials-Fourth-Edition</td>
      <td>Forked from PacktPublishing/Odoo-12-Developmen...</td>
      <td>Odoo 12 Development Essentials, Fourth Edition...</td>
    </tr>
  </tbody>
</table>
</div>



## Armazenando em um Banco de Dados

Para salvar o resultado temos que criar uma conexão com o banco de dados, criar uma tabela com os campos necessários e realizar o insert para cada registro do dataframe.

- Criando uma conexão


```python
# Criando um banco e uma conexão.
con = sqlite3.connect('proj_20210623.db')
# Criando um cursor para acessar os dados.
cur = con.cursor()
```

- Criando os scripts necessários


```python
# Definindo as instruções sql necessárias
sql_delete_table = '''DELETE FROM user_repositories_list'''
sql_create_table = '''CREATE TABLE IF NOT EXISTS user_repositories_list (user text, name text, fork text, desc text)'''
sql_insert_table = '''INSERT INTO user_repositories_list (user, name, fork, desc) VALUES (?, ?, ?, ?)'''
sql_query = '''SELECT * FROM user_repositories_list'''
```

- Criando uma tabela


```python
# Criando a tabela
try:
    cur.execute(sql_create_table)
finally:
    if OVERWRITE:
        cur.execute(sql_delete_table)
```

- Inserindo os Dados


```python
# Inserindo os dados
for r in user_repositories_list.itertuples():
    cur.execute(sql_insert_table, r[1:])
con.commit()
```

- Recuperando os dados


```python
pd.read_sql_query(sql_query, con)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>user</th>
      <th>name</th>
      <th>fork</th>
      <th>desc</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>https://github.com/uallasleles?tab=repositories</td>
      <td>uallasleles.github.io</td>
      <td></td>
      <td>SEM DESCRIÇÃO</td>
    </tr>
    <tr>
      <th>1</th>
      <td>https://github.com/uallasleles?tab=repositories</td>
      <td>proj_20210619-Early_Stage_Diabetes_Risk_Predic...</td>
      <td></td>
      <td>SEM DESCRIÇÃO</td>
    </tr>
    <tr>
      <th>2</th>
      <td>https://github.com/uallasleles?tab=repositories</td>
      <td>portfolio</td>
      <td></td>
      <td>Relação de projetos, ideias e habilidades dese...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>https://github.com/uallasleles?tab=repositories</td>
      <td>odoo</td>
      <td>Forked from odoo/odoo</td>
      <td>Odoo. Open Source Apps To Grow Your Business.</td>
    </tr>
    <tr>
      <th>4</th>
      <td>https://github.com/uallasleles?tab=repositories</td>
      <td>maintainer-tools</td>
      <td>Forked from OCA/maintainer-tools</td>
      <td>Odoo Maintainers Tools &amp; conventions for OCA m...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>317</th>
      <td>https://github.com/franklin390?tab=repositories</td>
      <td>Project_02_Group_Bimbo_Inventory_Demand</td>
      <td></td>
      <td>Neste projeto de aprendizado de máquina, vamos...</td>
    </tr>
    <tr>
      <th>318</th>
      <td>https://github.com/VictorSRocha?tab=repositories</td>
      <td>VictorSRocha.github.io</td>
      <td></td>
      <td>Website Profile</td>
    </tr>
    <tr>
      <th>319</th>
      <td>https://github.com/VictorSRocha?tab=repositories</td>
      <td>Prevendo-o-Nivel-de-Satisfacao-dos-Clientes</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>320</th>
      <td>https://github.com/VictorSRocha?tab=repositories</td>
      <td>Prevendo-Demanda-de-Estoque-com-Base-em-Vendas</td>
      <td></td>
      <td>Projeto para criação de um modelo para Previsã...</td>
    </tr>
    <tr>
      <th>321</th>
      <td>https://github.com/VictorSRocha?tab=repositories</td>
      <td>Deteccao-de-Fraudes-no-Trafego-de-Cliques-em-P...</td>
      <td></td>
      <td>Projeto para criação de um algoritmo para Dete...</td>
    </tr>
  </tbody>
</table>
<p>322 rows × 4 columns</p>
</div>



- Modificando os dados


```python
# Não vou precisar modificar estes dados, mas caso queira saber, isso seria possível da seguinte forma:
sql_udt_desc = '''UPDATE user_repositories_list SET desc = "SEM DESCRIÇÃO" WHERE desc = ""'''
cur.execute(sql_udt_desc)
con.commit()
```


```python
con.close()
```

## Conclusão
Criei uma sequencia de código para extrair dados de uma página web, então salvei estes dados em um banco de dados SQLite chamado 'proj_20210623.db'. A única parte que exige um pouco mais de trabalho é a construção da lógica do scraping, pois você terá que analisar a estrutura do documento html, identificar tags e definir uma estratégia para extrair o conteúdo desejado.
Após a execução do processo, podemos recuperar estes dados para análise, ou então, dar sequencia utilizando linguagem SQL para gerar consultas, gerarmos relatórios, procedimentos, agregação com outros conjuntos de dados, transferí-los para outras pessoas, etc.

## Fim
