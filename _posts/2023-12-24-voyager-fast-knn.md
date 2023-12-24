---
title: Voyager - Fast KNN
date: 2023-12-23 13:40:00 +0900
categories: [Machine Learning, KNN, Algorithm]
tags: [machine learning, knn, algorithm]
math: true
image: /assets/img/previews/2023-12-23-voyager-fast-knn.png
---


## Voyager - Fast KNN

Voyager는 인메모리 벡터 컬렉션에서 KNN을 빠르게 수행하기 위한 라이브러리입니다.
오픈 소스 hnswlib 패키지를 기반으로 하는 HNSW 알고리즘을 사용하며, 편의성과 속도를 위해 다양한 기능이 추가되었습니다.
Voyager는 Spotify의 프로덕션 환경에서 광범위하게 사용되고 있으며, 수많은 사용자 대면 기능을 구동하기 위해 하루에 수억 번 쿼리되고 있습니다.

하기 예제는 voyager를 사용하여 5차원 공간에서 가장 가까운 이웃을 찾는 방법을 보여줍니다.

```python
import numpy as np
from voyager import Index, Space
import time

# Create an empty Index object that can store vectors:
index = Index(Space.Euclidean, num_dimensions=5)
id_a = index.add_item([1, 2, 3, 4, 5])
id_b = index.add_item([6, 7, 8, 9, 10])
id_c = index.add_item([11, 12, 13, 14, 15])
id_d = index.add_item([16, 17, 18, 19, 20])


print(id_a)  # => 0
print(id_b)  # => 1
print(id_c)  # => 2
print(id_d)  # => 3

# Find the two closest elements:
start_time = time.time()
neighbors, distances = index.query([1, 6, 11, 16, 5], k=2)
end_time = time.time()

print(end_time - start_time) # 0.00013

print(neighbors)  # => [1, 0]
print(distances)  # => [109, 224]


vectors = np.array([[1, 2, 3, 4, 5],
          [6, 7, 8, 9, 10],
          [11, 12, 13, 14, 15],
          [16, 17, 18, 19, 20]])

q = np.array([1, 6, 11, 16, 5])

# Calculate the Euclidean distance between the query vector and all vectors in the array
start_time = time.time()
distances = np.sum((vectors - q)**2, axis=1)
neighbors = np.argsort(distances)[:2]
end_time = time.time()

print(end_time - start_time) # 0.0002


print(neighbors) # => [1, 0]
print(distances[neighbors]) # => [109, 224]
```
