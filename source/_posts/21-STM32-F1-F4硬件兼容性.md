---
title: STM32 F1 F4硬件兼容性
date: 2023-11-27 13:20:51
categories:
- 电子工程
- 微控制器
tags:
- STM32F1
- STM32F4
- 封装差别
- 引脚对比
---

STM32 F1 F4 100封装差别表格

<!-- more -->


| PIN  | F1          | F4      | 连接 |
| ---- | ----------- | ------- | ---- |
| 20   | VREF-       | VSSA    | GND  |
| 12   | RCC_OSC_IN  | PH0     | 晶振 |
| 13   | RCC_OSC_OUT | PH1     | 晶振 |
| 49   | VSS         | VCAP_1  |      |
| 19   | VSSA        | VDD     |      |
| 73   | NC          | V_CAP_2 |      |

