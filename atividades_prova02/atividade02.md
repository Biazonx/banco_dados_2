# Exercícios Práticos de NoSQL com MongoDB Atlas

## Tema: StreamFlix - Plataforma de Streaming

## Nível 1 - Primeiros contatos com documentos e coleções

### Exercício 1
Liste todos os documentos da coleção `usuarios`.

```javascript
use("streamflix")

db.usuarios.find()
```

### Exercício 2
Liste todos os documentos da coleção `conteudos`.

```javascript
use("streamflix")

db.conteudos.find()
```

### Exercício 3
Liste todos os usuários da cidade de `Curitiba`.

```javascript
use("streamflix")

db.usuarios.find({ cidade: "Curitiba" })
```

### Exercício 4
Liste todos os conteúdos do tipo `filme`.

```javascript
use("streamflix")

db.conteudos.find({ tipo: "filme" })
```

### Exercício 5
Busque o conteúdo cujo título é `Matrix`.

```javascript
use("streamflix")

db.conteudos.find({ titulo: "Matrix" })
```

### Exercício 6
Insira um novo usuário na coleção `usuarios`.

```javascript
use("streamflix")

db.usuarios.insertOne({
  nome: "Thiago Ramos",
  email: "thiago@email.com",
  idade: 26,
  cidade: "Maringá",
  estado: "PR",
  interesses: ["Ficção Científica", "Ação"],
  ativo: true
})
```

### Exercício 7
Insira um novo conteúdo do tipo `filme`.

```javascript
use("streamflix")

db.conteudos.insertOne({
  titulo: "Tenet",
  tipo: "filme",
  ano: 2020,
  generos: ["Ação", "Ficção Científica", "Suspense"],
  avaliacaoMedia: 7.4,
  duracaoMinutos: 150,
  disponivel: true
})
```

---

## Nível 2 - Operadores de comparação

### Exercício 8
Liste os conteúdos com avaliação média maior que `9`.

```javascript
use("streamflix")

db.conteudos.find({ avaliacaoMedia: { $gt: 9 } })
```

### Exercício 9
Liste os usuários com idade maior que `30`.

```javascript
use("streamflix")

db.usuarios.find({ idade: { $gt: 30 } })
```

### Exercício 10
Liste os conteúdos lançados antes do ano `2010`.

```javascript
use("streamflix")

db.conteudos.find({ ano: { $lt: 2010 } })
```

### Exercício 11
Liste os conteúdos lançados a partir de `2015`.

```javascript
use("streamflix")

db.conteudos.find({ ano: { $gte: 2015 } })
```

### Exercício 12
Liste os conteúdos cuja avaliação média seja menor ou igual a `8.8`.

```javascript
use("streamflix")

db.conteudos.find({ avaliacaoMedia: { $lte: 8.8 } })
```

### Exercício 13
Liste os usuários que não são do estado `PR`.

```javascript
use("streamflix")

db.usuarios.find({ estado: { $ne: "PR" } })
```

---

## Nível 3 - Consultas com arrays

### Exercício 14
Liste os conteúdos que possuem o gênero `Drama`.

```javascript
use("streamflix")

db.conteudos.find({ generos: "Drama" })
```

### Exercício 15
Liste os conteúdos que possuem o gênero `Ficção Científica`.

```javascript
use("streamflix")

db.conteudos.find({ generos: "Ficção Científica" })
```

### Exercício 16
Liste os conteúdos que possuem os gêneros `Drama` e `Crime` ao mesmo tempo.

```javascript
use("streamflix")

db.conteudos.find({ generos: { $all: ["Drama", "Crime"] } })
```

### Exercício 17
Liste os usuários que possuem interesse em `Suspense`.

```javascript
use("streamflix")

db.usuarios.find({ interesses: "Suspense" })
```

### Exercício 18
Liste os conteúdos que possuem pelo menos um dos gêneros `Terror` ou `Mistério`.

```javascript
use("streamflix")

db.conteudos.find({ generos: { $in: ["Terror", "Mistério"] } })
```

### Exercício 19
Liste os conteúdos que não possuem o gênero `Comédia`.

```javascript
use("streamflix")

db.conteudos.find({ generos: { $nin: ["Comédia"] } })
```

---

## Nível 4 - Objetos aninhados

### Exercício 20
Liste os conteúdos dirigidos por `Christopher Nolan`.

```javascript
use("streamflix")

db.conteudos.find({ "diretor.nome": "Christopher Nolan" })
```

