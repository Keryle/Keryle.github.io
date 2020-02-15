---
layout: default
title: uCOSII
description: os_core.c内核源码阅读
---

# os_core.c内头文件ucos_ii.h

```
#ifndef   OS_uCOS_II_H
#define   OS_uCOS_II_H

#include <app_cfg.h>  //用户配置的文件
#include <os_cfg.h>   //系统配置文件
#include <os_cpu.h>   //定义数据类型，中断管理方式等
```

## app_cfg.h

该文件定义了程序中具体任务的数量，优先级，堆栈大小等。

## os_cfg.h

该文件定义了n多系统常数。

## os_cpu.h

* 定义数据类型：
  ```
  typedef unsigned char  BOOLEAN;
  typedef unsigned char  INT8U;                    
  typedef signed   char  INT8S;    
  ······
  ```
* 控制中断的方式（方式三）
  通过保存中断的状态，打开或关闭中断。
  ```
  #if OS_CRITICAL_METHOD == 3
  #define  OS_ENTER_CRITICAL()  {cpu_sr = OS_CPU_SR_Save();}
  #define  OS_EXIT_CRITICAL()   {OS_CPU_SR_Restore(cpu_sr);}
  #endif
  ```
* 申明函数
  ```
  /* 汇编语言编写的函数 */
  OS_CPU_SR  OS_CPU_SR_Save(void);                //读取中断状态并关闭中断
  void       OS_CPU_SR_Restore(OS_CPU_SR cpu_sr); //返回中断状态并打开中断

  void       OSCtxSw(void);       //任务交换
  void       OSIntCtxSw(void);    //在中断中切换任务
  void       OSStartHighRdy(void);//开始最高优先级的任务

  /*PendSV在Cortex-M3中可设置为最低优先级的异常，下列异常处理程序可在没有中断发生的时候切换任务*/
  void       OS_CPU_PendSVHandler(void);

  /* See OS_CPU_C.C                                                    
  void       OS_CPU_SysTickHandler(void);
  void       OS_CPU_SysTickInit(void);

  /* See BSP.C                                                    
  INT32U     OS_CPU_SysTickClkFreq(void);
  ```

## 其他

其他宏定义过多，c函数具体使用时详细分析。

# os_core.code

```
#ifndef  OS_MASTER_FILE
#define  OS_GLOBALS
#include <ucos_ii.h>
#endif

/*
*********************************************************************************************************
*                                       优先级解码表
*
* Note: 输入参数查表，返回最高优先级位。(i.e. 0..7)
*********************************************************************************************************
*/

INT8U  const  OSUnMapTbl[256] = {
    0, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,       /* 0x00 to 0x0F                             */
    4, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,       /* 0x10 to 0x1F                             */
    5, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,       /* 0x20 to 0x2F                             */
    4, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,       /* 0x30 to 0x3F                             */
    6, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,       /* 0x40 to 0x4F                             */
    4, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,       /* 0x50 to 0x5F                             */
    5, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,       /* 0x60 to 0x6F                             */
    4, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,       /* 0x70 to 0x7F                             */
    7, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,       /* 0x80 to 0x8F                             */
    4, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,       /* 0x90 to 0x9F                             */
    5, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,       /* 0xA0 to 0xAF                             */
    4, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,       /* 0xB0 to 0xBF                             */
    6, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,       /* 0xC0 to 0xCF                             */
    4, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,       /* 0xD0 to 0xDF                             */
    5, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0,       /* 0xE0 to 0xEF                             */
    4, 0, 1, 0, 2, 0, 1, 0, 3, 0, 1, 0, 2, 0, 1, 0        /* 0xF0 to 0xFF                             */
};

*********************************************************************************************************
*                                       FUNCTION PROTOTYPES
*********************************************************************************************************
*/

static  void  OS_InitEventList(void);

static  void  OS_InitMisc(void);

static  void  OS_InitRdyList(void);

static  void  OS_InitTaskIdle(void);

#if OS_TASK_STAT_EN > 0
static  void  OS_InitTaskStat(void);
#endif

static  void  OS_InitTCBList(void);

static  void  OS_SchedNew(void);


```
