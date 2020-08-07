# CMSIS： arm cortex 微控制器接口标准
cortex：ARM公司在经典处理器ARM11以后的产品改用Cortex命名，并分成A、R和M三类，旨在为各种不同的市场提供服务。

## [结构](https://baike.baidu.com/item/CMSIS)
### CMSIS软件层次
CMSIS可以分为多个软件层次，分别由ARM公司、芯片供应商提供。  
其中ARM提供了下列部分，可用于多种编译器：  
- **内核设备访问层**：包含了用来访问内核的寄存器设备的名称定义，地址定义和助手函数。同时也为RTOS(实时操作系统)定义了独立于微控制器的接口，该接口包括调试通道定义。  
- **中间设备访问层**：为软件提供了访问外设的通用方法。芯片供应商应当修改中间设备访问层，以适应中间设备组件用到的微控制器上的外设。目前中间设备访问层仍处于开发过程中，本文不做详述。 

芯片供应商扩展下列软件层：  
- **微控制器外设访问层**：提供片上所有外设的定义。  
- **外设的访问函数(可选)**：为外设提供额外的助手函数。

CMSIS为Cortex-Mx微控制器系统定义了：  
- **访问外设寄存器**的通用方法和**定义异常向量**的通用方法。  
- **内核**设备的**寄存器名称**和**内核异常向量**的名称。  
- 独立于微控制器的RTOS接口，带**调试通道**。  
- **中间设备组件**接口(TCP/IP协议栈，闪存文件系统)。  
### CMSIS-RTOS API Structure
- 函數名，变量，参数
- 线程控制
- 终端控制程序
- 三中线程通信方式：信号量 消息 邮件
- 互斥锁和信号量被合并
- cpu时间的统一

### coding rules
命名规则
- os开头：实时系统函数变量
- os_开头：在应用中不使用但是在本头文件中使用（？）
- **后缀_t**说明当前变量是一个别名(typedef)

