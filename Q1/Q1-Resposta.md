# RESPOSTAS - TESTE DE MONGODB

## IDENTIFICAÇÃO

**Nome completo:** Felipe Mendes Almeida e Karliane Lopes Evangelista
**Matrícula:** 2528652 | 2527024
**Email:** felix.felipe@gmail.com | karliane.lopes.kl.kl@gmail.com 
**Data:** 05/06/2026

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