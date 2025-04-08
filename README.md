# **OBJETIVO**

**Fazer um estudo com dados do site IMDB (https://datasets.imdbws.com/), dos últimos 15 anos, sobre as avaliações dos títulos produzidos com avaliações igual ou acima de 5 e votos acima de 10000.**</br>
1)	Quais são os 15 filmes, ordenados por ano (mais recente) e avaliação (maior para o menor), com avaliações igual ou maior que 8?</br>
2)	Quais são os 15 filmes, ordenados por ano (mais recente) e avaliação (menor para o maior), com avaliações igual ou menor que 6?</br>
3)	Quais são os 10 filmes que menos receberam votos? Faça um ranking.</br>
4)	Quais são os 10 filmes que mais receberam votos? Faça um ranking.</br>
5)	Quais são os 10 filmes menos avaliados?</br>
6)	Quais são os 10 filmes mais bem avaliados?</br>
7)	Quantos filmes foram produzidos por ano?</br>
8)	Quais são os 5 anos que produziram mais filmes?</br>
9)	Quantos votos foram computados por ano?</br>
10)	Qual ano teve mais votos?</br>
11)	Qual ano teve menos votos?</br>
12)	Qual a média dos votos nos últimos 5 anos?</br>

# **COLETA**

https://datasets.imdbws.com/title.basics.tsv.gz</br>
https://datasets.imdbws.com/title.ratings.tsv.gz</br>
Técnica para importação: pandas, io e request</br> 

# **MODELAGEM**

**Esquema utilizado: Estrela**

### **Catálogo dos dados:**
<table>
  <thead>
    <h3>Tabela Title_basics</h3>
    <tr>
      <th scope="col">COLUNA</th>
      <th scope="col">TIPO DE DADO</th>
      <th scope="col">DESCRIÇÃO</th>
      <th scope="col">VALOR ESPERADO (min-max)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">tconst</th>
      <td>string</td>
      <td>chave primária</td>
      <td>identificador único os títulos</td>
    </tr>
     <tr>
      <th scope="row">titleType</th>
      <td>string</td>
      <td>tipo de título</td>
      <td>movie</td>
    </tr>
     <tr>
      <th scope="row">primaryTitle</th>
      <td>string</td>
      <td>nome do título</td>
      <td></td>
    </tr>
    <tr>
      <th scope="row">originalTitle</th>
      <td>string</td>
      <td>nome original do título</td>
      <td></td>
    </tr>
    <tr>
      <th scope="row">isAdult</th>
      <td>string</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th scope="row">startYear</th>
      <td>string</td>
      <td>ano inicial do título</td>
      <td>2010-2025</td>
    </tr>
    <tr>
      <th scope="row">endYear</th>
      <td>string</td>
      <td>ano final do título</td>
      <td></td>
    </tr>
    <tr>
      <th scope="row">runtimeMinutes</th>
      <td>string</td>
      <td>tempo do título</td>
      <td></td>
    </tr>
    <tr>
      <th scope="row">genres</th>
      <td>string</td>
      <td>gênero do título</td>
      <td></td>
    </tr>
</table>

<table>
  <thead>
    <h3>Tabela Title_ratings</h3>
    <tr>
      <th scope="col">COLUNA</th>
      <th scope="col">TIPO DE DADO</th>
      <th scope="col">DESCRIÇÃO</th>
      <th scope="col">VALOR ESPERADO (min-max)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">tconst</th>
      <td>string</td>
      <td>chave primária</td>
      <td>identificador único os títulos</td>
    </tr>
     <tr>
      <th scope="row">averageRating</th>
      <td>double</td>
      <td> nota do título</td>
      <td>5-10</td>
    </tr>
     <tr>
      <th scope="row">numVotes</th>
      <td>long</td>
      <td>número de votos</td>
      <td>10000</td>
    </tr>
</table>

<table>
  <thead>
    <h3>Tabela estudo</h3>
    <tr>
      <th scope="col">COLUNA</th>
      <th scope="col">TIPO DE DADO</th>
      <th scope="col">DESCRIÇÃO</th>
      <th scope="col">VALOR ESPERADO (min-max)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">tconst</th>
      <td>string</td>
      <td>chave primária</td>
      <td>identificador único os títulos</td>
    </tr>
    <tr>
      <th scope="row">titleType</th>
      <td>string</td>
      <td>tipo de título</td>
      <td>movie</td>
    </tr>
    <tr>
      <th scope="row">startYear</th>
      <td>string</td>
      <td>ano inicial do título</td>
      <td>2010-2025</td>
    </tr>
    <tr>
      <th scope="row">averageRating</th>
      <td>double</td>
      <td> nota do título</td>
      <td>5-10</td>
    </tr>
     <tr>
      <th scope="row">numVotes</th>
      <td>long</td>
      <td>número de votos</td>
      <td>10000</td>
    </tr>
