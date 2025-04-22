# ⏱️ STM32 HAL 任务调度器说明文档

本文件由 **匿名拓空者** 开源飞控魔改为 **STM32 HAL 库** 的轻量级任务调度器模块。该模块基于 **系统滴答定时器（SysTick）** 实现时间调度功能，能够高效地进行任务调度，适用于周期性任务管理（例如：LED 闪烁、传感器数据采集等）。

---

## 🚀 快速使用指南


首先在 `main.c` 中注册你的任务和其周期。示例代码如下：

```c
#include "scheduler.h"
#include "user_tasks.h"

int main(void)
{
    // 初始化 HAL 库和系统时钟
    HAL_Init();
    SystemClock_Config();

    // 初始化调度器
    Scheduler_Init();

    // 死循环中不断执行调度器任务
    while (1)
    {
        Scheduler_Run();  // 调度任务
    }
}

```

## 🧠 调度器原理
该任务调度器基于 SysTick 定时器实现，工作原理如下：

SysTick 每 1ms 触发一次中断，用于更新时间 tick_count。

Scheduler_Run() 函数会周期性地轮询任务表，检查任务是否到达执行周期。

如果当前时间超过任务周期，调度器会调用对应的任务函数。

## 🛠️ 注意事项
SysTick 已被 HAL 库初始化为每 1ms 中断一次，因此你无需手动设置 SysTick。

Scheduler_Run() 必须在主循环中不断调用，用来触发任务调度。

避免 在任务函数中使用阻塞代码（例如：HAL_Delay），以确保调度器能够及时调度其他任务。

如果你使用 CubeMX，请确保启用了 SysTick Timer。
## 🌟 结束语
这是一个轻量级、易于扩展的任务调度器，适用于小型嵌入式系统。你可以在此基础上根据项目需求进一步优化，支持更多的调度策略
