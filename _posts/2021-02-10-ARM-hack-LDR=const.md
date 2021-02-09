---
layout: post
title: ARM LDR Rd, =const
author: Younsle
date: '2021-02-10'
category: reversing
summary: reversing
thumbnail: /assets/img/posts/reversing/ARM/ARM-overview/brokenARM.png
changefreq : weekly
---

# LDR Rd, =const

- `LDR Rd, =const` 의사 명령은 단일 명령어에서 32비트 숫자 상수를 생성할 수 있다.
- 이 의사 명령어를 사용하여 MOV, MVN 명령어 범위를 벗어난 상수를 생성한다.

### LDR 의사명령은 특정 상수에 대해 가장 효율적인 코드를 생성한다.

- MOV, MVN 명령어를 사용하여 상수를 생성 할 수 있는 경우 어셈블러는 적절한 명령어를 생성한다.
- MOV, MVN 명령어로 상수를 생성할 수 없는 경우 어셈블러는 다음을 수행한다.
    - 값을 literal pool (상수 값을 유지하기 위해 코드에 포함된 메모리의 일부에 배치한다.)
    - Literal pool에서 상수를 읽는 프로그램 기준 주소로 LDR 명령어를 생성한다.

```cpp
LDR      rn, [pc, #offset to literal pool]  ; load register n with one word
                                                ; from the address [pc + offset]
```

- 어셈블러에서 생성한 LDR 명령어 범위 내에 리터럴 풀이 있는지 확인해야 한다.

### Literal pools 배치

- 어셈블러는 각 섹션의 끝에 리터럴 풀을 배치한다.
- 이들은 다음 섹션의 시작 부분에 있는 AREA instruction  또는 어셈블리 끝에 있는 END instruction 의해 정의된다.
- 포함된 파일 끝에 있는 END는 섹션의 끝을 알리지 않는다.
- 큰 섹션에서 기본 리터럴 풀은 하나 이상의 LDR 명령어 범위를 벗어날 수 있다.
- PC에서 상수까지의 오프셋은 다음과 같다.
    - ARM 상태에서 4KB 미만이지만 어느 방향으로 가능
    - Thumb 상태에서 앞으로 1KB 미만

### LDR Rd, = const 의사 명령어에서 상수를 리터럴 풀에 배치해야 하는 경우 어셈블러는 다음과 같다.

- 상수를 사용할 수 있고 이전 리터럴 풀에서 주소를 지정할 수 있는지 확인하며 기존 상수를 처리한다.
- 상수를 아직 사용할 수 없는 경우 next literal pool에 배치하려고 한다.

### next literal pool

- 범위를 벗어난 경우 어셈블러는 오류 메시지를 생성한다.
- 해당 경우 LTORG instruction을 사용하여 코드에 추가 리터럴 풀을 배치해야 한다.
- LTORG는 실패한 LDR 의사 명령어 뒤에 4KB(ARM) 또는 1KB (Thumb) 내에 배치한다.

### LTORG Instruction

- 어셈블러는 모든 코드 섹션의 끝에 현재 리터럴 풀을 어셈블한다.
- 코드 섹션 끝은 AREA 다음 섹션의 시작 부분 또는 어셈블리의 끝 부분에 있는 지시문에 의해 결정된다.
- 이러한 기본 리터럴 풀은 때때로 일부의 범위를 벗어날 수 있다. LDR, LDFD, LDFS 의사 명령어
- LTORG 리터럴 풀이 범위 내에서 어셈블되었는지 확인한다.

### 프로세서가 명령으로 실행을 시도하지 않는 곳에 리터럴 풀을 배치해야 한다.

- 무조건 분기 명령어 뒤에 배치하거나 서브 루틴 끝에 반환 명령어 뒤에 배치한다.

```cpp
AREA     Loadcon, CODE, READONLY
        ENTRY                              
start   BL       func1                     ; Branch 첫 번째 서브루틴
        BL       func2                     ; Branch 두 번째 서브루틴
stop    MOV      r0, #0x18                 ; angel_SWIreason_ReportException
        LDR      r1, =0x20026              ; ADP_Stopped_ApplicationExit
        SWI      0x123456                  ; ARM semihosting SWI
func1
        LDR      r0, =42                   ; => MOV R0, #42
        LDR      r1, =0x55555555           ; => LDR R1, [PC, #offset to
                                           ; Literal Pool 1]
        LDR      r2, =0xFFFFFFFF           ; => MVN R2, #0
        MOV      pc, lr
        LTORG                              ; Literal Pool 1 contains
                                           ; literal Ox55555555
func2
        LDR      r3, =0x55555555           ; => LDR R3, [PC, #offset to
                                           ; Literal Pool 1]
        ; LDR r4, =0x66666666              ; 만약에 주석 처리가 아니라면
                                           ; 실패한다. 왜냐하면 두 개의 리터럴 풀이기 때문
                                           ; 범위에 벗어난다.
        MOV      pc, lr
LargeTable
        SPACE    4200                      ; Starting at the current location,
                                           ; clears a 4200 byte area of memory
                                           ; to zero
        END                                ; Literal Pool 2 is empty
```