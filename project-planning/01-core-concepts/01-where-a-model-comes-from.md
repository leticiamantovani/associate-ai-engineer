# 1 where a model comes from


## When OPEN AI creates a model, how do they do that?

- there are three types of a languagem model is "created":
    - "created" from a empty math structure with an empty weights - this is training a model from scratch.
    - fina tuning: take a pre-trained model and continue training it on a new task.
    - prompting: take a pre-trained model and prompt it to do a new task.


## Creating a model from scratch

- To train a model, first you need to create a model, and I was thinking about where did it comes from. I did a research, and a model borns from a empty math structure with an empty weights. 

- the name of the archtecture that they use to create a model is called "Transformer" with a mechanism called "Attention" (Google invented this mechanism in 2017).

- weights are like decimals numbers, generated randomly.

- the full cycle is applied on every machine learning model, this is called "gradient descent":

    1. Forward pass — the model makes a prediction with the current weights
    2. Loss function — calculates the error between the prediction and the correct answer
    3. Backpropagation — calculates the gradient (direction of the adjustment)
    4. Optimizer — updates the weights a little in the right direction
    5. Repeat billions of times

- coding example to predict  y = x * 2 (where x is 2 and the correct answer is 4):

```python
import random

# Inicialização — modelo "burro" com peso aleatório
peso = random.uniform(-1.0, 1.0)
taxa_aprendizado = 0.01

print(f"Peso inicial: {peso:.4f}\n")

# Simulando 20 épocas de treino (GPT-4 faz isso bilhões de vezes)
for epoca in range(20):
    entrada = 2
    resposta_correta = 4

    # Forward pass — modelo faz uma previsão com o peso atual
    previsao = entrada * peso

    # Calcula o erro dessa previsão
    erro = (previsao - resposta_correta) ** 2

    # Calcula o gradiente (direção do ajuste)
    gradiente = 2 * (previsao - resposta_correta) * entrada

    # Atualiza o peso — aqui o modelo "aprende"
    peso = peso - taxa_aprendizado * gradiente

    print(f"Época {epoca + 1:2d} | Previsão: {previsao:.4f} | Erro: {erro:.4f} | Peso: {peso:.4f}")
```

---

