---
layout: post
title: TEST
author: Younsle
date: '2020-12-07'
category: pwn
summary: write-up
thumbnail: /assets/img/posts/pwn/Midnight_Sun/MGSCTF.png
changefreq : weekly
---

+-----------------+---------------------------------------------------+
| **Scan Plugin** | **Basic Network Scan**                            |
+=================+===================================================+
| **General**     |   이름          Metasploitable_3                  |
|                 |   ------------- ------------------                |
|                 |   설명          취약점 분석                       |
|                 |   작업공간      My Scans                          |
|                 |   호스트 대역   192.168.111.146                   |
+-----------------+---------------------------- -----------------------+
| **Discovery**   |   **Host Discovery**                              |
|                 |   --                                              |
|                 | -------------------- ---------------------------- |
|                 |                                                   |
|                 | **General Settings**   Test the local Nessus host |
|                 |   **Ping Methods**       ARP, TCP, ICMP, UDP      |
|                 |                                                   |
|                 | ※ Test the local Nessus host: 대상 네트워크       |
|                 | 대역에 스캐너가 존재할 경우 스캔 여부를 결정      |
+-----------------+---------------------------------------------------+
| **Assessment**  | +----------------------+----------------------+   |
|                 | | **Override normal    | Show potential false |   |
|                 | | accuracy**           | alarms               |   |
|                 | +======================+======================+   |
|                 | | Perform thorough     |                      |   |
|                 | | tests                |                      |   |
|                 | |                      |                      |   |
|                 | | (may disrupt your    |                      |   |
|                 | | network or impact    |                      |   |
|                 | | scan speed)          |                      |   |
|                 | +----------------------+----------------------+   |
|                 |                                                   |
|                 | ※ Show potential false alarms: 확실치 않은        |
|                 | 취약점을 필터링하지 않고 모든 취약점에 대해       |
|                 | 리포팅을 진행한다.                                |
|                 |                                                   |
|                 | ※ Perform thorough tests: 다수의 플러그인을       |
|                 | 활용하여 취약성 서비스를 검사한다. (많은 양의     |
|                 | 네트워크 트래픽과 분석을 야기할 수 있다.)         |
+-----------------+---------------------------------------------------+