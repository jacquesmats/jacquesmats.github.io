---
layout: post
title: Localizando dados em um Dataframe usando Pandas LOC e ILOC
---

Há muitas maneiras de extrair os elementos, linhas e colunas de um DataFrame. Alguns métodos de indexação são muito semelhantes, mas se comportam de maneira muito diferente. O objetivo deste post é identificar uma estratégia única para extrair dados de um DataFrame que seja simples de interpretar e produza resultados confiáveis. 

No caso de você querer pular para o final, aqui está o resumo:

    1. use .loc [] para rótulos
    2. use .iloc [] para posições
    3. designe explicitamente as linhas e colunas, mesmo que seja com ':'

Para começar, criamos um pequeno dataset usando dados da Wikipedia sobre as montanhas mais altas do mundo. Para cada montanha, temos seu nome, altura em metros, ano em que foi a primeira ascensão e a cordilheira a qual ela pertence. Se esta é sua primeira exposição a um DataFrame do pandas, cada montanha e suas informações associadas são uma linha, e cada informação, por exemplo, nome ou altitude, é uma coluna.



```python
import pandas as pd

#Cria um pequeno dataset
data = {'Nome':['Mount Everest', 'K2', 'Kangchenjunga', 'Lhotse'], 'Altitude':[8848,8611,8586,8616],'Primeira ascensão':[1953,1954,1955,1956],'Cordilheira':['Himalaia', 'Karakoram', 'Himalaia', 'Himalaia']}
df = pd.DataFrame(data)
display(df)
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
      <th>Nome</th>
      <th>Altitude</th>
      <th>Primeira ascensão</th>
      <th>Cordilheira</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mount Everest</td>
      <td>8848</td>
      <td>1953</td>
      <td>Himalaia</td>
    </tr>
    <tr>
      <th>1</th>
      <td>K2</td>
      <td>8611</td>
      <td>1954</td>
      <td>Karakoram</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Kangchenjunga</td>
      <td>8586</td>
      <td>1955</td>
      <td>Himalaia</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Lhotse</td>
      <td>8616</td>
      <td>1956</td>
      <td>Himalaia</td>
    </tr>
  </tbody>
</table>
</div>


Cada coluna tem um nome associado a ela, também conhecido como um rótulo. Os rótulos das nossas colunas são 'Nome', 'Altitude (m)', 'Primeira ascensão' e 'Cordilheira'. Em um Dataframe do Pandas, cada linha também tem um nome. Por padrão, esse rótulo é apenas o número da linha. No entanto, você pode definir uma das suas colunas como o índice do seu DataFrame, o que significa que seus valores serão usados como rótulos de linha. Nós definimos a coluna 'Nome' como nosso índice. <br>
Vale notar a utilização do parametro `inplace = True`, que não vai criar uma cópia do Dataframe, mas vai fazer a modificação diretamente no original.


```python
df.set_index('Nome', inplace=True)
```

## pandas.DataFrame.loc

É uma operação comum escolher uma das colunas do DataFrame para trabalhar. Para selecionar uma coluna por seu rótulo, usamos a função `.loc []`. Uma coisa que podemos fazer que torna nossos comandos fáceis de interpretar é sempre incluir tanto o índice da linha quanto o índice da coluna em que estamos interessados. Neste caso, estamos interessados em todas as linhas. Para mostrar isso, usamos dois pontos (:). Então, para indicar a coluna em que estamos interessados, adicionamos seu rótulo. O comando `df.loc [:, 'Primeira ascensão']` nos leva apenas a coluna 'Primeira ascensão'.



```python
df.loc[:, 'Primeira ascensão']
```




    Nome
    Mount Everest    1953
    K2               1954
    Kangchenjunga    1955
    Lhotse           1956
    Name: Primeira ascensão, dtype: int64



Vale a pena notar que este comando retorna uma série, a estrutura de dados que os pandas usam para representar uma coluna. Se, em vez de uma série, apenas quiséssemos uma matriz dos números que estão na coluna 'Primeira ascensão', então adicionamos '.values' ao final do nosso comando. Isso retorna um array numpy contendo [1953, 1954, 1955 e 1956].



```python
df.loc[:, 'Primeira ascensão'].values
```




    array([1953, 1954, 1955, 1956])



Se quisermos apenas obter uma única linha, usamos novamente a função `.loc[]`, dessa vez especificando um rótulo de linha e colocando dois pontos na posição da coluna.


```python
df.loc['K2',:]
```




    Altitude                  8611
    Primeira ascensão         1954
    Cordilheira          Karakoram
    Name: K2, dtype: object



Se quisermos apenas um único valor, por exemplo, o ano em que o K2 foi finalizado, podemos especificar os rótulos para a linha e a coluna. A linha sempre vem em primeiro lugar.


```python
df.loc['K2','Primeira ascensão']
```




    1954



Embora você possa usar apenas um argumento na função `.loc[]`, é mais simples interpretar se você sempre especificar a linha e a coluna, mesmo que seja com dois-pontos. <br>
Não precisamos nos limitar a uma única linha ou coluna usando esse método. Aqui, na posição de linha, passamos uma lista de rótulos. Isso retorna um conjunto de linhas, em vez de apenas um.


```python
df.loc[['K2','Lhotse'],:]
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
      <th>Altitude</th>
      <th>Primeira ascensão</th>
      <th>Cordilheira</th>
    </tr>
    <tr>
      <th>Nome</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>K2</th>
      <td>8611</td>
      <td>1954</td>
      <td>Karakoram</td>
    </tr>
    <tr>
      <th>Lhotse</th>
      <td>8616</td>
      <td>1956</td>
      <td>Himalaia</td>
    </tr>
  </tbody>
