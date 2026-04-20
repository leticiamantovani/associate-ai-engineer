# what to consider when creating a vector db

1. first think about vector dimensions

- each model has a dimension, so if you use a model to embedding chunks, and change, it is need to recreate the column and the embeddings

- if you create with wider dimensions and use a model that runs in smaller dimensions, the numbers of the embedding would be filled with 0 numbers, so it would create an error on the semantic search

2. columns of the table

- it is recommend to have a metadata column to make a research after with user id or tenant id

- example: 
    cmetadata JSON  -- você salva aqui: nome do arquivo, página, data, autor...

- you can also create index for the column of metadata

3. avoid duplicates

- when running the bot, avoid add document on each request, you can add a hash id for each embedding so avoid this problema

4. think about index

- there are two kinds

    1. ivfflat — faster for search, not so precise
    2. hnsw — more precise but it costs more memory

- it is recommend to create after that it has data, create when we have nothing is inefficient

5. chunkings strategy

- deoending of the chunk strategy, it affects the quality of the retrieval

## example of pgvector

```sql
CREATE EXTENSION IF NOT EXISTS vector;

CREATE TABLE IF NOT EXISTS langchain_pg_collection (
    uuid UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR NOT NULL,
    cmetadata JSON
);

CREATE TABLE IF NOT EXISTS langchain_pg_embedding (
    id VARCHAR PRIMARY KEY,
    collection_id UUID REFERENCES langchain_pg_collection(uuid) ON DELETE CASCADE,
    embedding vector(768),
    document TEXT,
    cmetadata JSON
);

CREATE INDEX IF NOT EXISTS idx_embedding
    ON langchain_pg_embedding USING ivfflat (embedding vector_cosine_ops);

```

- we will store EVERY PDF CHUNKS in langchain_pg_embedding table
- BUT we will know that it is a diff PDF by the collection_id
- DOCUMENT is the text of the chunk
- cmetadata is the metadata of the chunk