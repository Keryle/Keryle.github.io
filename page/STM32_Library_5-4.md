---
layout: default
title: STM32固件库
description: 结构变量，函数配置
---

STM32F10x官方固件库[下载](https://www.st.com/content/st_com/en/products/embedded-software/mcu-mpu-embedded-software/stm32-embedded-software/stm32-standard-peripheral-libraries/stsw-stm32054.html#overview)（需要注册）。

# 使用说明

## 缩写

| 缩写 | 对应设备/单元 |
|  -  |     -     |
| ADC | Analog/digital converter |
| BKP | Backup registers|
| CAN | Controller area network|
| CEC | Consumer Electronics Control |
| CRC | CRC calculation unit |
| DAC | Digital to analog converter|
| DBGMCU | Debug MCU |
| DMA | DMA controller |
| EXTI |  External interrupt/event controller |
| FSMC | Flexible static memory controller |
| FLASH | Flash memory |
| GPIO | General purpose I/O |
| I2C | Inter-integrated circuit |
| I2S | nter-integrated sound |
| IWDG | Independent watchdog |
| NVIC | Nested vectored interrupt controller |
| PWR | Power controller |
| RCC | Reset and clock controller |
| RTC | Reset and clock controller |
| SDIO | SDIO interface |
| SPI | Serial peripheral interface |
| SysTick | System tick timer
| TIM | Advanced-control,timer |
| USART | Universal synchronous asynchronous receiver transmitter |
| WWDG | Window watchdog |

## 命名规则

STM32F10x标准固件库使用如下命名规则：

* PPP为任意缩写，例如ADC。
* .c和.h文件由stm32f10x_开头。
* 所有常量大写。如需引用，在.h文件中申明。
* 函数名称由外设缩写+下划线+单词首字母大写，如USART_SendData。
* 初始化外设PPP的函数名称：PPP_Init。其参数定义在PPP_InitTypeDef。
* 重置外设PPP的函数名称：PPP_DeInit。
* 使结构变量PPP_InitTypeDef中的参数重置的函数：PPP_StructInit。
* 启动或停止外设的函数：PPP_Cmd。
* 启动或停止外设中断的函数：PPP_ITConfig。
* 启动或停止外设DMA的函数：PPP_DMAConfig。
* 配置外设的函数以‘Config’结束，例如：GPIO_PinRemapConfig。
* 检查外设的flag置位或重置的函数：PPP_GetFlagStatus。
* 清除外设flag的函数：PPP_ClearFlag。
* 检查外设中断发生的函数：PPP_GetITStatus。
* 清除外设中断等待位的函数：PPP_ClearITPendingBit。

## 外设寄存器

每一个外设都有与之对应的结构指针，通过配置结构指针来配置相应的寄存器。

### 外设结构数组

例如SPI的结构数组如下：
```
/**
  * @brief Serial Peripheral Interface
  */

typedef struct
{
  __IO uint16_t CR1;
  uint16_t  RESERVED0;
  __IO uint16_t CR2;
  uint16_t  RESERVED1;
  __IO uint16_t SR;
  uint16_t  RESERVED2;
  __IO uint16_t DR;
  uint16_t  RESERVED3;
  __IO uint16_t CRCPR;
  uint16_t  RESERVED4;
  __IO uint16_t RXCRCR;
  uint16_t  RESERVED5;
  __IO uint16_t TXCRCR;
  uint16_t  RESERVED6;
  __IO uint16_t I2SCFGR;
  uint16_t  RESERVED7;
  __IO uint16_t I2SPR;
  uint16_t  RESERVED8;  
} SPI_TypeDef;
```

## Bit-Banding 位带

Cortex-M3内核包含两个位带区域。位带区域中每位有一个别名来进行位寻址（参考51单片机的位寻址区域）。

### Mapping formula

bit_word_offset = (byte_offset x 32) + (bit_number + 4)
bit_word_addr = bit_band_base + bit_word_offset

* bit_word_offset是位寻址位置
* bit_word_addr别名区域的地址联系到目标位。
* bit_band_base别名区域的开始地址。
* byte_offset位带中目标位btye数字
* bit_number 目标位 位置（0-7）

## 配置启动文件

查看stm32f10x.h ( Libraries\ CMSIS\ CM3\ DeviceSupport\ ST\ STM32F10x), 文件，如下：
```
  /* #define STM32F10X_LD */     /*!< STM32F10X_LD: STM32 Low density devices */
  /* #define STM32F10X_LD_VL */  /*!< STM32F10X_LD_VL: STM32 Low density Value Line devices */  
  /* #define STM32F10X_MD*/      /*!< STM32F10X_MD: STM32 Medium density devices */
  /* #define STM32F10X_MD_VL */  /*!< STM32F10X_MD_VL: STM32 Medium density Value Line devices */  
  /* #define STM32F10X_HD */     /*!< STM32F10X_HD: STM32 High density devices */
  /* #define STM32F10X_HD_VL */  /*!< STM32F10X_HD_VL: STM32 High density value line devices */  
  /* #define STM32F10X_XL */     /*!< STM32F10X_XL: STM32 XL-density devices */
  /* #define STM32F10X_CL */     /*!< STM32F10X_CL: STM32 Connectivity line devices */

/*  Tip: To avoid modifying this file each time you need to switch between these
        devices, you can define the device in your toolchain compiler preprocessor.

 - Low-density devices are STM32F101xx, STM32F102xx and STM32F103xx microcontrollers
   where the Flash memory density ranges between 16 and 32 Kbytes.
 - Low-density value line devices are STM32F100xx microcontrollers where the Flash
   memory density ranges between 16 and 32 Kbytes.
 - Medium-density devices are STM32F101xx, STM32F102xx and STM32F103xx microcontrollers
   where the Flash memory density ranges between 64 and 128 Kbytes.
 - Medium-density value line devices are STM32F100xx microcontrollers where the
   Flash memory density ranges between 64 and 128 Kbytes.   
 - High-density devices are STM32F101xx and STM32F103xx microcontrollers where
   the Flash memory density ranges between 256 and 512 Kbytes.
 - High-density value line devices are STM32F100xx microcontrollers where the
   Flash memory density ranges between 256 and 512 Kbytes.   
 - XL-density devices are STM32F101xx and STM32F103xx microcontrollers where
   the Flash memory density ranges between 512 and 1024 Kbytes.
 - Connectivity line devices are STM32F105xx and STM32F107xx microcontrollers.
  */
```
根据上文中的tips选择启动文件。如flash为64KB是Medium-density devices，选择startup_STM32F10X_MD.s启动文件。
再添加stm32f10x.h，system_stm32f10x.c文件。

## 外围设备初始化和配置

1. 申明结构变量如：PPP_InitTypeDef PPP_InitStructure；

2. 给结构变量配置参数，有两种方法：

  * 第一种直接配置如下：
    >PPP_InitStructure.member1 = val1;
    >PPP_InitStructure.member2 = val2;
    >PPP_InitStructure.memberN = valN; // where N is the number of the structure members
    >或者在一行中:
    >PPP_InitTypeDef PPP_InitStructure = { val1, val2,.., valN}

  * 第二种选择性配置，选择某一些参数配置，其他保持默认值。
    >PPP_StructInit(&PPP_InitStructure);
    >PPP_InitStructure.memberX = valX;
    >PPP_InitStructure.memberY = valY;

3. 调用函数初始化外设。

>PPP_Init(PPP, &PPP_InitStructure);

4. 使能外设。

>PPP_Cmd(PPP, ENABLE);

**Note:**

1. 配置外设前，调用函数启动时钟：

>RCC_AHBPeriphClockCmd(RCC_AHBPeriph_PPPx, ENABLE);
>RCC_APB2PeriphClockCmd(RCC_APB2Periph_PPPx, ENABLE);
>RCC_APB1PeriphClockCmd(RCC_APB1Periph_PPPx, ENABLE);

2. 设置寄存器默认值:

>PPP_DeInit(PPP);

3. 在配置外设后修改设置：

>PPP_InitStucture.memberX = valX;
>PPP_InitStructure.memberY = valY; /* where X and Y are the
only members that user wants to modify*/
PPP_Init(PPP, &PPP_InitStructure);
