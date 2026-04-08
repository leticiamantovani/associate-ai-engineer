# what is vector db

- vector db is a database that will stores embeddings of chunks

- there are two kinds of vector db today:

a. natives vector dbs

- they were created to be a vector db, so they are optimized for this purpose, the velocity is high, because they are designed to be fast and efficient

    - chroma
    - pinecone
    - qdrant
    - milvus

b. extentions of others db

- they are extensions of other databases, so they are not optimized for this purpose, the velocity is lower, they continue to be a SQL in the case of pgvector, the architecture is the same, but the data is stored in a vector format

    - pgvector (in other words, pgvector is a extention of PostgreSQL that allows you to have a column of the kind vector)
    - redis vector

## when to use a vector db?

- when we have millions of chunks, it is prefered to use native vector db cause it was prepered for this, some dbs as pinecones has automatic indexing and sclaing so we wont suffer, but it's more expense


## how these data is stored on these dbs

- metada on pinecone is a JSON object that we can store chunks

PostgreSQL                  Pinecone

Database                    Project (conta)
└── Schema                  └── Index  ← equivalente a uma table
    └── Table                   └── Namespace  ← equivalente a uma partição
        └── Row                     └── Vector record  ← equivalente a uma row
            ├── id                      ├── id
            ├── content                 ├── values (o vetor)
            ├── embedding               └── metadata (JSON livre)
            └── page

a. pinecone exemple

| Campo | Tipo | O que é |
|-------|------|---------|
| id | string | Identificador único que você define |
| values | float[] | O embedding em si — ex: 1536 números |
| metadata | objeto JSON | Dados livres que você associa ao vetor |

b. pgvector exemple

```sql

CREATE EXTENSION vector;  -- habilita o pgvector

CREATE TABLE pdf_chunks (
    id          SERIAL PRIMARY KEY,
    content     TEXT NOT NULL,           -- texto original do chunk
    embedding   vector(1536) NOT NULL,   -- vetor de 1536 floats
    filename    TEXT,                    -- metadata
    page        INTEGER                  -- metadata
);
```

## how we search things on these dbs

a. pinecone exemple

- cosinem metric is set when you save this embedding on database, so it will be used when searching for similar embeddings

- you cannot set different metrics for different index (indexes are like tables in a relational database)

```python
# mesma ideia — você passa o vetor, ele retorna os mais próximos
results = index.query(
    vector=question_embedding,
    top_k=3,
    include_metadata=True
)

```

b. pgvector exemple

- when we set limit -> TOP K results

- <=> is the cosine similarity operator

```sql
# você passa o vetor da pergunta, ele retorna os mais próximos
results = conn.execute("""
    SELECT content, embedding <=> %s AS distance
    FROM pdf_chunks
    ORDER BY distance
    LIMIT 3  
""", (question_embedding,)).fetchall()
```
            
## pipeline using a vector db

PDF -> extraction of text -> chuncking split -> embedding -> storage on vector db

## difference between cosine similarity and euclidean distance

- cosine is the distance between the angle of two vectors

- euclidean is the distance between the points of two vectors

### why cosine is better for text similarity?

Imagina dois chunks:

Chunk A: "O pagamento deve ser feito em 30 dias."


Chunk B: "O pagamento deve ser feito em 30 dias. Após o vencimento, incide multa de 2% ao mês. O boleto deve ser emitido com 5 dias de antecedência."

the chunk b will be higher than the chunk a, because it has more words and more information, so the cosine similarity will be lower, euclidian calculates the points of the two vector so chunk b will be more distante cause of the length of the vector

## indexes on vector db

- indexes is a list of terms with the number of pages that will contain the term

- it is a structured data of b tree, bynary tree that it is keeped separeted from the database so it avoid to search for each row in database

- the two main types of indexes are:

- HASH -> equality search
- BTREE -> sorting, equality, range search

- example of a index in a B-TREE:

                    "m@gmail.com"
                   /             \
         "c@gmail.com"         "z@gmail.com"
        /            \
  "a@gmail.com"   "leticia@gmail.com"  ← achou em 3 saltos

a. native vector db

- mostly of the vector db has automic indexes, one example is Milvus, you ca create the index after you set the data

b. extention vector db of others db

- in the case of extentions of other dbs, you follow the pattern defined by the db

- for example, in pgvector you have to create a index for vector db column, you have two of them that are specific for vectors:

### HNSW

- this one gets a graph and see the conections that is closed of this graph in layers

- this consumes more memory than IVFFlat 


Camada 0 — todos os nós, conexões curtas (visão micro)
    A — E — C — F — G — D — H — B

Camada 1 — mais nós, conexões médias
    A ———— C ———— D ———— B

Camada 2 — poucos nós, conexões longas (visão macro)
    A ————————————————————— B

### IVFFlat

- this one separes the graph in clusters 

- it uses k means  (an algotithm that separes the data in clusters) and each cluster has a main vector that represent the cluster - this is the average vector of the cluster

- it compares these main graph and find the most similar and returns the k results most similar to the query vector

- it is used for large datasets

### HNSW vs IVFFlat — comparison

| | HNSW | IVFFlat |
|---|---|---|
| Query speed | ~1.5ms | ~2.4ms |
| Memory | High (2-5x more) | Low |
| Needs rebuild? | No — incremental | Yes — when data changes |
| Create on empty table? | Yes ✅ | Never ⚠️ |
| Best for | RAG, incremental data | Large static datasets |