### [reference](https://arm-software.github.io/CMSIS_5/RTOS2/html/modules.html)
（官网基于rtx操作系统）
定义在[pcmsis_os2.h](https://arm-software.github.io/CMSIS_5/RTOS2/html/cmsis__os2_8h.html)中

#### 内存管理 Memory Management

#### 内核信息与管理 Kernel Information and Control	
##### data structure
struct oaVersion_t
##### eum 内核状态
``` c 
enum  	osKernelState_t {
  osKernelInactive = 0,
  osKernelReady = 1,
  osKernelRunning = 2,
  osKernelLocked = 3,
  osKernelSuspended = 4,
  osKernelError = -1,
  osKernelReserved = 0x7FFFFFFFU
}
```
##### function
``` c 

osStatus_t osKernelInitialize(void);
//初始化实时操作系统内核，在此函数成功执行之前只能执行 osKernelGetInfo 和 osKernelGetState

osStatus_t osKernelGetInfo(osVersion_t *version, char *idBuf, UINT32 idSize);
//获得当前内核信息
osKernelState_t osKernelGetState(void);
//osKernelState_t 是 osStatus_t结构的其中之一，描述当前内核状态（）

osStatus_t osKernelStart(uint32_t cpuType);
//启动内核
（liteos中重新定义了函数，有些返回值设定为64位，这里引用的是官方定义接口）
uint32_t  osKernelLock(void);

uint32_t  osKernelUnlock(void);

uint32_t  osKernelRestoreLock(uint32_t  lock);
//只加一层锁的时候不需要restore
//能够保存之前每一次的锁状态 0,1,err

/*
*关于tick
*系统时钟是由定时/计数器产生的输出脉冲触发中断而产生的，通常定义为整数或长整数。
*输出脉冲的周期叫作一个“时钟滴答”。系统时钟也称为时标或者Tick。一个Tick的时长能够静态配置。
*tick是时间长度单位。tick设定的值是一个程序内的最小时间单位。
*程序的tick由开发人员设定，设置为n个计算机时钟周期的长度。
*计算机时钟周期由cpu主频决定。设置为n>=1个时钟周期的长度。一个tick内一般可以有多个时钟周期
*可以通过设定每秒钟的tick数量配置tick的长度(不能比主频大)
*/
uint32_t  osKernelGetTickCount(void);
//获得当前程序tick计数次数
//可以用isr调用该函数


uint32_t osKernelGetSysTimerFreq(void)；
//获得当前的计时器频率，当kernel不活跃时返回0

```
[时间管理](https://www.shangmayuan.com/a/e4e2bb1ba4f94193ad6342ce.html)

#### [线程管理 Thread Management](https://arm-software.github.io/CMSIS_5/RTOS2/html/group__CMSIS__RTOS__ThreadMgmt.html#details) 	
##### 线程状态
![线程状态](https://arm-software.github.io/CMSIS_5/RTOS2/html/ThreadStatus.png)
- **RUNNING**: 正在运行的线程只能有一个
- **READY**: 线程得到了所有资源,只等待CPU.一旦当前running进程被block或者terminate,就绪队列中的优先级最高进程就会转为running状态
- **BLOCKED**: 包括延迟\等待事件\或者阻塞 (**suspended** )
- **TERMINATED**: 调用接口**osThreadTerminate** 释放相关线程的所有资源
- **INACTIVE**: 包括terminated的进程和没有被创建的线程
##### 线程转台状态变迁
-  osThreadNew
``` c 
//原型

osThreadId_t
osThreadNew	( osThreadFunc_t func,//线程执行的函数
              void * 	argument,//线程回调函数参数
              const osThreadAttr_t *attr //线程属性  默认为NULL
)	
//返回线程ID
```
创建进程使其初始状态为running或ready
-  osThreadSetPriority
默认线程优先级是[osPriorityNormal](https://arm-software.github.io/CMSIS_5/RTOS2/html/cmsis__os2_8h.html#gad4e3e0971b41f2d17584a8c6837342eca45a2895ad30c79fb97de18cac7cc19f1)
该函数可以改变优先级.
由于cmsis实时os是一个抢占式(preemptive)操作系统,因此需要规定线程优先级(osPriority_t).优先级是
 [osThreadAttr_t](https://arm-software.github.io/CMSIS_5/RTOS2/html/group__CMSIS__RTOS__ThreadMgmt.html#structosThreadAttr__t)中的一个属性


(以下函数不能被中断服务程序调用)
- osThreadGetId
```c

osThreadId_t osThreadGetId(void)
```
获得线程ID
- osThreadGetState
```c

osThreadState_t osThreadGetState(osThreadId_t threadID)
```
获得线程状态

- osThreadGetStackSize
```c

uint32_t osThreadGetStackSize(osThreadId_t threadID)
```
获得线程控制快信息中分配的堆栈大小

- osThreadSetPriority
```c

osStatus_t osThreadSetPriority(osThreadId_t threadID, osPriority_t priority)；
```
设置进程优先级

- osThreadGetPriority
```c
osPriority_t osThreadGetPriority(osThreadId_t threadID)；
```
获得线程优先级

- osThreadTerminate
可以在任何时间终结活跃进程成为inactive进程并且不占用任何动态内存资源
```c
osStatus_t osThreadTerminate(
            osThreadId_t threadID
            )
```

- osThreadYield
```c
osStatus_t osThreadYield	(	void 		)	
	
```
将当前的thread移交给与自己同优先级的下一个ready thread。使其自身变为ready状态

- osThreadSuspend
```c
osStatus_t osThreadSuspend	(	
    osThreadId_t 	thread_id	)	
```
- osThreadResume
```c
osStatus_t osThreadResume	(	
    osThreadId_t 	thread_id	)	
```
- osThreadDetach
```c
//线程状态改为不可join
osStatus_t osThreadDetach	(	
    osThreadId_t 	thread_id	)	
```
- osThreadJoin
```c
osStatus_t osThreadJoin	(	
    osThreadId_t 	thread_id	)	
```
- osThreadGetCount
```c
UINT32 osThreadGetCount(void)
```
返回现在的活跃变两个数

##### 数据结构

###### osThreadAttr_t
||||
|--|--|--|
const char *|	name|	displayed during debugging|
uint32_t|	attr_bits	|osThreadDetached
|void *	|cb_mem	|NULL 使用动态内存|
uint32_t|	cb_size|	字节计数.默认为0|
void *	|stack_mem|	栈空间指针默认为NULL|
uint32_t|	stack_size	|size of stack|
osPriority_t|	priority|	默认: osPriorityNormal.|
TZ_ModuleId_t	|tz_module	|TrustZone module identifier.默认0.不能被其他线程调用
uint32_t|	reserved|	默认0
###### osThreadId_t
类型:void*
获得方式 
- osThreadNew 
- osThreadGetId() 当前running的id


#### Thread Flags	

#### Event Flags	
用于同步线程
#### Generic Wait Functions
会返回kernel状态
- osDelay   相对时间：延迟**ticks**个tick
```c
osStatus_t osDelay(uint32_t ticks);
```
- osDelayUntil    绝对时间：延迟直到系统tick**==**ticks
```c
 osStatus_t osDelayUntil(uint32_t ticks);	
```


#### Timer Management	
定时器的创建、控制与回调函数
##### 工作流程
1. Define the timers:
```c
osTimerId_t one_shot_id, periodic_id;
```
2. Define callback functions:
```c
static void one_shot_Callback (void *argument) {
  int32_t arg = (int32_t)argument; // cast back argument '0' 
  // do something, i.e. set thread/event flags
}
static void periodic_Callback (void *argument) {
  int32_t arg = (int32_t)argument; // cast back argument '5'
  // do something, i.e. set thread/event flags
}
```
3. Instantiate and start the timers:
```c
// creates a one-shot timer:
one_shot_id = osTimerNew(one_shot_Callback, osTimerOnce, (void *)0, NULL);     // (void*)0 是回调函数的参数

// creates a periodic timer:
periodic_id = osTimerNew(periodic_Callback, osTimerPeriodic, (void *)5, NULL); // (void*)5 是回调函数的参数

osTimerStart(one_shot_id, 500U);
osTimerStart(periodic_id, 1500U);
 
// 单次定时器用完一次就自动删除
osTimerStart(one_shot_id, 500U);
 
// when timers are not needed any longer free the resources:
osTimerDelete(one_shot_id);
osTimerDelete(periodic_id);
```
##### 计时器的结构 osTimerAttr_t
||||
|--|--|--|
const char *|	name|	displayed during debugging|
uint32_t|	attr_bits	|Reserved for future use
|void *	|cb_mem	|NULL 使用动态内存|
uint32_t|	cb_size|	字节计数.默认为0，配合上个属性|


##### function
- osTimerNew
```c
osTimerId_t osTimerNew(
        osTimerFunc_t func, \
        osTimerType_t type,\
        void *argument, \
        const osTimerAttr_t *attr);
```
- osTimerStart
```c
//定时器创建之后不会自动启动
osStatus_t osTimerStart(osTimerId_t timerID, uint32_t  ticks);
```
- osTimerGetName  
```c
const char *osTimerGetName(osTimerId_t timerID);

```

- osTimerStop 暂停一个特定的Timer
```c
osStatus_t osTimerStop(osTimerId_t timerID);
```

- osTimerIsRunning
```c
//返回0-not running 1-running
uint32_t  osTimerIsRunning(osTimerId_t timerID);
```
- osTimerDelete 定时器删除
```c

osStatus_t osTimerDelete(osTimerId_t timerID);
```

#### Mutex Management	
利用互斥锁的进程同步
#### Semaphores	
线程间同时访问共享资源

