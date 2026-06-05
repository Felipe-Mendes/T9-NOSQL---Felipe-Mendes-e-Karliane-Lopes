# RESPOSTAS - TESTE DE MONGODB

## IDENTIFICAÇÃO

**Nome completo:** Felipe Mendes Almeida e 
**Matrícula:** ____________________ 
**Email:** felix.felipe@gmail.com |     
**Data:** 05/06/2026

---

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
- Questao 02.png
- Questao 02 - Resposta.png
- Questao 02 - Stage01.png
- Questao 02 - Stage02.png
- Questao 02 - Stage03.png
- Questao 02 - Stage04.png
- Questao 02 - Stage05.png
- Questao 02 - Stage06.png

### Observações (opcional)

---