</table>

# **ANÁLISE (QUALIDADE DOS DADOS / SOLUÇÃO DO PROBLEMA)**

## **QUALIDADE DOS DADOS**

1) Importaremos as bibliotecas pandas, io e request para acessar os dados;</br>
`import pandas as pd`</br>
`import io`</br>
`import requests`</br></br>
2) Criaremos uma base BRONZE;</br>
`%sql  DROP DATABASE bronze;`</br>
`%sql  CREATE DATABASE bronze;`</br></br>
3) inserção dos dados conforme estão na fonte utilizando pandas;</br>
Tabela title_basics</br>
`url = "https://datasets.imdbws.com/title.basics.tsv.gz"`</br>
`title_basics = pd.read_csv(url, compression='gzip', sep='\t')`</br>
`title_basics.head()`</br>
Tabela title_ratings</br>
`url = "https://datasets.imdbws.com/title.ratings.tsv.gz"`</br>
`title_ratings = pd.read_csv(url, compression='gzip', sep='\t')`</br>
`title_ratings.head()`</br></br>
4) Criaremos as tabelas Title_basics e Title_ratings na base bronze com os dados do DataFrame criado;</br>
Criação tabela Title_basics</br>
`title_basics = spark.createDataFrame(title_basics)`</br>
`title_basics.write.mode("overwrite").saveAsTable("bronze.title_basics")`</br>
Criação tabela Title_ratings</br>
`title_ratings = spark.createDataFrame(title_ratings)`</br>
`title_ratings.write.mode("overwrite").saveAsTable("bronze.title_ratings")`</br></br>
5) Selecionameremos as colunas tconst, titleType, startYear da tabela Title_basics, trataremos a coluna titleType mantendo somente o valor 'movie' e trataremos a coluna startYear para remover qualquer valor que seja diferente do ano;</br>
`%sql select bronze.title_basics.titleType, count(*) from bronze.title_basics group by bronze.title_basics.titleType`</br>
`%sql select bronze.title_basics.startyear, count(*) from bronze.title_basics group by bronze.title_basics.startyear order by startYear desc`</br>
`%sql SELECT tb.tconst, tb.titleType, tb.originalTitle, tb.startYear FROM bronze.title_basics as tb WHERE tb.titleType='movie' and tb.startYear not like '%\N%'`</br></br>
6) Criaremos a base SILVER;</br>
`%sql DROP DATABASE silver;`</br>
`%sql CREATE DATABASE silver;`</br></br>
7) Criaremos a tabela Title_basics na base SILVER com os filtros de limpeza;</br>
`%sql create table silver.title_basics as SELECT tb.tconst, tb.titleType, tb.originalTitle, tb.startYear FROM bronze.title_basics as tb WHERE tb.titleType='movie' and tb.startYear not like '%\N%' and tb.startYear>='2010'`</br></br>
8) Selecionaremos as colunas tconst, averageRating, numVotes da tabela Title_ratings, trataremos a coluna averageRating para saber o valor mínimo e máximo dos ratings, agruparemos os ratings e listaremos para confirmar se não há dados diferentes de notas, trataremos a coluna numVotes para saber se há títulos com votos acima de 10000;</br>
`%sql select min(numVotes), max(numVotes) from bronze.title_ratings`</br>
`%sql select * from bronze.title_ratings where numVotes>1000`</br>
`%sql select min(averageRating), max(averageRating) from bronze.title_ratings`</br>
`%sql select averageRating from bronze.title_ratings group by averageRating order by averageRating`</br>
`%sql select * from bronze.title_ratings where averageRating>5`</br>
`%sql select * from bronze.title_ratings where averageRating>=5 and numVotes>10000`</br></br>
9) Criaremos a tabela Title_ratings na base SILVER com os filtros de limpeza;</br>
`%sql create table silver.title_ratings as select * from bronze.title_ratings where averageRating>=5 and numVotes>10000 `</br></br>
10) Faremos um JOIN com a chave primaria das tabelas do banco SILVER;</br>
`%sql SELECT * FROM silver.title_basics AS TB INNER JOIN silver.title_ratings AS TR ON TB.tconst=TR.tconst`</br></br>
11) Selecionaremos as colunas para criação de uma tabela FLAT com todos os dados das 2 tabelas;</br>
`%sql SELECT TB.tconst, TB.titleType, TB.originalTitle, TB.startYear, TR.averageRating, TR.numVotes FROM silver.title_basics AS TB INNER JOIN silver.title_ratings AS TR ON TB.tconst=TR.tconst`</br></br>
12) Criaremos a base GOLD;</br>
`%sql DROP DATABASE gold;`</br>
`%sql CREATE DATABASE gold;`</br></br>
13) Criaremos a tabela estudo na base GOLD a partir do JOIN feito;</br>
`CREATE TABLE gold.estudo AS SELECT TB.tconst, TB.titleType, TB.originalTitle, TB.startYear, TR.averageRating, TR.numVotes FROM silver.title_basics AS TB INNER JOIN silver.title_ratings AS TR ON TB.tconst=TR.tconst`</br></br>
## **SOLUÇÃO DO PROBLEMA**

