---
layout: post
title: ARM Register const load
author: Younsle
date: '2021-02-10'
category: reversing
summary: reversing
thumbnail: /assets/img/posts/reversing/ARM/ARM-overview/brokenARM.png
changefreq : weekly
---

# 레지스터에 상수 값 로드

- 메모리에서 데이터 로드를 수행하지 않고는 단일 명령어의 레지스터에 임의의 32비트 상수를 로드할 수 없다.
- ARM 명령어의 길이가 32 비트에 불과 하기 때문
- Thumb 명령어에는 비슷한 제한이 걸려 있다.

- 데이터 로드와 함께 32 비트 값을 레지스터에 로드 할 수 있지만 일반적으로 사용되는 많은 상수를 로드하는 보다 직접적이고 효율적인 방법이 있다.
- 또한 일반적으로 사용되는 많은 상수를 별도의 로드 작업 없이 데이터 처리 명령어 내에서 피연산자로 직접 포함할 수 있다.

## 직접 로딩 MOV, MVN Instruction

### MOV Register

- MOV 명령은 모든 8 비트 상수 값을 로드 하여 0x0 ~ 0xFF (0-255) 범위를 제공한다.
- 또한 이러한 값을 짝수로 회전 할 수 도 있다.

### MVN Register

- MVN은 이러한 값의 비트 보수를 로드 할 수 있다.
- 숫자 값은 `-(n+1)`

- 필요한 회전에 대해 계산할 필요가 없으며 어셈블러가 계산을 수행한다.
- MOV, MVN을 사용할지 결정할 필요가 없다.
- 어셈블러는 적절한 것을 사용한다.
- 값이 어셈블리 시간 변수 인 경우 유용하다.

- 생성 할 수 없는 상수 를 사용하여 명령어를 작성하면 어셈블러에서 오류를 보고한다.

```cpp
Immediate n out of range for this operation.
```