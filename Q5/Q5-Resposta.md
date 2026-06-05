# RESPOSTAS - TESTE DE MONGODB

## IDENTIFICAÇÃO

**Nome completo:** Felipe Mendes Almeida e Karliane Lopes Evangelista
**Matrícula:** 2528652 | 2527024
**Email:** felix.felipe@gmail.com | karliane.lopes.kl.kl@gmail.com 
**Data:** 05/06/2026

---

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
O gênero é considerado subestimado neste contexto pelos seguintes motivos:

1. **Pouca representatividade:** com apenas 44 filmes na base, é um gênero com baixíssimo volume de produção em comparação com Drama (~14.000), Comedy (~8.000) ou Action (~5.000).

2. **Baixa visibilidade:** por ser um nicho pequeno, esses filmes raramente aparecem nos rankings de popularidade ou bilheteria.

3. **Resultado restrito dos critérios:** a combinação dos filtros (10–50 filmes + média > 7.0) é bastante seletiva. Gêneros populares como Drama, Comedy e Documentary têm centenas ou milhares de filmes, ultrapassando o limite máximo de 50. Gêneros muito raros têm menos de 10 filmes e são excluídos. Apenas "News" sobreviveu a todos os filtros com alta qualidade.


### Screenshot
- Questao 05 - Resposta 02.png
- Questao 05 - Resposta.png
