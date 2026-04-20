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
