# what is rag?

- retrieval augment generation is a technica that enhance the model with a knowlage base document

- this document is commonly storaged in vectordb and we trieve them to pass to the model

- a model can have access more than one document

## why does it exists

- it allow model to answer questions the the model was not trained with

- without the cost of fining tuning

## pipeline

pergunta (texto)
      ↓
embedding model converte pra vetor semântico
      ↓
retriever busca chunks similares no vector store (por similaridade de cosseno)
      ↓
retorna chunks em texto
      ↓
[pergunta + chunks] são montados num prompt (texto)
      ↓
tokenizer converte o prompt completo pra token IDs
      ↓
LLM processa e gera resposta

- we can have many documents cause we turned them in chunks before

## before use document, we should treat them


documento (PDF, Word, etc)
      ↓
extrai o texto (Document Loader)
      ↓
divide em chunks
      ↓
converte chunks em embeddings (embedding model)
      ↓
salva no vector store (FAISS, Pinecone, etc)

- then we can use it on our chats


## what kind of documents can we use?

- pdf
- text
- csv
- json
- etc.
- video/audio (after transcription)

- basically we have specifics document loaders for each kind of document

## how can we test rag?

1. Offline primeiro
   → monta dataset com perguntas e respostas certas
   → testa antes de lançar

- create the dataset with questions and correct answers
-  run RAG with these same questions
- get an LLM (LLM as judge) ans see if they're correct with these metrics:
    - faifhness - did it invented something?
    - answer_relevancy - did it answer the question correctly?
    - context_precision - did it get the correct doc?
    - context-recall - did it lose some doc?
- you can do this with a library using RAGA and langsmith


2. Online depois
   → quando estiver em produção
   → usa logs reais de usuários
   → LLM avalia sem precisar de gabarito

3. Pairwise quando quiser melhorar
   → compara versão A vs versão B
   → ex: prompt diferente, chunk size diferente