**1)  Quais são os 15 filmes, ordenados por ano (mais recente) e avaliação (maior para o menor), com avaliações igual ou maior que 8?**</br>
`SELECT * FROM GOLD.estudo WHERE averageRating>=8 ORDER BY startYear desc, averageRating desc`</br>
R: Os filmes são: Oficina do Diabo(2025) - 8.2, Rekhachithram(2025) - 8, Shingeki no Kyojin: The Last Attack(2024) - 9.2, Solo Leveling: ReAwakening(2024) - 8.9, Dune: Part Two(2024) - 8.5, Maste eshgh(2024) - 8.5, Maharaja(2024) - 8.4, Meiyazhagan(2024) - 8.4, Ainda Estou Aqui(2024) - 8.3, No Other Land(2024) - 8.3, Manjummel Boys(2024) - 8.2, Ibelin(2024) - 8.2, Amaran(2024) - 8.2, The Wild Robot(2024) - 8.2 e Kishkindha Kaandam(2024) - 8.</br></br>
**2) Quais são os 15 filmes, ordenados por ano (mais recente) e avaliação (menor para o maior), com avaliações igual ou menor que 6?**</br>
`SELECT * FROM GOLD.estudo WHERE averageRating<=6 ORDER BY startYear desc , averageRating asc`</br>
R: Os filmes são: Flight Risk(2025) - 5.2, Emergency(2025) - 5.2, Game Changer(2025) - 5.5, You're Cordially Invited(2025) - 5.5, Wolf Man(2025) - 5.6, Back in Action(2025) - 5.9, The Electric State(2025) - 5.9, Captain America: Brave New World(2025) - 5.9, Yudhra(2024) - 5, Time Cut(2024) - 5, The Deliverance(2024) - 5.1, Irish Wish(2024) - 5.2, Miller's Girl(2024) - 5.2, Joker: Folie à Deux(2024) - 5.2 e AfrAId(2024) - 5.2.</br></br>
**3)  Quais são os 10 filmes que menos receberam votos? Faça um ranking.**</br>
`select originalTitle, startYear, numVotes, rank() over (order by numVotes asc) as ranking from gold.estudo order by ranking
limit 10`</br>
R: Os 10 filmes que menos receberam votos ordenados por ranking são: What Keeps You Alive, The Mercy, Tang shan da di zhen, Killing Ground, Anjaam Pathiraa, Come and Find Me, Dolphin Tale 2, Fúsi, Sound of Noise e Please Don't Destroy: The Treasure of Foggy Mountain respectivamente.</br></br>
**4)  Quais são os 10 filmes que mais receberam votos? Faça um ranking.**</br>
`select originalTitle, startYear, numVotes, rank() over (order by numVotes desc) as ranking from gold.estudo order by ranking
limit 10`</br>
R: Os 10 filmes que mais receberam votos ordenados por ranking são: Inception, Interstellar, The Dark Knight Rises, Django Unchained, The Wolf of Wall Street, Joker, Shutter Island, The Avengers Avengers: Endgame e Guardians of the Galaxy respectivamente.</br></br>
**5)  Quais são os 10 filmes menos avaliados?**</br>
`select originalTitle, startYear, averageRating, rank() over (order by averageRating asc) as ranking from gold.estudo order by ranking
limit 10`</br>
R: Os 10 filmes menos avaliados são: 47 Meters Down: Uncaged, The Romantics, 2000 Mules, Yudhra, Charlie's Angels, Time Cut, I Don't Know How She Does It, Blair Witch, Fahrenheit 451 e Elephant White respectivamente.</br></br>
**6)  Quais são os 10 filmes mais bem avaliados?**</br>
`select originalTitle, startYear, averageRating, rank() over (order by averageRating desc) as ranking from gold.estudo order by ranking
limit 10`</br>
R: Os 10 filmes mais bem avaliados são: 47 Meters Down: Uncaged, The Romantics, 2000 Mules, Yudhra, Charlie's Angels, Time Cut, I Don't Know How She Does It, Blair Witch, Fahrenheit 451 e Elephant White respectivamente.</br></br>
**7)  Quantos filmes foram produzidos por ano?**</br>
`select startYear, count(*) as qtd_filmes from gold.estudo group by startYear order by startYear desc`</br>
R: Foram produzidos em 2025, 32 filmes; Foram produzidos em 2024, 243 filmes; Foram produzidos em 2023, 322 filmes; Foram produzidos em 2022, 338 filmes; Foram produzidos em 2021, 302 filmes; Foram produzidos em 2020, 259 filmes; Foram produzidos em 2019, 351 filmes; Foram produzidos em 2018, 380 filmes; Foram produzidos em 2017, 350 filmes; Foram produzidos em 2016, 363 filmes; Foram produzidos em 2015, 345 filmes; Foram produzidos em 2014, 371 filmes; Foram produzidos em 2013, 354 filmes; Foram produzidos em 2012, 316 filmes; Foram produzidos em 2011, 325 filmes; Foram produzidos em 2010, 297 filmes;</br></br>
**8)  Quais são os 5 anos que produziram mais filmes?**</br>
`select startYear, count(*) as qtd_filmes from gold.estudo group by startYear order by qtd_filmes desc
limit 5`</br>
R: Os anos que mais produziram filmes foram 2018, 2014, 2016, 2013 e 2019 respectivamente.</br></br>
**9)  Quantos votos foram computados por ano?**</br>
`select startYear, sum(numVotes) from gold.estudo group by startYear order by startYear desc`</br>
R: 2025 teve 961854 votos; 2024 teve 13140638 votos; 2023 teve 19756644 votos; 2022 teve 23453898 votos; 2021 teve 21838323 votos; 2020 teve 13504278 votos; 2019 teve 31417078 votos; 2018 teve 29182593 votos; 2017 teve 31846333 votos; 2016 teve 35561997 votos; 2015 teve 31841547 votos; 2014 teve 42709464 votos; 2013 teve 42025108 votos; 2012 teve 37903271 votos; 2011 teve 37514890 votos; 2010 teve 35637814 votos;</br></br>
**10) Qual ano teve mais votos?**</br>
`select startYear, sum(numVotes) as Total from gold.estudo group by startYear order by Total desc
limit 1`</br>
R: O ano de 2014, foi o ano que teve mais votos.</br></br>
**11) Qual ano teve menos votos?**</br>
`select startYear, sum(numVotes) as Total from gold.estudo group by startYear order by Total asc
limit 1`</br>
R: O ano de 2025, foi o ano que teve menos votos.</br></br>
**12) Qual a média dos votos nos últimos 5 anos?**</br>
`select avg(numVotes) as Media from gold.estudo where startYear between '2020' and '2025'`</br>
R: A média de votos nos últimos 5 anos foi de aproximadamente 61935.</br>

