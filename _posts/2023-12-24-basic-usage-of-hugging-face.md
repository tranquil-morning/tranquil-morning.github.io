---
title: Basic Usage of Hugging Face
date: 2023-12-24 20:40:00 +0900
categories: [Machine Learning, Deep Learning, Transformer],
tags: [machine learning, deep learning, transformer]
math: true
image: /assets/img/previews/2023-12-24-basic-usage-of-hugging-face.png
---


## Working with pipelines


Hugging Face에서 제공하는 🤗 Transformers 라이브러리의 가장 기본적인 사용법은 pipeline을 사용하는 것입니다.
pipeline은 텍스트를 입력으로 받아서 텍스트를 출력으로 내보내는 모델을 간단하게 사용할 수 있도록 해줍니다.

```python
from transformers import pipeline
classifier = pipeline("sentiment-analysis")
classifier("I've been waiting for a HuggingFace course my whole life.")
# [{'label': 'POSITIVE', 'score': 0.9598047137260437}]
```

여러 문장을 입력으로 받는 것 역시 가능합니다.

```python
classifier([
    "I've been waiting for a HuggingFace course my whole life.", 
    "I hate this so much!"
])

# [{'label': 'POSITIVE', 'score': 0.9598047137260437}
#  {'label': 'NEGATIVE', 'score': 0.9996438026428223}]
```

classifier가 처음 초기화되었을 때, 디폴트 값으로 설정된 모델이 다운로드 됩니다.
모델이 다운로드 된 이후 캐쉬에 저장되기 때문에, 다음에 같은 모델을 사용할 때는 다시 다운로드 받지 않습니다.

text를 pipeline에 넘길 때 세가지 단계를 거치게 됩니다.
1. 모델이 이해할 수 있도록 텍스트를 선처리합니다.
2. 선처리된 텍스트를 모델에 넘겨줍니다.
3. 모델의 출력을 다시 사람이 이해할 수 있도록 후처리합니다.

pipeline은 여러가지 다른 task들을 지원합니다.

* feature-extraction
* fill-mask
* ner (named entity recognition)
* question-answering
* sentiment-analysis
* summarization
* text-generation
* translation
* zero-shot-classification

이 중 몇가지에 대해서 살펴보겠습니다.

### Zero-shot classification

레이블이 지정되지 않은 텍스트를 분류해야 하는 작업부터 시작하겠습니다. 텍스트에 레이블을 다는 작업은 일반적으로 시간이 많이 걸리고 도메인 전문 지식이 필요하기 때문에 실제 프로젝트에서 흔히 볼 수 있는 시나리오입니다. 이 사용 사례에서 제로 샷 분류 파이프라인은 매우 유용합니다. 분류에 사용할 레이블을 지정할 수 있으므로 사전 학습된 모델의 레이블에 의존할 필요가 없습니다. 모델이 이 두 가지 레이블을 사용하여 문장을 긍정 또는 부정으로 분류하는 방법을 이미 살펴보았지만, 원하는 다른 레이블 세트를 사용하여 텍스트를 분류할 수도 있습니다.

```python
classifier = pipeline("zero-shot-classification")
classifier(
    "This is a course about the Transformers library",
    candidate_labels=["education", "politics", "business"],
)

# {'sequence': 'This is a course about the Transformers library'
#  'labels': ['education', 'business', 'politics']
#  'scores': [0.8445963859558105, 0.11197651141881943, 0.04342708304595947]}
```

이 파이프라인은 _zero-shot_ 이라고 불리는데, 모델에 대한 파인 튜닝이 필요하지 않기 때문입니다.

### Text generation

이제 파이프라인을 사용하여 텍스트를 생성해보겠습니다. 이 파이프라인의 경우, 주어진 텍스트를 기반으로 모델이 이어지는 텍스트를 생성합니다. 스마트폰에서 흔히 볼 수 있는 자동 완성 기능과 비슷합니다.

```python
generator = pipeline("text-generation")
generator("In this course, we will teach you how to")
# [{'generated_text': 'In this course, we will teach you how to use the Transformers library to solve text generation'}]
```

`num_return_sequences` 파라미터를 사용하여 생성할 텍스트의 수를 지정할 수 있습으며, `max_length` 파라미터를 사용하여 생성할 텍스트의 최대 길이를 지정할 수 있습니다.

### Using any model from the Hub in a pipeline

