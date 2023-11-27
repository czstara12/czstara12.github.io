---
title: DIY调试器硬件兼容性分析
date: 2023-11-27 13:33:56
categories:
- 嵌入式硬件
tags:
- daplink
- jlink
- stlink
---

分析三个调试器在STM32F103CxT6上的io

<!-- more -->

# daplink jlink stlink硬件兼容性分析

| interface  | daplink        | jlink ob                      | stlink         | Black Magic Probe |
| ---------- | -------------- | ----------------------------- | -------------- | ----------------- |
| SWDIO/JTMS | PB14-100R-PB12 | PA7(PA4-R-PA7)                | PB14-100R-PB12 | (DIR:PA1)PA4      |
| SWCLK/JTCK | PB13-PA5       | PA5(PA3-R-PA5-PB13)           | PB13-PA5       | PA5               |
| JTDI       | NS             | PA2(PB0-R-PB14)               | PA7            | PA3               |
| JTDO/SWO   | NS             | PA10(V1.4:PA6)(PA6-PB15-PA10) | PA6            | PA6               |
| nRST       | PB0            | PA1(PA2-R-PA1)                | PB0            | PA2(IN:PA7)       |
| UTX        | PA2            | NS                            | PA2            | PA9               |
| URX        | PA3            | NS                            | PA3            | PA10              |
| SWO        | PA10           | PA10                          | PA10           | PA6               |
| USB D+     | NC             | PA9                           | PA15           | PA8               |

附标准jtag接口定义

![jtag/swd](https://docs.platformio.org/en/latest/_images/generic_jtag_swd_connector.jpg)