# **DISCUSSÃO**

Pensar em um problema e começar o desenvolvimento para solução foi um tanto quanto desafiador, pois no meu cotidiano costumo trazer somente soluções para os problemas que surgem na empresa em que trabalho. Tive bastente dificuldade para usar a plataforma, um outro aluno Leandro Loriato, me ajudou bastante com a linguagem spark e o exemplo que o professor Victor Almeida demonstrou foi um divisor de aguás para eu começar o projeto, que até então eu não sabia nem como começar.</br>
Comecei fazendo o projeto com dados da empresa em que eu trabalho, mas no momento de inserir os dados da camada gold, começou a dar diversos problemas e por medo de não conseguir a tempo a solução, comecei do zero com base no exemplo do professor Victor Almeida.</br>
Após as inserções de tabelas e tratamento dos dados, não tive dificuldade em manipular os dados em SQL, pois é a linguagem que hoje eu mais domino.</br>
Os problemas citados para o objetivo do trabalho, são o que mais refletem no meu cotidiano quando solicitam algum insight para soluções de problemas. Acostumado a fazer querys com diversas tabelas, trazendo um grau de complexidade bastante desafiadora, tratar os dados da maneira que tratei em camadas bronze, silver e gold para trazer querys mais simples com resposta objetivas e acertivas, foi o que mais me impactou positivamente no trabalho. Agradeço a ajuda de todos, estou muito feliz com o desenvolvimento desse projeto e o meu crescimento profissional.
