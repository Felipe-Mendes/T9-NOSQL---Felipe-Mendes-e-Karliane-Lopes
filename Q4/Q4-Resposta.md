# RESPOSTAS - TESTE DE MONGODB

## IDENTIFICAÇÃO

**Nome completo:** Felipe Mendes Almeida e 
**Matrícula:** ____________________ 
**Email:** felix.felipe@gmail.com |     
**Data:** 05/06/2026

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


---
