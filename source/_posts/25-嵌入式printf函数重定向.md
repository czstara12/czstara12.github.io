---
title: 嵌入式printf函数重定向
date: 2023-11-30 12:57:06
categories:
- 嵌入式开发
tags:
- printf重定向
- HAL库
- 串口通信
- 编译器兼容性
---

printf重定向

<!-- more -->

# printf重定向函数

```
int _write(int file, char *ptr, int len)
{
    HAL_UART_Transmit(&huart1, (uint8_t *)ptr, len, 1000);
    return len;
}//gcc系列编译器重定向
int fputc(int c, FILE *f) {

    HAL_UART_Transmit(&huart1, (uint8_t *)&c, 1, 10);
        return 1;
}//MDK系列编译器重定向
```

一个不错的[博客](https://www.strongerhuang.com/)讲printf重定向其他的玩法
