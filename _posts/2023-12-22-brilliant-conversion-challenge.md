---
title: Brilliant Conversion Challenge
date: 2023-12-22 21:33:00 +0900
categories: [Brilliant, Programming]
tags: [brilliant, programming, python, rust, r]
---


> "외국어를 모르는 사람은 자신의 언어도 모른다."
>> 요한 볼프강 폰 괴테



## Challenge Part 1

```python
seconds = 155
minutes = seconds // 60
leftover_seconds = seconds % 60

print(seconds, "seconds is the same as")
print(minutes, "minutes and", leftover_seconds, "seconds")
```

```rust
fn main() {
    let seconds = 155;
    let minutes = seconds / 60;
    let leftover_seconds = seconds % 60;

    println!("{} seconds is the same as", seconds);
    println!("{} minutes and {} seconds", minutes, leftover_seconds);
}
```

```r
seconds <- 155
minutes <- seconds %/% 60
leftover_seconds <- seconds %% 60

print(paste(seconds, "seconds is the same as"))
print(paste(minutes, "minutes and", leftover_seconds, "seconds"))
```

## Challenge Part 2

```python
seconds = 14926

hours = seconds // 3600

minutes = (seconds-3600*hours) // 60

final_seconds = seconds - 3600*hours - 60*minutes

print(str(seconds) , "seconds is the same as")
print(str(hours) , "hours," , minutes  , "minutes, and" , final_seconds , "seconds")
```

```rust
fn main() {
    let seconds = 14926;
    let hours = seconds / 3600;
    let minutes = (seconds - 3600 * hours) / 60;
    let final_seconds = seconds - 3600 * hours - 60 * minutes;

    println!("{} seconds is the same as", seconds);
    println!("{} hours, {} minutes, and {} seconds", hours, minutes, final_seconds);
}
```

```r
seconds <- 14926
hours <- seconds %/% 3600
minutes <- (seconds - 3600 * hours) %/% 60
final_seconds <- seconds - 3600 * hours - 60 * minutes

print(paste(seconds, "seconds is the same as"))
print(paste(hours, "hours,", minutes, "minutes, and", final_seconds, "seconds"))
```
