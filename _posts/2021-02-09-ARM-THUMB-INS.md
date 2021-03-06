---
layout: post
title: ARM and THUMB Instruction
author: Younsle
date: '2021-02-09'
category: reversing
summary: reversing
thumbnail: /assets/img/posts/reversing/ARM/ARM-overview/brokenARM.png
changefreq : weekly
---
# Thumb and ARM Instruction


## ARM Instruction

### Load/Store 구조

- ARM에는 메모리 내에 데이터를 직접적으로 접근하는 것이 불가능하다.
- LDR, STR과 같은 명령을 통해서 메모리와 레지스터 사이에 데이터를 전송한다.

### 3-Address date Processing

- 두 개의 source operand, result operand


### ARM 모든 명령어는 조건부 실행 가능

- 모든 ARM 명령어는 CPSR의 ALU 상태 플래그 값에 대해 조건부로 실행될 수 있다.
- 일련의 명령어가 동일한 조건에 종속 될 때 더 좋을 수 있지만 조건부 명령어를 건너 뛰기 위해 분기를 사용할 필요가 없다.
- 데이터 처리 명령어가 이러한 플래그의 상태를 설정하는지 여부를 지정할 수 있다.
- 한 명령어로 설정된 플래그를 사용하여 그 사이에 많은 명령어가 있더라도 다른 명령어의 실행을 제어할 수 있다.

### Register Access

- ARM 상태에서 모든 명령어는 r0 ~ r14 에 액세스가 가능하다.
- 대부부은 r15(pc)에 대한 액세스도 허용한다.
- MRS, MSR 은 이들이 정상적인 데이터 처리 연산에 의해 조작될 수 있는 범용 레지스터로 CPSR, SPSR의 콘텐츠를 이동시킬 수 있다.

### Access to the inline barrel shifter

- ARM 산술 논리 장치에는 시프트 및 회전 작업이 가능한 32비트 배럴 시프터가 있다.
- 모든 ARM 데이터 처리및 단일 레지스터 데이터 전송 명령어에 대한 두 번째 피연사자는 데이터 처리 또는 데이터 전송이 명령어의 일부로 실행되기 전에 이동 될 수 있다.
- 확장된 주소 지정
- 상수로 곱셈
- 상수 생성

## Thumb 명령어 집합

- ARM 명령어 하위 집합이다.
- C, C++ 최적화 되어 있다.
- 모든 Thumb 명령어는 길이가 16비트 이고 메모리에 half word로 정렬되어 저장된다.
- 명령어 주소의 최하위 비트는 Thumb 상태에서 항상 0 이다.
- 일부 명령어는 최하위 비트를 사용하여 분기되는 코드가 Thumb 코드인지 ARM 코드인지 확인한다.
- 레지스터의 전체 32bit 값에서 작동
- 데이터 액세스 및 명령어 가져오기에 전체 32비트 주소를 사용한다.

## Thumb 명령어 기능

 

### 조건부 실행

- 조건부 분기 명령어는 CPSR, ALU 상태 플래그 값에 대해 조건부로 실행할 수 있는 유일한 Thumb 명령어이다.
- 하나 이상의 상위 레지스터가 `MOV` 또는 `ADD` 명령어의 피연산자로 지정된 경우를 제외하고 모든 데이터 처리 명령어는 이러한 플래그를 업데이트 한다.
- 조건을 설정하는 명령어와 이에 종속된 조건 분기 사이에는 데이터 처리 명령어가 있을 수 없다.
- 조건부로 지정하려는 명령어 위에 조건부 분기를 사용해야 한다.

### Register Access

- Thumb 상태에서 대부분의 명령어는 r0 ~ r7에만 액세스 할 수 있으며 Low Register이라고 한다.
- r8 ~ r15 는 제한된 액세스 레지스터이다.
- Thumb 상태에서는 이를 High Register 이라 한다.
- 예를 들어 FIQ 를 사용할 수 있다.

## Thumb and ARM Differences

### Branch Instruction

- 루프를 형성하기 위해 뒤로 분기
- 조건부 구조에서 앞으로 분기
- 서브 루틴으로 분기
- 프로세서를 Thumb 상태에서 ARM 상태로 변경

### Data Processing instructions

- 범용 레지지스터에서 작동한다.
- 대부분의 경우 연산 결과는 세 번째 레지스터가 아닌 피연산자 레지스터 중 하나에 넣어야 한다.
- ARM 상태보다 사용 가능한 데이터 처리 작업이 적다.
- 레지스터 r8 ~ r15에 대한 엑세스가 제한된다.
- CPSR의 ALU 상태 플래그는 MOV, ADD 명령어가 레지스터 r8 ~ r15에 엑세스 하는 경우를 제외하고 항상 이러한 명령어에 의해 업데이트 된다.
- 레지스터 r8 ~ r15에 액세스 하는 Thumb 데이터 처리 명령어는 플래그를 업데이트 할 수 없다.

### Single Register Load and Store Instruction

- LDM, STM 메모리에서 로드하고 r0~r7 범위의 레지스터 서브 세트를 메모리에 저장한다.

### Multi Register Load and Store Instruction

- PUSH, POP 지침은 기본으로 스택 포인터 (r13) 를 사용하여 전체 내림차순 스택을 구현한다.
- r0을 r7로 전송하는 것 외에도 PUSH 링크 레지스터를 저장하고 POP 하고 프로그램 카운터를 로드 할 수 있다.