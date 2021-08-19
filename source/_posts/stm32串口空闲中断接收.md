---
title: stm32串口空闲中断接收
date: 2021-03-10 15:21:13
categories:
- c/c++
tags:
- stm32
- uart
- dma
---

# STM32 HAL库 串口空闲中断

<!-- more -->

开启

```c
__HAL_UART_ENABLE_IT(&huartX, UART_IT_IDLE);
```

这时候启动传输

```c
HAL_UART_Receive_DMA(&huartx, addr, len);
```

在中断函数中

```c
	if(USART3->SR&0x10)//如果是空闲中断
	{
		__HAL_UART_CLEAR_IDLEFLAG(&huartX);//如果检测到空闲中断无论如何都要清除空闲中断标志 切记
		if(addr[0]!=0x5a)//验证包头 或者其他验证数据有效性方法
		{
			HAL_UART_AbortReceive(&huart3);//终止传输
			HAL_UART_Receive_DMA(&huartX, addr, 82);//重启传输
			//while(USART3->SR&0x10);//
		}
	}
```

## 不定长DMA接收

```c
	if(USART1->SR&0x10)//如果是空闲中断
	{
		__HAL_UART_CLEAR_IDLEFLAG(&huart1);//清除空闲中断标志
		if(rxdata[0]==0x55)//验证包头 或者其他验证数据有效性方法
		{
			HAL_UART_RxCpltCallback(&huart1);
		}
		HAL_UART_AbortReceive(&huart1);//终止传输
		HAL_UART_Receive_DMA(&huart1, rxdata, 50);//重启传输
	}
```

与定长不同的地方是无论验证包数据是否成功 都进行重置 当验证成功后处理包数据

也可

```
	if(USART3->SR&0x10)//如果是空闲中断
	{
		__HAL_UART_CLEAR_IDLEFLAG(&huartX);//清除空闲中断标志
		if(addr[0]!=0x5a)//验证包头 或者其他验证数据有效性方法
		{
			HAL_UART_AbortReceive(&huart3);//终止传输
			HAL_UART_Receive_DMA(&huartX, addr, 82);//重启传输
		}
		if(addr[len-DMA剩余长度-1]==0x55)//验证包尾 或者其他验证数据完整性方法
		{
			HAL_UART_AbortReceive(&huart3);//终止传输
			HAL_UART_Receive_DMA(&huartX, addr, 82);//重启传输
			fun()//处理数据代码
		}
		//好处是 如果包头验证成功 而没有检测到包尾 就跳过本次空闲中断 以接收完整数据 不过在包内加上字长节防止数据错误
	}
```

