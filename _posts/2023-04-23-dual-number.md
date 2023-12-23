---
title: Dual Number & Automatic Differentiation
date: 2023-04-23 23:41:00 +0900
categories: [Mathmatics]
tags: [mathmatics]
math: true
---

> "자연은 단순함과 일체성을 사랑한다."
>> 요하네스 케플러

## Dual Number

Dual Number는 실수의 확장으로, 실수부와 듀얼부로 구성됩니다. 듀얼부는 듀얼 수의 제곱이 0이 되는 수입니다. 듀얼 수는 다음과 같이 표현할 수 있습니다:

$a + b\epsilon$

여기서 $a$는 실수부, $b$는 듀얼부, $\epsilon$은 듀얼부의 제곱이 0이 되는 수입니다.

듀얼 수의 덧셈과 뺄셈은 실수부와 듀얼부를 각각 더하고 빼는 것으로 정의됩니다. 듀얼 수의 곱셈은 다음과 같이 정의됩니다:

$(a + b\epsilon)(c + d\epsilon) = ac + (ad + bc)\epsilon$

## Forward Automatic Differentiation

전진 모드 자동 미분(Forward Mode Automatic Differentiation)은 미분을 계산하고자 하는 함수의 입력 변수를 듀얼 수로 대체하여 함수의 값을 계산합니다. 이를 통해 함수의 값을 계산하고, 이를 통해 미분을 계산할 수 있습니다.

계산 방식은 다음과 같습니다:

$f(x) = f(a + b\epsilon) = f(a) + f'(a)b\epsilon$

예시를 들어보겠습니다:

$f(x) = 3x^2 + x + 1$ 일 때, $f(2)$의 미분값을 계산해보겠습니다.
이를 위해 $x$를 듀얼 수로 대체합니다: $x = 2 + 1\epsilon$. 
이를 함수에 대입하면: $f(2 + 1\epsilon) = 3(2 + 1\epsilon)^2 + (2 + 1\epsilon) + 1$이 됩니다.
이를 풀면: $f(2 + 1\epsilon) = 15 + 13\epsilon$이 됩니다. 
이를 통해 $f'(2)$는 13이라는 것을 알 수 있습니다.

이제 이를 코드로 구현해보겠습니다:

```python
# Dual Number
class DualNumber:
    def __init__(self, real, dual):
        self.real = real
        self.dual = dual

    def add(self, other):
        return DualNumber(self.real + other.real, self.dual + other.dual)

    def sub(self, other):
        return DualNumber(self.real - other.real, self.dual - other.dual)

    def mul(self, other):
        return DualNumber(self.real * other.real, self.real * other.dual + self.dual * other.real)

    def truediv(self, other):
        return DualNumber(self.real / other.real, (self.dual * other.real - self.real * other.dual) / (other.real ** 2))

    def pow(self, other):
        return DualNumber(self.real ** other.real, self.real ** other.real * (other.dual * np.log(self.real) + other.real * self.dual / self.real))

# Automatic Differentiation using Dual Number

def f(x):
    return DualNumber(3,0).mul(x).mul(x).add(x).add(DualNumber(1,0))

x = DualNumber(2, 1)
y = f(x) # y = f(2 + 1ε) = 15 + 13ε
print(y.dual) # 13
```
