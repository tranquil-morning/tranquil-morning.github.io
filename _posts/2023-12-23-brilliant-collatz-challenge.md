---
title: Brilliant Collatz Challenge
date: 2023-12-23 11:23:00 +0900
categories: [Brilliant, Programming]
tags: [brilliant, programming, python, rust, r]
image:
  path: /assets/img/previews/2023-12-23-brilliant-collatz-challenge copy.png
  alt: visualizations of the collatz conjecture
---

>"수학에서는 문제를 해결하는 것보다 그것을 제안하는 예술이 더 높은 가치를 지녀야 한다."
>> 게오르크 칸토어

## 콜라츠 추측

콜라츠 추측은 어떤 자연수 n이라고 할 때, 다음과 같은 작업을 반복하면 1로 만들 수 있다는 추측입니다.

이는 아래와 같은 단계를 따릅니다 :

1. n이 짝수라면 2로 나눕니다.
2. n이 홀수라면 3을 곱하고 1을 더합니다.
3. 이 과정을 1이 될 때까지 반복합니다.

예를 들어, n이 6이라면, 6 → 3 → 10 → 5 → 16 → 8 → 4 → 2 → 1 이 되어 총 8번 만에 1이 됩니다.

이 추측은 아직 증명되지 않았습니다. 하지만, 1,000,000 이하의 모든 짝수는 이 추측을 만족한다는 것은 증명되었습니다.

## Challenge

```python
# Write a function that returns steps to reach 1
def collatz_steps(n):
    steps = 0
    while n != 1:
        if n % 2 == 0:
            n = n // 2
        else:
            n = 3*n + 1
        steps += 1
    return steps
```

```rust
// Write a function that returns steps to reach 1
fn collatz_steps(n: u32) -> u32 {
    let mut steps = 0;
    let mut n = n;
    while n != 1 {
        if n % 2 == 0 {
            n = n / 2;
        } else {
            n = 3*n + 1;
        }
        steps += 1;
    }
    steps
}
```

```r
# Write a function that returns steps to reach 1
collatz_steps <- function(n) {
    steps <- 0
    while (n != 1) {
        if (n %% 2 == 0) {
            n <- n / 2
        } else {
            n <- 3*n + 1
        }
        steps <- steps + 1
    }
    steps
}
```