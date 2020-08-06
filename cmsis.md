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
要求标识模块名
- os开头：实时系统函数变量
- os_开头：在应用中不使用但是在本头文件中使用（？）

- 后缀_t说明当前变量是一个别名(typedef)
### [reference](https://arm-software.github.io/CMSIS_5/RTOS2/html/modules.html)
(不考虑RTX系列芯片)
定义在[pcmsis_os2.h](https://arm-software.github.io/CMSIS_5/RTOS2/html/cmsis__os2_8h.html)中
#### 
#### 内存管理 Memory Management

#### 内核信息与管理 Kernel Information and Control	
#### [线程管理 Thread Management](https://arm-software.github.io/CMSIS_5/RTOS2/html/group__CMSIS__RTOS__ThreadMgmt.html#details) 	
##### 线程状态
![线程状态](https://arm-software.github.io/CMSIS_5/RTOS2/html/ThreadStatus.png)
- **RUNNING**: 正在运行的县城只能有一个
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
- osThreadTerminate
可以在任何时间终结活跃进程成为inactive进程并且不占用任何动态内存资源
(以下函数不能被中断服务程序调用)
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
###### osThreadAttr_t
#### Thread Flags	Synchronize threads using flags
#### Event Flags	Synchronize threads using event flags
#### Generic Wait Functions	Wait for a certain period of time
#### Timer Management	Create and control timer and timer callback functions
#### Mutex Management	Synchronize resource access using Mutual Exclusion (Mutex)
#### Semaphores	Access shared resources simultaneously from different threads
