# RESPOSTAS - TESTE DE MONGODB

## IDENTIFICAÇÃO

**Nome completo:** Felipe Mendes Almeida e 
**Matrícula:** ____________________ 
**Email:** felix.felipe@gmail.com |     
**Data:** ___/___/______

---

## QUESTÃO 1 - Consulta Básica com Filtros

### Comando Utilizado
```javascript
db.movies.find(
  {
    genres: "Drama",
    year: { $gte: 2010, $lte: 2015 },
    "imdb.rating": { $gt: 7.5 }
  },
  {
    _id: 0,
    title: 1,
    year: 1,
    "imdb.rating": 1,
    genres: 1
  }
).sort({ "imdb.rating": -1 }).limit(20)
```

### Resultado Obtido
- **Quantidade de documentos encontrados:** ______
- **5 primeiros filmes (título e rating):**
 1. Drishyam | 8.9
 2. Killswitch | 8.8
 3. Kaakkaa Muttai | 8.8
 4. The Great Alone | 8.7
 5. Interstellar | 8.7

### Screenshot
- Questao-01.png
- Questao-01 - Print01.png
- Questao-01 - Print02.png
- Questao-01 - Print03.png
- Questao-01 - Print04.png
- Questao-01 - Print05.png

### Observações (opcional)

## QUESTÃO 2 - Agregação com Agrupamento

### Pipeline Utilizada
```javascript
db.movies.aggregate([
  { $match: { countries: { $exists: true, $ne: [] } } },
  { $unwind: "$countries" },
  { $group: {
      _id: "$countries",
      total_filmes: { $sum: 1 }
  }},
  { $sort: { total_filmes: -1 } },
  { $limit: 10 },
  { $project: {
      _id: 0,
      pais: "$_id",
      total_filmes: 1
  }}
])
```

### Resultado Obtido

| # | País          | Total de Filmes |
|---|---------------|----------------|
| 1 | USA           | 10.921 |
| 2 | UK            | 2.652 |
| 3 | France        | 2.647 |
| 4 | Germany       | 1.494 |
| 5 | Canada        | 1.260 |
| 6 | Italy         | 1.217 |
| 7 | Japan         | 786 |
| 8 | Spain         | 675 |
| 9 | India         | 564 |
| 10 | Australia    | 470 |

### Screenshot


### Observações (opcional)

---

# QUESTÃO 3 - Pipeline com $unwind e $group

### Pipeline Utilizada
```javascript
db.movies.aggregate([
  { $match: { "imdb.rating": { $exists: true, $type: "double" } } },
  { $unwind: "$cast" },
  { $group: {
      _id: "$cast",
      quantidade_filmes: { $sum: 1 },
      media_rating: { $avg: "$imdb.rating" }
  }},
  { $sort: { quantidade_filmes: -1 } },
  { $limit: 5 },
  { $project: {
      _id: 0,
      ator: "$_id",
      quantidade_filmes: 1,
      media_rating: { $round: ["$media_rating", 2] }
  }}
])
```

### Resultado Obtido


| # | Ator                 | Qtd. de Filmes | Média IMDB |
|---|----------------------|----------------|------------|
| 1 | Gérard Depardieu     | 67             |   6.69     |
| 2 | Robert De Niro       | 58             |    6.96    |
| 3 | Michael Caine        | 51             |    6.71    |
| 4 | Bruce Willis         | 49             |   6.41     |
| 5 | Samuel L. Jackson    | 48             |   6.40     |

### Screenshot
- Questao 03.png
- Questao 03 - resposta 02.png
- Questao 03 - resposta.png
- Questao 03 - stage01.png
- Questao 03 - stage02.png
- Questao 03 - stage03.png
- Questao 03 - stage04.png
- Questao 03 - stage05.png
- Questao 03 - stage06.png

### Observações (opcional)


## QUESTÃO 4 - Agregação com $lookup

## Pipeline Utilizada

```javascript
db.comments.aggregate([
  { $group: {
      _id: "$movie_id",
      quantidade_comentarios: { $sum: 1 },
      comentaristas: { $push: { nome: "$name", email: "$email" } }
  }},
  { $sort: { quantidade_comentarios: -1 } },
  { $limit: 5 },
  { $lookup: {
      from: "movies",
      localField: "_id",
      foreignField: "_id",
      as: "filme"
  }},
  { $unwind: "$filme" },
  { $project: {
      _id: 0,
      titulo: "$filme.title",
      ano: "$filme.year",
      quantidade_comentarios: 1,
      primeiros_comentaristas: { $slice: ["$comentaristas", 3] }
  }}
])
```

## Resultado

### Top 5 Filmes com Mais Comentários

