# RESPOSTAS - TESTE DE MONGODB

## IDENTIFICAÇÃO

**Nome completo:** Felipe Mendes Almeida e Karliane Lopes Evangelista
**Matrícula:** 2528652 | 2527024
**Email:** felix.felipe@gmail.com | karliane.lopes.kl.kl@gmail.com 
**Data:** 05/06/2026

---

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

