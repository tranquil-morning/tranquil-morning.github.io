---
title: Brilliant Fibonacci Challenge
date: 2023-12-23 11:03:00 +0900
categories: [Brilliant, Programming]
tags: [brilliant, programming, python, rust, r]
math: true
---

>"Numbers have life; they're not just symbols on paper."
>> Shakuntala Devi

## Fibonacci Sequence

The Fibonacci sequence is a series of numbers where a number is the sum of the two numbers before it. Starting with 0 and 1, the sequence goes 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, and so forth. Written as a rule, the expression is $x_n = x_{n-1} + x_{n-2}$.

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

