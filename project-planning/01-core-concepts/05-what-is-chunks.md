# what is chunks

- chunks are a piece of text that was splitted

- this piece of text has tokens

- IT IS A PIECE OF STRINGS BEFORE IT BECAMES INTO NUMBERS (EMBEDDINGS)

# text splitting

- we use a text splitter to split the text and create the chunks

- we can do this with pure python or with a framework like langchain

- sometimes we use overlap to split the text, in other words, we take a piece of what was splitted and use it to continue the next piece of text

## ways to split text

- REMEMBER - this is only strategies, we can do this usinig python or many framworkds, but it's common to use a framework like langchain

- pure python:

```python
texto = "Um texto longo aqui..."

# Split simples por parágrafo
chunks = texto.split("\n\n")

# Ou com tamanho fixo
tamanho = 500
chunks = [texto[i:i+tamanho] for i in range(0, len(texto), tamanho)]
```

- langchain:

```python
from langchain.text_splitter import CharacterTextSplitter

text_splitter = CharacterTextSplitter(chunk_size=500, chunk_overlap=0)
chunks = text_splitter.split_text(texto)
```

- other frameworks:

```python
# chonkie — biblioteca focada só em chunking
pip install chonkie

# unstructured — muito usada para PDFs e documentos complexos  
pip install unstructured
```

## Different strategies to split text

### Text Structure Based:

- this is the smarter strategy cause this keeps the phrase/senteces/words together

- this strategy is independent, we can make it using pure python, but if we want to use it with a framework, we can use the Langchain

- first it tries to divide by paragraphs, if it is too big -> it divides by phrases, if it still too big -> it tries to divide by words

- it tries to respect this hierarchy to avoid split in the middle of a phrase/sentence/word

- example:

chunk_size = 100 caracteresxs

Parágrafo inteiro → 300 caracteres → GRANDE DEMAIS → tenta dividir por frases
  Frase inteira   → 150 caracteres → GRANDE DEMAIS → tenta dividir por palavras
    Palavra       →  10 caracteres → CABE! → para aqui

- real example:

```python

from langchain_text_splitters import RecursiveCharacterTextSplitter

# Um parágrafo longo (bem mais que 100 caracteres)
texto = """O pagamento deve ser efetuado até o dia 10 de cada mês corrente. Atrasos geram multa de 2% ao mês. Em caso de inadimplência superior a 30 dias, o contrato será suspenso automaticamente."""

splitter = RecursiveCharacterTextSplitter(
    chunk_size=100,    # cada chunk pode ter no máximo 100 caracteres
    chunk_overlap=20,  # 20 caracteres de sobreposição entre chunks
)

chunks = splitter.split_text(texto)

for i, chunk in enumerate(chunks):
    print(f"--- Chunk {i+1} ({len(chunk)} caracteres) ---")
    print(chunk)
    print()

```

- output:

```
--- Chunk 1 (98 caracteres) ---
O pagamento deve ser efetuado até o dia 10 de cada mês corrente. Atrasos geram multa de 2% ao mês.

--- Chunk 2 (87 caracteres) ---
ao mês. Em caso de inadimplência superior a 30 dias, o contrato será suspenso automaticamente.
```

- this is a good example cause it keeps the phrase/senteces/words together


### length based:

- this strategy consists the length of the text, very simples

- we just set a length and when it hits the length, it splits the text

- the length of the text can be of two types:

    tokens
    characters

- example of characters -- look at the functions:

```python
from langchain_text_splitters import CharacterTextSplitter

texto = """O pagamento deve ser efetuado até o dia 10 de cada mês corrente. Atrasos geram multa de 2% ao mês. Em caso de inadimplência superior a 30 dias, o contrato será suspenso automaticamente."""

splitter = CharacterTextSplitter(
    chunk_size=100,    # máximo de 100 CARACTERES por chunk
    chunk_overlap=20,  # 20 caracteres de sobreposição
    separator="\n\n",  # só divide nesse separador — sem hierarquia
)

chunks = splitter.split_text(texto)

for i, chunk in enumerate(chunks):
    print(f"--- Chunk {i+1} ({len(chunk)} caracteres) ---")
    print(chunk)
    print()
```

- example of tokens -- look at the functions:

```python
from langchain_text_splitters import CharacterTextSplitter

texto = """O pagamento deve ser efetuado até o dia 10 de cada mês corrente. Atrasos geram multa de 2% ao mês. Em caso de inadimplência superior a 30 dias, o contrato será suspenso automaticamente."""

splitter = CharacterTextSplitter.from_tiktoken_encoder(
    encoding_name="cl100k_base",  # encoding do GPT-4 e text-embedding-3
    chunk_size=20,                 # máximo de 20 TOKENS por chunk
    chunk_overlap=5,               # 5 tokens de sobreposição
)

chunks = splitter.split_text(texto)

for i, chunk in enumerate(chunks):
    print(f"--- Chunk {i+1} ---")
    print(chunk)
    print()
```

### document structure based:

- this strategy consists of splitting the text by the type of document

- example:
Markdown: Split based on headers (e.g., #, ##, ###)
HTML: Split using tags
JSON: Split by object or array elements
Code: Split by functions, classes, or logical blocks

## When should I use each one?

- PDFs, contratos, artigos → RecursiveCharacterTextSplitter (text structure based)
- Precisa controle preciso de tokens → CharacterTextSplitter com tiktoken (length based)
- Markdown, HTML, código → document structure based

## how the length of the chunk should i choose?

- it depends of the length of your document

- overlap should be less or more than 10% of the chunk size

- small chunks -> many pieces
- big chunks -> fewer pieces