---
title: Fine Tuning on Hugging Face
date: 2023-12-25 10:40:00 +0900
categories: [Machine Learning, Deep Learning, Transformer],
tags: [machine learning, deep learning, transformer]
image: /assets/img/previews/2023-12-24-basic-usage-of-hugging-face.png
---

## Hub에서 데이터셋 다운로드하기

Hub는 단순히 모델만을 저장하는 곳이 아닙니다. 데이터셋을 저장할 수도 있습니다. 
GLUE 벤치마크에서 제공하는 MRPC 데이터셋을 다운로드 받아보겠습니다.

```python
from datasets import load_dataset

raw_datasets = load_dataset("glue", "mrpc")
raw_datasets

# DatasetDict({
#     train: Dataset({
#         features: ['sentence1', 'sentence2', 'label', 'idx'],
#         num_rows: 3668
#     })
#     validation: Dataset({
#         features: ['sentence1', 'sentence2', 'label', 'idx'],
#         num_rows: 408
#     })
#     test: Dataset({
#         features: ['sentence1', 'sentence2', 'label', 'idx'],
#         num_rows: 1725
#     })
# })

```

이 데이터셋은 3개의 split으로 구성되어 있습니다. train, validation, test입니다. 각각의 split은 여러개의 feature를 가지고 있습니다. 이 데이터셋은 sentence1, sentence2, label, idx로 구성되어 있습니다. 이 데이터셋은 문장이 두개가 주어지고, 이 두 문장이 같은 의미인지 아닌지를 판단하는 데이터셋입니다. label은 0 또는 1로 구성되어 있습니다. 0은 같은 의미가 아니고, 1은 같은 의미라는 의미입니다. idx는 데이터셋의 인덱스입니다.

train의 데이터를 살펴봅시다.

```python
raw_datasets["train"][0]

# {'idx': 0,
#  'label': 1,
#  'sentence1': 'Amrozi accused his brother , whom he called " the witness " , of deliberately distorting his evidence .',
#  'sentence2': 'Referring to him as only " the witness " , Amrozi accused his brother of deliberately distorting his evidence .'}

raw_datasets["train"].features

# {'sentence1': Value(dtype='string', id=None),
#  'sentence2': Value(dtype='string', id=None),
#  'label': ClassLabel(num_classes=2, names=['not_equivalent', 'equivalent'], names_file=None, id=None),
#  'idx': Value(dtype='int32', id=None)}
```

### 데이터 전처리

데이터 전체를 처리하기 전, 하나의 예시를 살펴보겠습니다.

    
```python
from transformers import AutoTokenizer

checkpoint = "bert-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)

inputs = tokenizer("This is the first sentence.", "This is the second one.")
inputs

# { 
#   'input_ids': [101, 2023, 2003, 1996, 2034, 6251, 1012, 102, 2023, 2003, 1996, 2117, 2028, 1012, 102],
#   'token_type_ids': [0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1],
#   'attention_mask': [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
# }
```
위 `input_ids`를 다시 텍스트로 변환해보겠습니다.

```python
tokenizer.convert_ids_to_tokens(inputs["input_ids"])

# ['[CLS]', 'this', 'is', 'the', 'first', 'sentence', '.', '[SEP]', 'this', 'is', 'the', 'second', 'one', '.', '[SEP]']
```

확인할 수 있듯이, 두 문장이 \[CLS\]와 \[SEP\]로 구분되어 있습니다. 이는 BERT 모델이 두 문장을 구분하기 위해서 사용하는 특수 토큰입니다.


이제 데이터셋 전체를 전처리해보겠습니다.

```python
from datasets import load_dataset
from transformers import AutoTokenizer, DataCollatorWithPadding

raw_datasets = load_dataset("glue", "mrpc")
checkpoint = "bert-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)


def tokenize_function(example):
    return tokenizer(example["sentence1"], example["sentence2"], truncation=True)


tokenized_datasets = raw_datasets.map(tokenize_function, batched=True)
data_collator = DataCollatorWithPadding(tokenizer=tokenizer)
```

### Fine Tuning

이제 Fine Tuning을 해보겠습니다. Fine Tuning을 위해서는 Trainer를 사용해야 합니다. 

```python
from transformers import TrainingArguments

training_args = TrainingArguments("test-trainer")

from transformers import AutoModelForSequenceClassification
model = AutoModelForSequenceClassification.from_pretrained(checkpoint, num_labels=2)

from transformers import Trainer

trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=tokenized_datasets["train"],
    eval_dataset=tokenized_datasets["validation"],
    data_collator=data_collator,
)

trainer.train()

predictions = trainer.predict(tokenized_datasets["validation"])
print(predictions.predictions.shape, predictions.label_ids.shape) # (408, 2) (408,)

import evaluate

metric = evaluate.load("glue", "mrpc")
metric.compute(predictions=preds, references=predictions.label_ids)

# {'accuracy': 0.8431372549019608}
```