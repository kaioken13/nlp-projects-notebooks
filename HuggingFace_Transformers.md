# HuggingFace Transformers


```python
from transformers import pipeline, AutoTokenizer, AutoModelForSequenceClassification
import torch
```


```python
sentiment_classifier = pipeline("sentiment-analysis")
```

    No model was supplied, defaulted to distilbert/distilbert-base-uncased-finetuned-sst-2-english and revision 714eb0f.
    Using a pipeline without specifying a model name and revision in production is not recommended.



    Loading weights:   0%|          | 0/104 [00:00<?, ?it/s]



```python
sentiment_classifier("I'm so excited to learn about Large Language Models.")
```




    [{'label': 'POSITIVE', 'score': 0.9997144341468811}]




```python
ner = pipeline("ner", model = "dslim/bert-base-NER")
```


    config.json:   0%|          | 0.00/829 [00:00<?, ?B/s]



    model.safetensors:   0%|          | 0.00/433M [00:00<?, ?B/s]



    Loading weights:   0%|          | 0/199 [00:00<?, ?it/s]


    BertForTokenClassification LOAD REPORT from: dslim/bert-base-NER
    Key                      | Status     |  | 
    -------------------------+------------+--+-
    bert.pooler.dense.bias   | UNEXPECTED |  | 
    bert.pooler.dense.weight | UNEXPECTED |  | 
    
    Notes:
    - UNEXPECTED	:can be ignored when loading from different task/architecture; not ok if you expect identical arch.



    tokenizer_config.json:   0%|          | 0.00/59.0 [00:00<?, ?B/s]



    vocab.txt: 0.00B [00:00, ?B/s]



    added_tokens.json:   0%|          | 0.00/2.00 [00:00<?, ?B/s]



    special_tokens_map.json:   0%|          | 0.00/112 [00:00<?, ?B/s]



```python
ner("Her name is Anna and she works in New York City at Morgan Stanley")
```




    [{'entity': 'B-PER',
      'score': np.float32(0.9956285),
      'index': 4,
      'word': 'Anna',
      'start': 12,
      'end': 16},
     {'entity': 'B-LOC',
      'score': np.float32(0.999548),
      'index': 9,
      'word': 'New',
      'start': 34,
      'end': 37},
     {'entity': 'I-LOC',
      'score': np.float32(0.9993838),
      'index': 10,
      'word': 'York',
      'start': 38,
      'end': 42},
     {'entity': 'I-LOC',
      'score': np.float32(0.9995616),
      'index': 11,
      'word': 'City',
      'start': 43,
      'end': 47},
     {'entity': 'B-ORG',
      'score': np.float32(0.93956363),
      'index': 13,
      'word': 'Morgan',
      'start': 51,
      'end': 57},
     {'entity': 'I-ORG',
      'score': np.float32(0.98586315),
      'index': 14,
      'word': 'Stanley',
      'start': 58,
      'end': 65}]




```python
zeroshot_classifier = pipeline("zero-shot-classification", model="facebook/bart-large-mnli")
```


    config.json: 0.00B [00:00, ?B/s]



    model.safetensors:   0%|          | 0.00/1.63G [00:00<?, ?B/s]



    Loading weights:   0%|          | 0/515 [00:00<?, ?it/s]



    tokenizer_config.json:   0%|          | 0.00/26.0 [00:00<?, ?B/s]



    vocab.json: 0.00B [00:00, ?B/s]



    merges.txt: 0.00B [00:00, ?B/s]



    tokenizer.json: 0.00B [00:00, ?B/s]



```python
sequence_to_classify = "one day I will see the world"
candidate_labels = ["travel", "cooking", "dancing"]
```


```python
zeroshot_classifier(sequence_to_classify, candidate_labels)
```




    {'sequence': 'one day I will see the world',
     'labels': ['travel', 'dancing', 'cooking'],
     'scores': [0.9938650727272034, 0.0032737997826188803, 0.002861031796783209]}



# Pre-trained Tokenizers


```python
model = "bert-base-uncased"
```


```python
tokenizer = AutoTokenizer.from_pretrained(model)
```


    config.json:   0%|          | 0.00/570 [00:00<?, ?B/s]



    tokenizer_config.json:   0%|          | 0.00/48.0 [00:00<?, ?B/s]



    vocab.txt: 0.00B [00:00, ?B/s]



    tokenizer.json: 0.00B [00:00, ?B/s]



```python
sentence = "I'm so excited to be learning about Large Language Models"
```