| # | Título                     | Ano  | Comentários |
|---|----------------------------|------|-------------|
| 1 | The Taking of Pelham 1 2 3 | 2009 | 161         |
| 2 | Ocean's Eleven             | 2001 | 158         |
| 3 | Terminator Salvation       | 2009 | 158         |
| 4 | 50 First Dates             | 2004 | 158         |
| 5 | About a Boy                | 2002 | 158         |


**1. The Taking of Pelham 1 2 3**
| Nome           | Email                      |
|----------------|--------------------------- |
| Alliser Thorne | owen_teale@gameofthron.es  |
| Amy Ramirez    | amy_ramirez@fakegmail.com  |
| Amy Ramirez    | amy_ramirez@fakegmail.com  |

**2. Ocean's Eleven**
| Nome          | Email                       |
|---------------|-----------------------------|
| Amy Phillips  | amy_phillips@fakegmail.com  |
| Anthony Hurst | anthony_hurst@fakegmail.com |
| Anthony Hurst | anthony_hurst@fakegmail.com |

**3. Terminator Salvation**
| Nome          | Email                       |
|---------------|-----------------------------|
| Amy Phillips  | amy_phillips@fakegmail.com  |
| Amy Phillips  | amy_phillips@fakegmail.com  |
| Amy Phillips  | amy_phillips@fakegmail.com  |

**4. 50 First Dates**
| Nome           | Email                      |
|----------------|----------------------------|
| Alliser Thorne | owen_teale@gameofthron.es  |
| Alliser Thorne | owen_teale@gameofthron.es  |
| Amy Ramirez    | amy_ramirez@fakegmail.com  |

**5. About a Boy**
| Nome          | Email                       |
|---------------|-----------------------------|
| Amy Phillips  | amy_phillips@fakegmail.com  |
| Anthony Cline | anthony_cline@fakegmail.com |
| Anthony Smith | anthony_smith@fakegmail.com |


### Screenshot
- Questao 04 -Filme 01.png
- Questao 04 -Filme 02.png
- Questao 04 -Filme 03.png
- Questao 04 -Filme 04.png
- Questao 04 -Filme 05.png
- Questao 04 - Resposta 02.png
- Questao 04 - Resposta.png


### Observações (opcional)


## QUESTÃO 5 - Agregação com Múltiplos Estágios

### Pipeline Utilizada
```javascript
db.movies.aggregate([
  { $match: {
      "imdb.rating": { $exists: true, $type: "double" },
      genres: { $exists: true, $ne: [] }
  }},
  { $unwind: "$genres" },
  { $group: {
      _id: "$genres",
      quantidade_filmes: { $sum: 1 },
      media_rating: { $avg: "$imdb.rating" }
  }},
  { $match: {
      quantidade_filmes: { $gte: 10, $lte: 50 },
      media_rating: { $gt: 7.0 }
  }},
  { $sort: { media_rating: -1 } },
  { $project: {
      _id: 0,
      genero: "$_id",
      quantidade_filmes: 1,
      media_rating: { $round: ["$media_rating", 2] }
  }}
])
```

### Resultado Obtido

| # | Gênero | Qtd. de Filmes | Média IMDB |
|---|--------|----------------|------------|
| 1 | News   | 44             | 7.25       |

**Nota:** apenas 1 gênero atendeu a todos os critérios simultaneamente (10 ≤ filmes ≤ 50 e média > 7.0).

### Explicação
**Por que esses gêneros são considerados subestimados?**
O gênero **News** é considerado subestimado neste contexto pelos seguintes motivos:

1. **Pouca representatividade:** com apenas 44 filmes na base, é um gênero com baixíssimo volume de produção em comparação com Drama (~14.000), Comedy (~8.000) ou Action (~5.000).

2. **Alta qualidade média:** apesar do pequeno volume, os filmes classificados como "News" possuem média de **7.25 no IMDB** — acima da maioria dos gêneros populares, cujas médias geralmente ficam entre 6.0 e 6.8.

3. **Baixa visibilidade:** por ser um nicho pequeno, esses filmes (geralmente documentários jornalísticos ou dramas baseados em fatos reais de mídia) raramente aparecem nos rankings de popularidade ou bilheteria.

4. **Resultado restrito dos critérios:** a combinação dos filtros (10–50 filmes + média > 7.0) é bastante seletiva. Gêneros populares como Drama, Comedy e Documentary têm centenas ou milhares de filmes, ultrapassando o limite máximo de 50. Gêneros muito raros têm menos de 10 filmes e são excluídos. Apenas "News" sobreviveu a todos os filtros com alta qualidade.


### Screenshot