</table>
</div>



Também podemos obter um subconjunto das colunas, especificando as colunas inicial e final e colocando um ':' entre elas. <br> Neste caso, `'Altitude':'Primeira ascensão'` nos dará todas as colunas entre elas. Observe que isso é diferente da indexação numérica em numpy, em que a última coluna é omitida. Além disso, como já especificamos a coluna de nome como o índice, ela também será retornada no dataframe que receberemos de volta.


```python
df.loc[:,'Altitude':'Primeira ascensão']
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
      <th>Altitude</th>
      <th>Primeira ascensão</th>
    </tr>
    <tr>
      <th>Nome</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Mount Everest</th>
      <td>8848</td>
      <td>1953</td>
    </tr>
    <tr>
      <th>K2</th>
      <td>8611</td>
      <td>1954</td>
    </tr>
    <tr>
      <th>Kangchenjunga</th>
      <td>8586</td>
      <td>1955</td>
    </tr>
    <tr>
      <th>Lhotse</th>
      <td>8616</td>
      <td>1956</td>
    </tr>
  </tbody>
</table>
</div>



Além disso, podemos selecionar linhas ou colunas onde o valor atende a uma determinada condição. Neste caso, queremos encontrar as linhas onde os valores da coluna 'Primeira ascensão' são maiores que 1954. Na posição das linhas, podemos colocar qualquer expressão booleana que tenha o mesmo número de valores que nós temos linhas. Nós poderíamos fazer o mesmo para colunas, se quiséssemos.


```python
df.loc[df.loc[:,'Primeira ascensão'] > 1954,:]
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
      <th>Altitude</th>
      <th>Primeira ascensão</th>
      <th>Cordilheira</th>
    </tr>
    <tr>
      <th>Nome</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Kangchenjunga</th>
      <td>8586</td>
      <td>1955</td>
      <td>Himalaia</td>
    </tr>
    <tr>
      <th>Lhotse</th>
      <td>8616</td>
      <td>1956</td>
      <td>Himalaia</td>
    </tr>
  </tbody>
</table>
</div>



## pandas.DataFrame.iloc

Como alternativa à seleção de linhas e colunas por seus rótulos, também podemos selecioná-los por seus números de linha e coluna. A ordenação das colunas e, portanto, suas posições, depende de como o dataframe é inicializado. A coluna do índice, nossa coluna "nome", não é contada. <br>
Para selecionar dados por sua posição, usamos a função `.iloc[]`. Novamente, o primeiro argumento é para as linhas, e o segundo argumento é para as colunas. Para selecionar todas as colunas na linha zero, escrevemos `.iloc[0,;]`


```python
df.iloc[0,:] # Mount Everest
```




    Altitude                 8848
    Primeira ascensão        1953
    Cordilheira          Himalaia
    Name: Mount Everest, dtype: object



Da mesma forma, podemos selecionar uma coluna por posição, colocando o número da coluna que queremos na posição da coluna da função .iloc [].


```python
df.iloc[:,2] # Cordilheira
```




    Nome
    Mount Everest     Himalaia
    K2               Karakoram
    Kangchenjunga     Himalaia
    Lhotse            Himalaia
    Name: Cordilheira, dtype: object



Podemos extrair um único valor, especificando a posição da linha e da coluna.


```python
df.iloc[0,2] # Mount Everest,Cordilheira
```




    'Himalaia'



Podemos passar uma lista de posições se quisermos selecionar determinadas linhas e / ou determinadas colunas.


```python
df.iloc[[1,3],:] 
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
      <th>Altitude</th>
      <th>Primeira ascensão</th>
      <th>Cordilheira</th>
    </tr>
    <tr>
      <th>Nome</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>K2</th>
      <td>8611</td>
      <td>1954</td>
      <td>Karakoram</td>
    </tr>
    <tr>
      <th>Lhotse</th>
      <td>8616</td>
      <td>1956</td>
      <td>Himalaia</td>
    </tr>
  </tbody>
</table>
</div>



Também podemos usar o operador de intervalo de dois pontos para obter um conjunto de linhas ou colunas por posição. Observe que, diferentemente da função `.loc[]` usando rótulos, a função `.iloc[]` usando posições não inclui o nó de extremidade, ou seja, a última coluna/linha. Nesse caso, ele retorna apenas as colunas zero e um e não retorna a coluna dois.


```python
df.iloc[:,0:2] 
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
      <th>Altitude</th>
      <th>Primeira ascensão</th>
    </tr>
    <tr>
      <th>Nome</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Mount Everest</th>
      <td>8848</td>
      <td>1953</td>
    </tr>
    <tr>
      <th>K2</th>
      <td>8611</td>
      <td>1954</td>
    </tr>
    <tr>
      <th>Kangchenjunga</th>
      <td>8586</td>
      <td>1955</td>
    </tr>
    <tr>
      <th>Lhotse</th>
      <td>8616</td>
      <td>1956</td>
    </tr>
  </tbody>
</table>
</div>



Tudo isso pode ser resumido da seguinte maneira:

    1. use .loc [] para rótulos
    2. use .iloc [] para posições
    3. designe explicitamente as linhas e colunas, mesmo que seja com ':'
    
Eu sempre tive dificuldade para entender qual comando usar, e tive que parar e estudar com exemplos para realmente saber qual método utilizar. Esse tutorial é uma tentiva de fornecer um atalho para quem se encontra na mesma posição que eu estava e espero que ele tenha te ajudado de alguma forma.
