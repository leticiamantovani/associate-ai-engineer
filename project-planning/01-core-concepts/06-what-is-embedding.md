# what is embedding?

- embedding is a process that we convert strings into numbers (vectors) -> [0.21, 0.10]

- in the middle of the process,, strings will be converted into tokens (encoding) (IDs) -> [100, 200, 300], and then, into vectors -> [0.21, 0.10]


# how we do this

- we use a model to convert strings into vectors

- example:

```python
from openai import OpenAI
client = OpenAI()

response = client.embeddings.create(
    input="Your text string goes here",
    model="text-embedding-3-small"
)

print(response.data[0].embedding)
```

response:

```
{
  "object": "list",
  "data": [
    {
      "object": "embedding",
      "index": 0,
      "embedding": [
        -0.006929283495992422, -0.005336422007530928, -4.547132266452536e-5,
        -0.024047505110502243
      ]
    }
  ],
  "model": "text-embedding-3-small",
  "usage": {
    "prompt_tokens": 5,
    "total_tokens": 5
  }
}
```

## how to interpret the response

- think of the embedding number as a vector, the other number too, the distance between the angle of the vectors is the similarity between the strings

- the closer the angle, the more similar the strings are

- the closer the number compared to the other, the more similar the strings are

- we alwaus use cosine, this choice doesnt matter too much cause embedding are normalized to 1, this means that cosine similarity is fastly using a dot product

- cosine and euclidean will return the same ranking, not the same result, as we have normalized the vectors to 1

- normalization = the length of the vector is 1

## dimension parameter

- we have the dimension parameter that defines the length of the vector number 

- we do this sometimes to reduce the cost of the embedding, or to fit in database, but we can lose some accuracy

## many situations we can use embedding

Search (where results are ranked by relevance to a query string)
Clustering (where text strings are grouped by similarity)
Recommendations (where items with related text strings are recommended)
Anomaly detection (where outliers with little relatedness are identified)
Diversity measurement (where similarity distributions are analyzed)
Classification (where text strings are classified by their most similar label)