### Output of the code
```
Peso inicial: -0.3821

Época  1 | Previsão: -0.7642 | Erro: 22.1141 | Peso: -0.2285
Época  2 | Previsão: -0.4570 | Erro: 19.6015 | Peso: -0.0893
Época  3 | Previsão: -0.1785 | Erro: 17.3814 | Peso:  0.0370
Época  4 | Previsão:  0.0741 | Erro: 15.4051 | Peso:  0.1518
...
Época 18 | Previsão:  3.7621 | Erro:  0.0570 | Peso:  1.8811
Época 19 | Previsão:  3.7621 | Erro:  0.0505 | Peso:  1.8991
Época 20 | Previsão:  3.9driver | Erro:  0.0012 | Peso:  1.9980

### does it changes on prod?

- the process doenst change, what changes is the data that is used to train the model and sometimes how the error is calculated, also mostly of the companies uses a framework to do that calculation, for exemple, "PyTorch" or "TensorFlow" or "Jax" or "Flax" or "JAX".

- data mostly of the time IS STRING - in other words, text - in other words, natural language - in other words, a sequence of words.

- so the words are converted to numbers, and the numbers are used to train the model.

- this is called "tokenization", and the model uses a tokenizer to convert the text to numbers.

- each token is a breakdown of the text, for example, "Hello, how are you?" is converted to "Hello,", "how", "are", "you", "?" -> it uses Byte-Pair Encoding algorithm to do that.

- after that, this unique id is embedded into a vector of numbers, and this vector is used to train the model.

## Embedding

- embedding transforms a token into a vector of numbers, and this vector is used to train the model, example:
    - "Hello" -> 01 -> [0.1, 0.2, 0.3]
    - "World" -> 02 -> [0.4, 0.5, 0.6]
    - ","     -> 03 -> [0.7, 0.1, 0.2]

- **what happens if we have the same word in the prompt?**

- the model will use the same vector for the same word, example:
    - "Hello" -> 01 -> [0.1, 0.2, 0.3]
    - "Hello" -> 01 -> [0.1, 0.2, 0.3]

    **BUT** -> before sending it to ATTENTION, there's a process called "positional encoding" to add the position of the word in the sentence - THIS HAPPENS FOR ALL WORD

- it uses Byte-Pair Encoding algorithm to do that -> each model defines its own vocabulary -> hello can be 1 token or 100 tokens, depending on the model.

- these numbers are  MODIFIED - in other words, the model changes the numbers to make the model learn the relationship between the tokens.

- embedding_dim -> is the number of dimensions of the vector, example:
    - embedding_dim = 3 -> [0.1, 0.2, 0.3]
    - embedding_dim = 6 -> [0.1, 0.2, 0.3, 0.4, 0.5, 0.6]

- num_embeddings -> is the quantity of words (tokens) that the model knows, example:
    - num_embeddings = 4
    - line 0  "o"      ->  [0.12, -0.43,  0.87] // embedding_dim = 3
    - line 1  "gato"   ->  [0.38, -1.20,  0.76]
    - line 2  "subiu"  ->  [0.91,  0.44, -0.23]
    - line 3  "no"     ->  [0.05, -0.88,  0.42]

## How does the code learns after embedding?


- these embeddings pass at phase called layers - THE NUMBERS OF EMBEDDING CHANGES ON EACH LAYER

- the numbers of layer is an architecture decision

- Attention + Feed Forward = 1 block (layer) - every model has a different number of layers - but each layer is the same

Attention — the token looks at all other tokens in the sentence and decides which are relevant to it. For example, in the sentence "the animal didn't cross the road because it was tired", the "was" needs to understand that it refers to the "animal", not to the "road". Attention solves this - THIS IS A MATH OPERATION

Feed Forward — after the Attention, each token is transformed individually by a mini-neural network. It's here that the model applies the knowledge it learned during training — associations, facts, language patterns - THIS IS ALSO A MATH OPERATION


## ENTIRE FLOW

Frase: "o gato mordeu o cachorro"
        ↓
Tokenização
        ↓
[0, 1, 2, 0, 3]   ← IDs dos tokens
        ↓
Embedding          ← cada ID vira um vetor (busca na tabela)
        ↓
[                  
  [0.12, -0.43,  0.87],   ← "o"
  [0.38, -1.20,  0.76],   ← "gato"
  [0.91,  0.44, -0.23],   ← "mordeu"
  [0.12, -0.43,  0.87],   ← "o"       (igual ao primeiro)
  [0.77,  0.31, -0.91],   ← "cachorro"
]
        ↓
Positional Encoding   ← soma vetor de posição em cada token
        ↓
[
  [0.12, -0.43,  0.87] + [pos_0],   ← "o" na posição 0
  [0.38, -1.20,  0.76] + [pos_1],   ← "gato" na posição 1
  [0.91,  0.44, -0.23] + [pos_2],   ← "mordeu" na posição 2
  [0.12, -0.43,  0.87] + [pos_3],   ← "o" na posição 3  ← agora diferente!
  [0.77,  0.31, -0.91] + [pos_4],   ← "cachorro" na posição 4
]
        ↓
Bloco 1 (Attention + Feed Forward)
        ↓
Bloco 2 (Attention + Feed Forward)
        ↓
...
        ↓
Bloco N (Attention + Feed Forward)
        ↓
Saída → próxima palavra

## Parameters vs input vs hyperparameters

Parameters → numbers that the model learned and saved fixed - it can be changed during the training

Input → text that you send to the model - it changes in every prompt you send

Hyperparameter → decision of the architecture you take BEFORE the training - embedding_dim, num_embeddings, number of blocks, etc - it is not learned - it is defined by you



## Interesting Topics

Mecanismo de Attention — the heart of the Transformer; explains why LLMs understand context so well
Positional encoding — how the model learns the order of the words in the sentence
Scaling laws — mathematical laws that describe how the quality of the model grows with more data and parameters
Mixture of Experts (MoE) — architecture that allows having billions of parameters (weights) without activating all at each inference - each model is a mixture of experts, and each expert is a model that treat a specific part of the data
LoRA — lightweight fine-tuning technique to specialize models with few resources
Chunking strategies — how to divide documents for RAG in an efficient way