---
title: Brilliant Fibonacci Challenge
date: 2023-12-23 11:03:00 +0900
categories: [Brilliant, Programming]
tags: [brilliant, programming, python, rust, r]
math: true
image:
  path: /assets/img/previews/2023-12-23-brilliant-fibonacci-challenge.png
  alt: visualizations of the Fibonacci sequence
---

>"숫자는 삶을 가지고 있다. 그들은 종이 위의 상징물이 아니다."
>> 샤쿤탈라 데비

## 피보나치 수열

피보나치 수열은 한 숫자가 그 앞에 있는 두 숫자의 합이 되는 일련의 숫자입니다. 0과 1로 시작하는 수열은 0, 1, 1, 2, 3, 5, 8, 13, 21, 34 등의 순서로 진행됩니다. 일반적으로 $x_n = x_{n-1} + x_{n-2}$로 표현됩니다.

## Challenge

```python
# Write a function that returns the nth Fibonacci number
def fibonacci(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fibonacci(n-1) + fibonacci(n-2)

# Write a function that returns the nth Fibonacci number using a cache
fibonacci_cache = {}
def fibonacci_cache(n):
    if n in fibonacci_cache:
        return fibonacci_cache[n]
    elif n == 0:
        value = 0
    elif n == 1:
        value = 1
    else:
        value = fibonacci_cache(n-1) + fibonacci_cache(n-2)
    fibonacci_cache[n] = value
    return value
    

# Calculate efficiency of the cache
import time
start = time.time()
fibonacci_cache(30)
end = time.time()
print(end - start) # 0.016 seconds

start = time.time()
fibonacci(30)
end = time.time()
print(end - start) # 0.278 seconds
```

```rust
// Write a function that returns the nth Fibonacci number
fn fibonacci(n: u32) -> u32 {
    if n == 0 {
        return 0;
    } else if n == 1 {
        return 1;
    } else {
        return fibonacci(n-1) + fibonacci(n-2);
    }
}
```

```r
# Write a function that returns the nth Fibonacci number
fibonacci <- function(n) {
    if (n == 0) {
        return(0)
    } else if (n == 1) {
        return(1)
    } else {
        return(fibonacci(n-1) + fibonacci(n-2))
    }
}
```

