# What is tokenization?

- tokens is the process to convert string into integer tokens ids, each id maps a block of text

- when it enters the input, it is first converted into tokens

- context windows is measured in tokens, not characters

- example: "Hello, how are you?" -> ["Hello,", "how", "are", "you?"]

## Technical details about tokens

- tokens uses an algorith called Byte Pair Encoding (BPE)

- each model has a encoding code that is used to generate the tokens, but it uses BPE under the hood

- tokenization is the same for identical words, what difference them is the attention process that will be made after this, it will distinguish the by the context of the text, exemple: hello ->[100] hello -> [100] - same token id

- - on the pipeline, the string is converted to bytes, then BPE groups those bytes using learned merge rules, and finally looks up each group in the dictionary to get the token ID

"olá"  →  b'ol\xc3\xa1'  →  [2275, 6043]
string       bytes (UTF-8)     tokens (IDs)

- strings are converted into bytes (UTF-8 - BPE), and then converted into tokens (IDs) (simplified version)

- different encodings for models:

o200k_base  → gpt-4o, gpt-4o-mini
cl100k_base → gpt-4-turbo, gpt-4, gpt-3.5-turbo,
              text-embedding-3-small, text-embedding-3-large
p50k_base   → Codex, text-davinci-002, text-davinci-003

## Pipelines for tokenization

- tokens are not only generated when the input comes, but also when the output is generated

Input:  string → bytes (UTF-8) → token IDs → model
Output: model → token IDs → bytes (UTF-8) → string

## Important things about tokens

- tokens differ between the models, so in one model it might be "Hello, how are you?" -> ["Hello,", "how", "are", "you?"] and in another model it might be "Hello, how are you?" -> ["Hello,", "how", "are", "you?"]

- spaces are considered in tokenizer, example: " red""

- uppercase letters are considered as different tokens, example: "Hello"

- punctuation is considered in tokenizer, exemple "hello?" -> ["hello", "?"]

- Non English characters are considered as different tokens, 
        example: ("hello")    — 1 token
        example: ("olá")  — 2 tokens pra 3 letras
        example: ("você")   — pode ser 3+ tokens

- special characters are considered as different tokens, example: ∝ √ ∅ °

- emojis are considered as different tokens one emoji can consume 2/3 tokens each, example: "👋" -> ["👋"]