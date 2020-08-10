# CMSIS： arm cortex 微控制器接口标准
cortex：ARM公司在经典处理器ARM11以后的产品改用Cortex命名，并分成A、R和M三类，旨在为各种不同的市场提供服务。
([cmsis-rtos v2 官网](https://baike.baidu.com/item/CMSIS))
<ul>
<li><a href="#cmsis-arm-cortex-%E5%BE%AE%E6%8E%A7%E5%88%B6%E5%99%A8%E6%8E%A5%E5%8F%A3%E6%A0%87%E5%87%86">CMSIS： arm cortex 微控制器接口标准</a>
<ul>
<li><a href="#cmsis%E8%BD%AF%E4%BB%B6%E5%B1%82%E6%AC%A1">CMSIS软件层次</a></li>
<li><a href="#cmsis-rtos-api-structure">CMSIS-RTOS API Structure</a></li>
<li><a href="#coding-rules">coding rules</a></li>
<li><a href="#referencehttpsarm-softwaregithubiocmsis_5rtos2htmlmoduleshtml">reference</a>
<ul>
<li><a href="#%E5%86%85%E5%AD%98%E7%AE%A1%E7%90%86-memory-management">内存管理 Memory Management</a></li>
<li><a href="#%E5%86%85%E6%A0%B8%E4%BF%A1%E6%81%AF%E4%B8%8E%E7%AE%A1%E7%90%86-kernel-information-and-control">内核信息与管理 Kernel Information and Control</a>
<ul>
<li><a href="#data-structure">data structure</a></li>
<li><a href="#eum-%E5%86%85%E6%A0%B8%E7%8A%B6%E6%80%81">eum 内核状态</a></li>
<li><a href="#function">function</a></li>
</ul>
</li>
<li><a href="#%E7%BA%BF%E7%A8%8B%E7%AE%A1%E7%90%86-thread-managementhttpsarm-softwaregithubiocmsis_5rtos2htmlgroup__cmsis__rtos__threadmgmthtmldetails">线程管理 Thread Management</a>
<ul>
<li><a href="#%E7%BA%BF%E7%A8%8B%E7%8A%B6%E6%80%81">线程状态</a></li>
<li><a href="#%E7%BA%BF%E7%A8%8B%E8%BD%AC%E5%8F%B0%E7%8A%B6%E6%80%81%E5%8F%98%E8%BF%81">线程转台状态变迁</a></li>
<li><a href="#%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84">数据结构</a>
<ul>
<li><a href="#osthreadattr_t">osThreadAttr_t</a></li>
<li><a href="#osthreadid_t">osThreadId_t</a></li>
</ul>
</li>
</ul>
</li>
<li><a href="#thread-flags">Thread Flags</a></li>
<li><a href="#event-flags">Event Flags</a>
<ul>
<li><a href="#%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86">工作原理</a></li>
<li><a href="#struct-oseventflagsattr_t">struct osEventFlagsAttr_t</a></li>
<li><a href="#function-1">function</a></li>
</ul>
</li>
<li><a href="#generic-wait-functions">Generic Wait Functions</a></li>
<li><a href="#timer-management">Timer Management</a>
<ul>
<li><a href="#%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%A8%8B">工作流程</a></li>
<li><a href="#%E8%AE%A1%E6%97%B6%E5%99%A8%E7%9A%84%E7%BB%93%E6%9E%84-ostimerattr_t">计时器的结构 osTimerAttr_t</a></li>
<li><a href="#function-2">function</a></li>
</ul>
</li>
<li><a href="#mutex-management">Mutex Management</a>
<ul>
<li><a href="#%E7%BB%93%E6%9E%84%E5%AE%9A%E4%B9%89">结构定义</a></li>
<li><a href="#api">API</a></li>
</ul>
</li>
<li><a href="#semaphores">Semaphores</a>
<ul>
<li><a href="#%E7%BB%93%E6%9E%84%E5%AE%9A%E4%B9%89-1">结构定义</a></li>
<li><a href="#api-1">API</a></li>
</ul>
</li>
</ul>
</li>
</ul>
</li>
</ul>

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
- 所有的osXXXXID_t都是void* 的别名
### [reference](https://arm-software.github.io/CMSIS_5/RTOS2/html/modules.html)
(不考虑RTX系列芯片)
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
**osKernelInitialize**需要第一个执行(除了osKernelGetInfo,osKernelGetState)

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
线程flag比事件flag更加细化,只能够传送给某个特定的线程,而非全局使用
- osThreadFlagsSet
```c
uint32_t 	osThreadFlagsSet (osThreadId_t thread_id, uint32_t flags)
```
Set之后会唤醒在队列中优先级最高的线程观察是否符合唤醒条件,如果有,默认clear掉所符合位的1,但是返回值仍然是没有clear之前的值,实际上的flag已经变化了

![两个线程的同步过程](https://arm-software.github.io/CMSIS_5/RTOS2/html/msc_inline_mscgraph_2.png)
- osThreadFlagsClear
```c
uint32_t 	osThreadFlagsClear (uint32_t flags)
```
返回值是clear之前的flag

- osThreadFlagsGet
```c
uint32_t 	osThreadFlagsGet (void)
```
- osThreadFlagsWait

```c
uint32_t 	osThreadFlagsWait
                             (
                            uint32_t flags,
                            uint32_t options, 
                            uint32_t timeout
                            );
)	
```

Option	||
|--|--|
osFlagsWaitAny|Wait for any flag (default).|
osFlagsWaitAll|Wait for all flags.|
osFlagsNoClear|Do not clear flags which have been specified to wait for.
 

#### Event Flags	
##### 工作原理
进程间的同步.A等待B的相关信号,则A应该调用eventWait函数,等待对应ID的信号;B循环执行delay函数与相应的信号Set函数
![同步工作流程图](https://www.keil.com/pack/doc/CMSIS/RTOS2/html/simple_signal.png)
##### struct osEventFlagsAttr_t

||||
|--|--|--|
const char *|	name|	displayed during debugging|
uint32_t|	attr_bits	|Reserved for future use默认需要设为全0
|void *	|cb_mem	|NULL 使用动态内存|
uint32_t|	cb_size|	字节计数.默认为0，配合上个属性|

##### function
用于同步线程
- osEventFlagsNew
```c
osEventFlagsId_t 	osEventFlagsNew (const osEventFlagsAttr_t *attr)
```
返回flagID

- osEventFlagsSet
```c
uint32_t 	osEventFlagsSet (osEventFlagsId_t ef_id, uint32_t flags)
```
	ef_id	 由osEventFlagsNew返回
由被等待线程设置,会返回错误码或者是设置成功的32位flag.
函数执行后所有在等待这个FLAG的函数都会

- osEventFlagsClear
```c
uint32_t 	osEventFlagsClear (osEventFlagsId_t ef_id, uint32_t flags)
```
- osEventFlagsGet
```c
uint32_t 	osEventFlagsGet (osEventFlagsId_t ef_id)
```
- osEventFlagsWait
```c
uint32_t 	osEventFlagsWait (osEventFlagsId_t ef_id, uint32_t flags, uint32_t options, uint32_t timeout)
```
**options**	specifies flags options (osFlagsXxxx).
- osFlagsWaitAny	Wait for any flag (default).
- osFlagsWaitAll	Wait for all flags.
- osFlagsNoClear	Do not clear flags which have been specified to wait for.
**timeout**	Timeout Value or 0 in case of no time-out.
- osEventFlagsDelete
```c
osStatus_t 	osEventFlagsDelete (osEventFlagsId_t ef_id)
```
- osEventFlagsGetName
```c
const char * 	osEventFlagsGetName (osEventFlagsId_t ef_id)
```

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
##### 结构定义
|osMutexAttr_t|||
|--|--|--|
const char *|	name|默认NULL;	for debugging|
uint32_t|	attr_bits	|默认0  ; osMutexRecursive 该互斥锁可以被(自身递归)使用几次  ;   osMutexPrioInherit 继承优先级     ;   osMutexRobust 主线程终结后锁会自动释放;    三位可以用 \| 设置
|void *	|cb_mem	|NULL 使用动态内存|
uint32_t|	cb_size|	字节计数.默认为0，配合上个属性|

- osMutexPrioInherit
当线程得到互斥锁的时候,会继承比她优先级更高的请求该互斥锁的线程的优先级,生命周期到互斥锁被释放为止.
-  osMutexRobust
 主线程终结后互斥锁会自动释放 防止没有被释放的互斥锁成为死锁
```C

#include "cmsis_os2.h"
 
osMutexId_t mutex_id;  
 
const osMutexAttr_t Thread_Mutex_attr = {
  "myThreadMutex",     // human readable mutex name
  osMutexPrioInherit,  // attr_bits
  NULL,                // memory for control block   
  0U                   // size for control block
};
 
void HighPrioThread(void *argument) {
  osDelay(1000U); // wait 1s until start actual work
  while(1) {
    osMutexAcquire(mutex_id, osWaitForever); // try to acquire mutex
    // do stuff
    osMutexRelease(mutex_id);
  }
}
 
void MidPrioThread(void *argument) {
  osDelay(1000U); // wait 1s until start actual work
  while(1) {
    // do non blocking stuff
  }
}
 
void LowPrioThread(void *argument) {
  while(1) {
    osMutexAcquire(mutex_id, osWaitForever);
    osDelay(5000U); // block mutex for 5s
    osMutexRelease(mutex_id);
    osDelay(5000U); // sleep for 5s
  }
}

```
##### API	
利用互斥锁的进程同步
- osMutexNew	创建一个互斥锁
```c
osMutexId osMutexNew	(	const osMutexAttr_t * 	attr	)	;
```
- osMutexAcquire	请求一个互斥锁
```c
osStatus_t osMutexAcquire(osMutexId_t mutexID, UINT32 timeout);
```
请求失败的线程会进入**blocked**状态

- osMutexRelease	释放一个互斥锁
```c
osStatus_t osMutexRelease(osMutexId_t mutexID)
```
正在请求该互斥锁的线程(blocked)会进入**ready**状态

- osMutexGetOwner	释放互斥锁
```c
osThreadId_t osMutexGetOwner(osMutexId_t mutexID);
```

- osMutexDelete	删除互斥锁
```c
osStatus_t osMutexDelete(osMutexId_t mutexID)
```
不能再被使用,需要重新osMutexNew才能使用
#### Semaphores	
线程间同时访问共享资源
##### 结构定义
|struct osSemaphoreAttr_t|||
|--|--|--|
const char *|	name|	displayed during debugging|
uint32_t|	attr_bits	|必须初始化为0
|void *	|cb_mem	|NULL 使用动态内存|
uint32_t|	cb_size|	字节计数.默认为0，配合上个属性|

##### API	


利用互斥锁的进程同步
- osSemaphoreNew	创建一个信号量
```c
osSemaphoreId_t osSemaphoreNew	(	uint32_t 	max_count,
uint32_t 	initial_count,
const osSemaphoreAttr_t * 	attr 
)	;
```
设置信号量的计数最大值(资源可以用的总数)与计数的初始值

- osSemaphoreAcquire	等待信号量
```c
const char * osSemaphoreGetName	(	osSemaphoreId_t 	semaphore_id	)	;

```

- osSemaphoreRelease	释放信号量
```c
osStatus_t osSemaphoreRelease	(	osSemaphoreId_t 	semaphore_id	)	
)	;
```

- osSemaphoreGetCount	删除信号量
```c
uint32_t osSemaphoreGetCount	(	osSemaphoreId_t 	semaphore_id	)	

```

- osSemaphoreDelete	删除信号量
```c
osStatus_t osSemaphoreDelete	(	osSemaphoreId_t 	semaphore_id	)	;
```