### Exercício 21
Liste os conteúdos cujo diretor é do `Reino Unido`.

```javascript
use("streamflix")

db.conteudos.find({ "diretor.pais": "Reino Unido" })
```

### Exercício 22
Liste os usuários cujo bairro seja `Centro`.

```javascript
use("streamflix")

db.usuarios.find({ "endereco.bairro": "Centro" })
```

### Exercício 23
Liste os usuários que possuem o campo `endereco`.

```javascript
use("streamflix")

db.usuarios.find({ endereco: { $exists: true } })
```

### Exercício 24
Liste os usuários que não possuem o campo `endereco`.

```javascript
use("streamflix")

db.usuarios.find({ endereco: { $exists: false } })
```

---

## Nível 5 - Atualizações básicas

### Exercício 25
Atualize o usuário `Carlos Lima` para que o campo `ativo` passe a ser `true`.

```javascript
use("streamflix")

db.usuarios.updateOne(
  { nome: "Carlos Lima" },
  { $set: { ativo: true } }
)
```

### Exercício 26
Atualize o conteúdo `Cidade de Deus` para que o campo `disponivel` passe a ser `true`.

```javascript
use("streamflix")

db.conteudos.updateOne(
  { titulo: "Cidade de Deus" },
  { $set: { disponivel: true } }
)
```

### Exercício 27
Adicione o campo `idiomaOriginal` ao filme `Matrix`, com o valor `Inglês`.

```javascript
use("streamflix")

db.conteudos.updateOne(
  { titulo: "Matrix" },
  { $set: { idiomaOriginal: "Inglês" } }
)
```

### Exercício 28
Adicione o campo `classificacao` ao filme `Interestelar`, com o valor `10+`.

```javascript
use("streamflix")

db.conteudos.updateOne(
  { titulo: "Interestelar" },
  { $set: { classificacao: "10+" } }
)
```

### Exercício 29
Atualize a avaliação média de `Avatar` para `9.0`.

```javascript
use("streamflix")

db.conteudos.updateOne(
  { titulo: "Avatar" },
  { $set: { avaliacaoMedia: 9.0 } }
)
```

---

## Nível 6 - Atualizações com operadores

### Exercício 30
Incremente em `1` a quantidade de visualizações do conteúdo `Matrix`.

```javascript
use("streamflix")

db.conteudos.updateOne(
  { titulo: "Matrix" },
  { $inc: { visualizacoes: 1 } }
)
```

### Exercício 31
Incremente em `1000` a quantidade de visualizações de todos os conteúdos disponíveis.

```javascript
use("streamflix")

db.conteudos.updateMany(
  { disponivel: true },
  { $inc: { visualizacoes: 1000 } }
)
```

### Exercício 32
Adicione o gênero `Clássico` ao filme `Matrix`.

```javascript
use("streamflix")

db.conteudos.updateOne(
  { titulo: "Matrix" },
  { $push: { generos: "Clássico" } }
)
```

### Exercício 33
Remova o gênero `Clássico` do filme `Matrix`.

```javascript
use("streamflix")

db.conteudos.updateOne(
  { titulo: "Matrix" },
  { $pull: { generos: "Clássico" } }
)
```

### Exercício 34
Remova o campo `telefone` da usuária `Beatriz Nunes`.

```javascript
use("streamflix")

db.usuarios.updateOne(
  { nome: "Beatriz Nunes" },
  { $unset: { telefone: "" } }
)
```

### Exercício 35
Adicione o benefício `sem anúncios` aos usuários do plano `Premium` na coleção `assinaturas`.

```javascript
use("streamflix")

db.assinaturas.updateMany(
  { plano: "Premium" },
  { $push: { beneficios: "sem anúncios" } }
)
```

---

## Nível 7 - Operadores lógicos

### Exercício 36
Liste os conteúdos que são filmes e possuem avaliação média maior que `9`.

```javascript
use("streamflix")

db.conteudos.find({
  $and: [
    { tipo: "filme" },
    { avaliacaoMedia: { $gt: 9 } }
  ]
})
```

### Exercício 37
Liste os usuários que são de `Curitiba` ou de `Maringá`.

```javascript
use("streamflix")

db.usuarios.find({
  $or: [
    { cidade: "Curitiba" },
    { cidade: "Maringá" }
  ]
})
```

### Exercício 38
Liste os conteúdos que são séries ou documentários.