```python
input_ids = tokenizer(sentence)
print(input_ids)
```

    {'input_ids': [101, 1045, 1005, 1049, 2061, 7568, 2000, 2022, 4083, 2055, 2312, 2653, 4275, 102], 'token_type_ids': [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 'attention_mask': [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]}



```python
tokens = tokenizer.tokenize(sentence)
print(tokens)
```

    ['i', "'", 'm', 'so', 'excited', 'to', 'be', 'learning', 'about', 'large', 'language', 'models']



```python
token_ids = tokenizer.convert_tokens_to_ids(tokens)
print(token_ids)
```

    [1045, 1005, 1049, 2061, 7568, 2000, 2022, 4083, 2055, 2312, 2653, 4275]



```python
decoded_ids = tokenizer.decode(token_ids)
print(decoded_ids)
```

    i ' m so excited to be learning about large language models



```python
tokenizer.decode(101)
```




    '[CLS]'




```python
tokenizer.decode(102)
```




    '[SEP]'




```python
model2 = "xlnet-base-cased"
```


```python
tokenizer2 = AutoTokenizer.from_pretrained(model2)
```


    config.json:   0%|          | 0.00/760 [00:00<?, ?B/s]



    spiece.model:   0%|          | 0.00/798k [00:00<?, ?B/s]



    tokenizer.json: 0.00B [00:00, ?B/s]



```python
input_ids2 = tokenizer2(sentence)
print(input_ids2)
```

    {'input_ids': [35, 26, 98, 102, 5564, 22, 39, 1899, 75, 9332, 9531, 7770, 23, 4, 3], 'attention_mask': [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]}



```python
tokens = tokenizer2.tokenize(sentence)
print(tokens)
```

    ['▁I', "'", 'm', '▁so', '▁excited', '▁to', '▁be', '▁learning', '▁about', '▁Large', '▁Language', '▁Model', 's']



```python
token_ids = tokenizer2.convert_tokens_to_ids(tokens)
print(token_ids)
```

    [35, 26, 98, 102, 5564, 22, 39, 1899, 75, 9332, 9531, 7770, 23]



```python
tokenizer2.decode(4)
```




    '<sep>'




```python
tokenizer2.decode(3)
```




    '<cls>'



# HuggingFace and Pytorch/TensorFlow


```python
print(sentence)
print(input_ids)
```

    I'm so excited to be learning about Large Language Models
    {'input_ids': [101, 1045, 1005, 1049, 2061, 7568, 2000, 2022, 4083, 2055, 2312, 2653, 4275, 102], 'token_type_ids': [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0], 'attention_mask': [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]}



```python
tokenizer = AutoTokenizer.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")
```


    config.json:   0%|          | 0.00/629 [00:00<?, ?B/s]



    tokenizer_config.json:   0%|          | 0.00/48.0 [00:00<?, ?B/s]



    vocab.txt: 0.00B [00:00, ?B/s]



```python
input_ids_pt = tokenizer(sentence, return_tensors="pt")
print(input_ids_pt)
```

    {'input_ids': tensor([[ 101, 1045, 1005, 1049, 2061, 7568, 2000, 2022, 4083, 2055, 2312, 2653,
             4275,  102]]), 'token_type_ids': tensor([[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]), 'attention_mask': tensor([[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]])}



```python
model = AutoModelForSequenceClassification.from_pretrained("distilbert-base-uncased-finetuned-sst-2-english")
```


    model.safetensors:   0%|          | 0.00/268M [00:00<?, ?B/s]



    Loading weights:   0%|          | 0/104 [00:00<?, ?it/s]



```python
with torch.no_grad():
    logits = model(**input_ids_pt).logits

predicted_class_id = logits.argmax().item()
model.config.id2label[predicted_class_id]
```




    'POSITIVE'



# Saving and loading models


```python
model_directory = "my_saved_models"
```


```python
tokenizer.save_pretrained(model_directory)
```




    ('my_saved_models/tokenizer_config.json', 'my_saved_models/tokenizer.json')




```python
model.save_pretrained(model_directory)
```


    Writing model shards:   0%|          | 0/1 [00:00<?, ?it/s]



```python
my_tokenizer = AutoTokenizer.from_pretrained(model_directory)
```


```python
my_model = AutoModelForSequenceClassification.from_pretrained(model_directory)
```


    Loading weights:   0%|          | 0/104 [00:00<?, ?it/s]



```python

```
