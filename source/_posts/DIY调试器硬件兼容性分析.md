---
title: DIY调试器硬件兼容性分析
date: 2023-11-27 13:33:56
categories:
- 硬件
- 嵌入式系统
tags:
- STM32F103CxT6
- 调试器
- daplink
- jlink
- stlink
- IO分布
- 硬件兼容性
---

分析三个调试器在STM32F103CxT6上的io分布

<!-- more -->

# daplink jlink stlink diy硬件兼容性分析

| interface  | daplink        | jlink ob                      | stlink         |
| ---------- | -------------- | ----------------------------- | -------------- |
| SWDIO/JTMS | PB14-100R-PB12 | PA7(PA4-R-PA7)                | PB14-100R-PB12 |
| SWCLK/JTCK | PB13-PA5       | PA5(PA3-R-PA5-PB13)           | PB13-PA5       |
| JTDI       | NS             | PA2(PB0-R-PB14)               | PA7            |
| JTDO/SWO   | NS             | PA10(V1.4:PA6)(PA6-PB15-PA10) | PA6            |
| nRST       | PB0            | PA1(PA2-R-PA1)                | PB0            |
| UTX        | PA2            | NS                            | PA2            |
| URX        | PA3            | NS                            | PA3            |
| SWO        | PA10           | PA10                          | PA10           |
| USB D+     | NC             | PA9                           | PA15           |

附标准jtag接口定义

![jtag/swd](../img/generic_jtag_swd_connector.jpg)