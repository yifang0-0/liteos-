<body for="html-export">
      <div class="mume markdown-preview  ">
      <h1 class="mume-header" id="liteos">LITEOS</h1>

<ul>
<li><a href="#liteos">LITEOS</a>
<ul>
<li><a href="#%E4%BD%93%E7%B3%BB%E6%9E%B6%E6%9E%84">体系架构</a></li>
<li><a href="#%E5%86%85%E6%A0%B8%E6%9E%B6%E6%9E%84">内核架构</a></li>
<li><a href="#%E5%86%85%E6%A0%B8%E5%8A%9F%E8%83%BD">内核功能</a></li>
<li><a href="#%E6%80%BB%E7%BB%93%E6%96%87%E6%A1%A3">总结文档</a>
<ul>
<li><a href="#task">Task</a></li>
<li><a href="#timer">Timer</a></li>
<li><a href="#mutex">Mutex</a></li>
<li><a href="#semaphore">Semaphore</a></li>
<li><a href="#messagequeue">MessageQueue</a></li>
</ul>
</li>
</ul>
</li>
</ul>
<h2 class="mume-header" id="%E4%BD%93%E7%B3%BB%E6%9E%B6%E6%9E%84">体系架构</h2>

<h2 class="mume-header" id="%E5%86%85%E6%A0%B8%E6%9E%B6%E6%9E%84">内核架构</h2>

<ul>
<li>内存管理</li>
<li>内核调度</li>
<li>进程间通信</li>
<li>调试开发</li>
</ul>
<h2 class="mume-header" id="%E5%86%85%E6%A0%B8%E5%8A%9F%E8%83%BD">内核功能</h2>

<ul>
<li>硬件相关：硬中断、硬件定时器</li>
<li>内存管理：静态动态、内存统计、泄漏检测</li>
<li>内核功能：任务管理、软件定时器</li>
<li>通信：信号量、互斥量、事件、消息队列</li>
<li>调试：内存检测、任务栈检测</li>
</ul>
<h2 class="mume-header" id="%E6%80%BB%E7%BB%93%E6%96%87%E6%A1%A3">总结文档</h2>

<h3 class="mume-header" id="task">Task</h3>

<ul>
<li>LiteOS的任务一共有32个优先级(0-31)，最高优先级为0，最低优先级为31。同一优先级任务使用双向链表管理。</li>
<li>每一个任务都含有一个任务控制块(TCB)。TCB包含了任务上下文SP、任务状态、任务优先级、任务ID、任务名、任务栈大小等信息。</li>
<li>任务切换：在<strong>M3内核</strong>中利用第14号PendSV异常进行任务切换。 与11号SVC异常必须在执行SVC指令后立即得到响应，如果得不到响应，会上访成硬fault不同，PendSV可以像普通的中断一样被悬起的。</li>
<li>
<ul>
<li>就绪（资源分配完成，<strong>仅仅等待CPU</strong>）</li>
<li>运行（正在被CPU执行）</li>
<li>阻塞（任务被挂起、延迟、等待信号量、读写队列、等待读写时间）</li>
</ul>
</li>
<li>任务调度：创建任务、任务挂起、任务恢复、任务延时、解锁任务调度、信号量操作</li>
<li>当前任务为最高优先级任务或锁任务调度时，任务调度会退出，不会进行任务切换</li>
</ul>
<p>功能</p>
<ul>
<li>任务删除 任务创建</li>
<li>任务信息查询 LOS_TaskSelf</li>
</ul>
<h3 class="mume-header" id="timer">Timer</h3>

<p>初始化：完成内存的分配、相关参数的设置、内部逻辑结构的建立、相关数据的初始化和为处理超时处理函数创建0号优先级任务。<br>
为了方便管理软件定时器控制块，在初始化时对定时器控制块进行申请内存空间，以数组的形式访问，从而达到定时器的快速定位，同时为了方便定时器的使用管理，把所有空闲定时器控制块以单向链表组织起来，构成一条空闲链表，当使用时直接从链表头取，回收时直接插入到链表首部。</p>
<p>初始化，创建0号超时处理函数/所有空闲定时器控制块以单向链表链接<br>
定时器模块：计时结构sortlink，全部指向一个单向循环链表</p>
<ul>
<li>
<p>初始化 分四个部分<br>
<strong>1.定时器</strong>本体</p>
<ul>
<li>内存分配</li>
<li>控制块数据初始化</li>
</ul>
<p><strong>2.定时器链表</strong><br>
1.初始化</p>
<ul>
<li>m_pstSwTmrSortList=NULL</li>
</ul>
<p>2.规则</p>
<ul>
<li>定时器按照超时从早到晚排序</li>
<li>每个tick中断对当前链表进行扫描，超时的进行处理</li>
</ul>
<p><strong>3.sortlink</strong>设置</p>
<ul>
<li>Tick当前检验的定时器链表类 初始==0</li>
<li>TimerList*指向<strong>链表数组</strong>首地址，每个sortlink元素指向一个单向循环链表</li>
</ul>
<p><strong>4.定时器超时回调结构内存池与队列</strong></p>
<ul>
<li>m_aucSwTmrHandlerPool 一块8字节 4回调函数指针+4回调函数参数</li>
<li>m_uwSwTmrHandlerQueue 队列长度等于上一个的块数</li>
<li>在tick中扫描到有定时器超时，申请一块固定大小内存块，封装放入队列<br>
<strong>5.定时器超时处理队列与任务</strong></li>
<li>超时回调队列创建完成后<strong>创建定时器超时处理任务</strong></li>
<li><strong>定时器超时处理任务</strong>读取队列执行回调函数</li>
<li>等待时间永远</li>
<li>根据队列内容（回调函数指针与参数）</li>
</ul>
</li>
</ul>
<p>注意：定时器回调函数中不要调用挂起定时器超时处理接任务的接口。因为串行处理，后方定时器超时后会提前调度、等待；或者使得整个队列爆满</p>
<ul>
<li>软件定时器创建（需要用户指定）
<ul>
<li>调用创建接口</li>
<li>设置属性：计时长度、触发模式、超时处理函数、超时处理参数、返回ID</li>
</ul>
</li>
<li>软件定时器启动
<ul>
<li>
<p>调用接口：设置状态为 OS_SWTMR_STATUS_TICKING设定计时开始/重新开始</p>
</li>
<li>
<p>定时器内部逻辑结构设定</p>
</li>
</ul>
</li>
<li>sortlink表头m_pstSwTmrSortList初始化为NULL</li>
<li>每个Tick中断对定时器初始处理</li>
<li></li>
</ul>
<h3 class="mume-header" id="mutex">Mutex</h3>

<h3 class="mume-header" id="semaphore">Semaphore</h3>

<h3 class="mume-header" id="messagequeue">MessageQueue</h3>


      </div>
      
      
    
    
    
    
    
    
    
    
  
    </body>