이전까지는 default 모델을 사용하였으나, Hugging Face Hub에서 제공하는 모든 모델을 사용할 수 있습니다. 모델을 사용하려면 모델의 이름을 지정해야 합니다. 모델의 이름은 모델 카드에서 찾을 수 있습니다.

_distilgpt2_ 모델을 사용해보겠습니다.

```python
generator = pipeline("text-generation", model="distilgpt2")
generator(
    "In this course, we will teach you how to",
    max_length=30,
    num_return_sequences=1,
)

# [{'generated_text': 'In this course, we will teach you how to manipulate the world and '
#                     'move your mental and physical capabilities to your advantage.'}
```

### Mask filling

마스크 필링은 문장의 빈칸을 채워주는 기능입니다.

```python
unmasker = pipeline("fill-mask")
unmasker("This course will teach you all about <mask> models.", top_k=1)
# [{'sequence': 'This course will teach you all about mathematical models.',
#   'score': 0.19610512256622314,
#   'token': 30412,
#   'token_str': ' mathematical'}
```

`top_k`는 `<mask>`를 채울 때 사용할 후보의 수를 지정합니다.

### Named entity recognition

NER은 텍스트에서 이름이 있는 엔티티를 찾는 작업입니다. 예를 들어, 텍스트에서 사람, 장소, 회사 등을 찾는 작업입니다.

```python
ner = pipeline("ner", grouped_entities=True)
ner("My name is Sylvain and I work at Hugging Face in Brooklyn.")


# [{'entity_group': 'PER', 'score': 0.99816, 'word': 'Sylvain', 'start': 11, 'end': 18}, 
#  {'entity_group': 'ORG', 'score': 0.97960, 'word': 'Hugging Face', 'start': 33, 'end': 45}, 
#  {'entity_group': 'LOC', 'score': 0.99321, 'word': 'Brooklyn', 'start': 49, 'end': 57}
# ]
```

### Question answering

Question answering은 주어진 context에서 주어진 질문에 대한 답을 찾는 작업입니다.

```python
question_answerer = pipeline("question-answering")
question_answerer(
    question="Where do I work?",
    context="My name is Sylvain and I work at Hugging Face in Brooklyn"
)

# {'score': 0.6385916471481323, 'start': 33, 'end': 45, 'answer': 'Hugging Face'}
```
이 파이프라인은 주어진 context에서 정보를 추출하는 것이기에, 항상 정답을 찾는 것은 아닙니다.

### Summarization

summarization은 긴 텍스트를 요약하는 작업으로, 주어진 문장에서 중요한 부부만을 추출하는 것입니다.

```python
summarizer = pipeline("summarization")
summarizer(
    """
    America has changed dramatically during recent years. Not only has the number of 
    graduates in traditional engineering disciplines such as mechanical, civil, 
    electrical, chemical, and aeronautical engineering declined, but in most of 
    the premier American universities engineering curricula now concentrate on 
    and encourage largely the study of engineering science. As a result, there 
    are declining offerings in engineering subjects dealing with infrastructure, 
    the environment, and related issues, and greater concentration on high 
    technology subjects, largely supporting increasingly complex scientific 
    developments. While the latter is important, it should not be at the expense 
    of more traditional engineering.

    Rapidly developing economies such as China and India, as well as other 
    industrial countries in Europe and Asia, continue to encourage and advance 
    the teaching of engineering. Both China and India, respectively, graduate 
    six and eight times as many traditional engineers as does the United States. 
    Other industrial countries at minimum maintain their output, while America 
    suffers an increasingly serious decline in the number of engineering graduates 
    and a lack of well-educated engineers.
"""
)

# [{'summary_text': ' America has changed dramatically during recent years . The '
#                   'number of engineering graduates in the U.S. has declined in '
#                   'traditional engineering disciplines such as mechanical, civil '
#                   ', electrical, chemical, and aeronautical engineering . Rapidly '
#                   'developing economies such as China and India, as well as other '
#                   'industrial countries in Europe and Asia, continue to encourage '
#                   'and advance engineering .'}]
```

text generation과 마찬가지로, `max_length`와 `min_length` 파라미터를 사용하여 요약할 텍스트의 길이를 지정할 수 있습니다.


### Translation

번역은 텍스트를 다른 언어로 번역하는 작업입니다.

```python
translator = pipeline("translation", model="Helsinki-NLP/opus-mt-ko-en")
translator("이것은 Transformers 라이브러리에 대한 강의입니다.")

# [{'translation_text': 'This is a lecture on the Transformers Library.'}]
```