```javascript
use("streamflix")

db.conteudos.find({
  $or: [
    { tipo: "serie" },
    { tipo: "documentario" }
  ]
})
```

### Exercício 39
Liste os conteúdos que possuem avaliação maior que `9` e visualizações acima de `2000000`.

```javascript
use("streamflix")

db.conteudos.find({
  $and: [
    { avaliacaoMedia: { $gt: 9 } },
    { visualizacoes: { $gt: 2000000 } }
  ]
})
```

### Exercício 40
Liste os usuários ativos com idade menor que `30`.

```javascript
use("streamflix")

db.usuarios.find({
  $and: [
    { ativo: true },
    { idade: { $lt: 30 } }
  ]
})
```

---

## Nível 8 - Campos opcionais e flexibilidade NoSQL

### Exercício 41
Liste os conteúdos que possuem o campo `premios`.

```javascript
use("streamflix")

db.conteudos.find({ premios: { $exists: true } })
```

### Exercício 42
Liste os conteúdos que não possuem o campo `diretor`.

```javascript
use("streamflix")

db.conteudos.find({ diretor: { $exists: false } })
```

### Exercício 43
Liste os usuários que possuem o campo `premium`.

```javascript
use("streamflix")

db.usuarios.find({ premium: { $exists: true } })
```

### Exercício 44
Liste os conteúdos que possuem o campo `temporadas`.

```javascript
use("streamflix")

db.conteudos.find({ temporadas: { $exists: true } })
```

### Exercício 45
Explique por que os documentos da coleção `conteudos` podem ter campos diferentes.

No MongoDB, cada documento é independente e não precisa seguir um esquema fixo. Isso permite que filmes tenham campos como `duracaoMinutos` e `diretor`, séries tenham `temporadas` e `episodios`, e documentários tenham `narrador`, sem que os demais documentos precisem conter esses campos. Essa flexibilidade é uma característica central dos bancos de dados NoSQL orientados a documentos.

---

## Nível 9 - Remoção de documentos

### Exercício 46
Remova o usuário que você criou no Exercício 6.

```javascript
use("streamflix")

db.usuarios.deleteOne({ email: "thiago@email.com" })
```

### Exercício 47
Remova o conteúdo que você criou no Exercício 7.

```javascript
use("streamflix")

db.conteudos.deleteOne({ titulo: "Tenet" })
```

### Exercício 48
Remova todas as avaliações com nota menor que `8`.

```javascript
use("streamflix")

db.avaliacoes.deleteMany({ nota: { $lt: 8 } })
```

### Exercício 49
Remova os registros de histórico cujo progresso seja menor que `40`.

```javascript
use("streamflix")

db.historico.deleteMany({ progressoPercentual: { $lt: 40 } })
```

### Exercício 50
Explique a diferença entre manter as informações separadas em várias coleções e armazenar tudo em um único documento.

Manter informações em várias coleções facilita a atualização e reutilização dos dados, pois uma alteração em um registro reflete automaticamente para todos que o referenciam. Armazenar tudo em um único documento torna as leituras mais rápidas e simples, pois todos os dados relacionados já estão agrupados, porém pode gerar duplicação e dificuldade de manutenção quando o mesmo dado precisa ser atualizado em muitos lugares.

### Exercício 51
Explique uma vantagem e uma desvantagem de usar documentos aninhados no MongoDB.

Uma vantagem é a performance de leitura: ao agrupar dados relacionados no mesmo documento, uma única consulta retorna tudo que é necessário sem precisar cruzar coleções. Uma desvantagem é a dificuldade de atualização: se o dado aninhado se repetir em vários documentos, qualquer alteração precisará ser propagada manualmente em cada ocorrência.

### Exercício 52
Explique em quais situações seria melhor usar referência entre coleções.

Referências entre coleções são mais indicadas quando os dados são compartilhados por muitos documentos e precisam ser atualizados com frequência, como o cadastro de um usuário que está associado a assinaturas, avaliações e histórico. Também é preferível quando os dados referenciados crescem de forma independente e não fazem sentido embutidos em cada documento que os utiliza.

### Exercício 53
Explique em quais situações seria melhor usar dados incorporados no mesmo documento.

Dados incorporados são mais indicados quando as informações são exclusivas daquele documento e raramente mudam de forma independente, como o endereço de um usuário ou o objeto `diretor` de um filme. Também é a melhor escolha quando os dados são sempre consultados juntos, pois evita operações adicionais de busca e melhora o desempenho das leituras.