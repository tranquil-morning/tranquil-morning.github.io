---
title: Humming Bird - Compiling traditional ML models to Tensor
date: 2023-12-23 23:41:00 +0900
categories: [Algorithm]
tags: [algorithm]
math: true
image: /assets/img/previews/2023-12-23-humming-bird.png
---

> "필요 이상으로 실체를 늘려서는 안 된다."
>> 오컴의 윌리엄

  
  
  
## Humming Bird는...

Hummingbird는 기존 ML 모델을 텐서 계산으로 컴파일하는 라이브러리입니다. 이를 통해 사용자는 PyTorch와 같은 신경망 프레임워크를 이용하여 전통적 ML 모델을 가속화할 수 있습니다. Hummingbird는 신경망 프레임워크의 최신 최적화, 하드웨어 가속, 전통적 및 신경망 모델을 지원하는 단일 플랫폼의 이점을 제공합니다. 현재 Hummingbird는 PyTorch, TorchScript, ONNX, TVM으로 훈련된 전통적 ML 모델을 변환할 수 있으며, scikit-learn 결정 트리, 랜덤 포레스트, LightGBM, XGBoost 분류기/회귀 분석기 등 다양한 모델을 지원합니다.

## Random Forest 예제

```python
import numpy as np
from sklearn.ensemble import RandomForestClassifier
from hummingbird.ml import convert, load
import time

# Set the seed for reproducibility
np.random.seed(42)

# Create some random data for binary classification
num_classes = 2

X = np.random.rand(100000, 28)
y = np.random.randint(num_classes, size=100000)



# Create and train a model
rf_original = RandomForestClassifier(n_estimators=10, max_depth=10)
rf_original.fit(X, y)

# Create the Hummingbird model
rf_hb = convert(rf_original, 'pytorch')
rf_hb.to('cuda')


# Calculate time for prediction
start_time = time.time()
preds = rf_original.predict(X)
end_time = time.time()
print("Original model inference time: {}".format(end_time - start_time)) #0.15s

start_time = time.time()
preds = rf_hb.predict(X)
end_time = time.time()
print("Hummingbird model inference time: {}".format(end_time - start_time)) #0.0121s


rf_hb.save("rf_hb")
```




