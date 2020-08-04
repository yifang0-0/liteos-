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
<li><a href="#task">Task</a>
<ul>
<li><a href="#%E7%AE%80%E4%BB%8B">简介</a></li>
<li><a href="#%E7%8A%B6%E6%80%81">状态</a></li>
<li><a href="#%E5%8A%9F%E8%83%BD">功能</a></li>
<li><a href="#api">API</a></li>
</ul>
</li>
<li><a href="#timer">Timer</a>
<ul>
<li><a href="#%E6%B5%81%E7%A8%8B">流程</a>
<ul>
<li><a href="#%E5%88%9D%E5%A7%8B%E5%8C%96-%E5%88%86%E5%9B%9B%E4%B8%AA%E9%83%A8%E5%88%86">初始化 分四个部分</a></li>
<li><a href="#%E8%BD%AF%E4%BB%B6%E5%AE%9A%E6%97%B6%E5%99%A8%E5%88%9B%E5%BB%BA-ostimercreate">软件定时器创建 osTimerCreate</a></li>
<li><a href="#%E8%BD%AF%E4%BB%B6%E5%AE%9A%E6%97%B6%E5%99%A8%E5%90%AF%E5%8A%A8">软件定时器启动</a></li>
<li><a href="#%E8%BD%AF%E4%BB%B6%E5%AE%9A%E6%97%B6%E5%99%A8%E8%B6%85%E6%97%B6%E6%89%AB%E6%8F%8F">软件定时器超时扫描</a></li>
<li><a href="#%E5%AE%9A%E6%97%B6%E5%99%A8%E5%88%A0%E9%99%A4-ostimerdelete">定时器删除 osTimerDelete</a></li>
</ul>
</li>
<li><a href="#api-1">API</a></li>
</ul>
</li>
<li><a href="#semaphore">Semaphore</a>
<ul>
<li><a href="#%E4%BF%A1%E5%8F%B7%E9%87%8F%E8%AE%BE%E8%AE%A1">信号量设计</a>
<ul>
<li><a href="#%E5%88%9D%E5%A7%8B%E5%8C%96">初始化</a></li>
<li><a href="#%E4%BF%A1%E5%8F%B7%E9%87%8F%E5%88%9B%E5%BB%BA%E4%B8%8E%E5%88%A0%E9%99%A4">信号量创建与删除</a></li>
</ul>
</li>
<li><a href="#%E4%BF%A1%E5%8F%B7%E9%87%8F%E7%94%B3%E8%AF%B7">信号量申请</a></li>
<li><a href="#%E4%BF%A1%E5%8F%B7%E9%87%8F%E7%9A%84%E9%87%8A%E6%94%BE">信号量的释放</a></li>
</ul>
</li>
<li><a href="#mutex">Mutex</a>
<ul>
<li><a href="#%E4%BA%92%E6%96%A5%E9%94%81%E7%8A%B6%E6%80%81">互斥锁状态</a></li>
<li><a href="#%E4%BA%92%E6%96%A5%E9%94%81%E8%AE%BE%E8%AE%A1">互斥锁设计</a>
<ul>
<li><a href="#%E5%88%9D%E5%A7%8B%E5%8C%96-1">初始化</a></li>
<li><a href="#%E4%BA%92%E6%96%A5%E9%94%81%E7%94%B3%E8%AF%B7">互斥锁申请</a></li>
<li><a href="#%E4%BA%92%E6%96%A5%E9%94%81%E9%87%8A%E6%94%BE">互斥锁释放</a></li>
</ul>
</li>
<li><a href="#api-2">API</a></li>
<li><a href="#%E6%B3%A8%E6%84%8F">注意:</a></li>
</ul>
</li>
<li><a href="#messagequeue">MessageQueue</a>
<ul>
<li><a href="#%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86">基础知识</a></li>
<li><a href="#%E8%BF%90%E4%BD%9C%E6%9C%BA%E5%88%B6">运作机制</a>
<ul>
<li><a href="#%E6%B6%88%E6%81%AF%E5%8F%91%E9%80%81">消息发送</a></li>
<li><a href="#%E6%B6%88%E6%81%AF%E6%8E%A5%E6%94%B6">消息接收</a></li>
</ul>
</li>
</ul>
</li>
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

<h4 class="mume-header" id="%E7%AE%80%E4%BB%8B">简介</h4>

<ul>
<li>LiteOS的任务一共有32个优先级(0-31)，最高优先级为0，最低优先级为31。同一优先级任务使用双向链表管理。</li>
<li>每一个任务都含有一个任务控制块(TCB)。TCB包含了任务上下文SP、任务状态、任务优先级、任务ID、任务名、任务栈大小等信息。</li>
<li>任务切换：在<strong>M3内核</strong>中利用第14号PendSV异常进行任务切换。 与11号SVC异常必须在执行SVC指令后立即得到响应，如果得不到响应，会上访成硬fault不同，PendSV可以像普通的中断一样被悬起的。</li>
</ul>
<h4 class="mume-header" id="%E7%8A%B6%E6%80%81">状态</h4>

<ul>
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
<h4 class="mume-header" id="%E5%8A%9F%E8%83%BD">功能</h4>

<ul>
<li>任务删除 任务创建</li>
<li>任务信息查询 LOS_TaskSelf</li>
</ul>
<h4 class="mume-header" id="api">API</h4>

<h3 class="mume-header" id="timer">Timer</h3>

<p>初始化：完成内存的分配、相关参数的设置、内部逻辑结构的建立、相关数据的初始化和为处理超时处理函数创建0号优先级任务。<br>
为了方便管理软件定时器控制块，在初始化时对定时器控制块进行申请内存空间，以数组的形式访问，从而达到定时器的快速定位，同时为了方便定时器的使用管理，把所有空闲定时器控制块以单向链表组织起来，构成一条空闲链表，当使用时直接从链表头取，回收时直接插入到链表首部。</p>
<h4 class="mume-header" id="%E6%B5%81%E7%A8%8B">流程</h4>

<p>初始化，创建0号超时处理函数/所有空闲定时器控制块以单向链表链接<br>
定时器模块：计时结构sortlink，全部指向一个单向循环链表</p>
<h5 class="mume-header" id="%E5%88%9D%E5%A7%8B%E5%8C%96-%E5%88%86%E5%9B%9B%E4%B8%AA%E9%83%A8%E5%88%86">初始化 分四个部分</h5>

<p><strong>1.定时器</strong>本体</p>
<ul>
<li>内存分配</li>
<li>控制块数据初始化</li>
</ul>
<p><strong>2.定时器链表</strong><br>
1.初始化 osTimerCreate<br>
- m_pstSwTmrSortList=NULL</p>
<pre class="language-text">2.规则
- 定时器按照超时从早到晚排序
- 每个tick中断对当前链表进行扫描，超时的进行处理
</pre>
<p><strong>3.sortlink</strong>设置</p>
<ul>
<li>Tick当前检验的定时器链表类 初始==0</li>
<li>TimerList*指向<strong>链表数组</strong>首地址，每个sortlink元素指向一个单向循环链表</li>
</ul>
<p><strong>4.定时器超时回调结构内存池与队列</strong></p>
<ul>
<li>m_aucSwTmrHandlerPool 一块8字节 4回调函数指针+4回调函数参数</li>
<li>m_uwSwTmrHandlerQueue 队列长度等于上一个的块数</li>
<li>在tick中扫描到有定时器超时，申请一块固定大小内存块，封装放入队列</li>
</ul>
<p><strong>5.定时器超时处理队列与任务</strong></p>
<ul>
<li>超时回调队列创建完成后<strong>创建定时器超时处理任务</strong></li>
<li><strong>定时器超时处理任务</strong>读取队列执行回调函数</li>
<li>等待时间永远</li>
<li>根据队列内容（回调函数指针与参数）</li>
</ul>
<p>注意：定时器回调函数中不要调用挂起定时器超时处理接任务的接口。因为串行处理，后方定时器超时后会提前调度、等待；或者使得整个队列爆满</p>
<h5 class="mume-header" id="%E8%BD%AF%E4%BB%B6%E5%AE%9A%E6%97%B6%E5%99%A8%E5%88%9B%E5%BB%BA-ostimercreate">软件定时器创建 osTimerCreate</h5>

<pre class="language-text">- 调用创建接口
- 设置属性osTimerDef：计时长度、触发模式、超时处理函数（Timer1_Callback）、超时处理参数、返回ID
</pre>
<h5 class="mume-header" id="%E8%BD%AF%E4%BB%B6%E5%AE%9A%E6%97%B6%E5%99%A8%E5%90%AF%E5%8A%A8">软件定时器启动</h5>

<pre class="language-text">- 调用接口：设置状态为 OS_SWTMR_STATUS_TICKING设定计时开始/重新开始  
- 设置计数时间初始值为当前计时总和与
</pre>
<h5 class="mume-header" id="%E8%BD%AF%E4%BB%B6%E5%AE%9A%E6%97%B6%E5%99%A8%E8%B6%85%E6%97%B6%E6%89%AB%E6%8F%8F">软件定时器超时扫描</h5>

<ul>
<li>每个Tick中断对定时器初始处理</li>
<li>定时器内部逻辑结构设定:每个定时器设置超时时间与计数时间，计数时间Cn采用向下减一的方式计数.</li>
</ul>
<blockquote>
<p>n=1 Cn=Tn;<br>
n&gt;1 Cn=Tn-C1-C2...-C(N-1)<br>
将定时器插在第一个使其Cn值不为0的地方<br>
可以只读第一个就判断是否存在超时定时器</p>
</blockquote>
<ul>
<li>如果扫描到Cn==0的定时器,判断其状态:periodic(restart),once(delete)</li>
</ul>
<h5 class="mume-header" id="%E5%AE%9A%E6%97%B6%E5%99%A8%E5%88%A0%E9%99%A4-ostimerdelete">定时器删除 osTimerDelete</h5>

<p>软件定时器的删除操作是把已经处于计时或处于暂停状态的定时器进行回收，把孤立散列的控制块插入到空闲链表中，如果定时器控制块在计时链表上，则先停止，再将孤立控制块回收。</p>
<ul>
<li>用户调用软件定时器的删除接口，传入定时器ID</li>
<li>验证其合法性</li>
</ul>
<blockquote>
<p>未使用状态（<strong>OS_SWTMR_STATUS_UNUSED</strong>），则返回系统错误<br>
计时状态（<strong>OS_SWTMR_STATUS_TICKING</strong>），则先停止该定时器，再将其插入到空闲链表中，并修改相应的定时器统计状态<br>
空闲状态（<strong>OS_SWTMR_STATUS_CREATED</strong>），则直接将其插入到空闲链表中，并修改状态<br>
最后已经删除的定时器的状态为<strong>OS_SWTMR_STATUS_UNUSED</strong>（空闲状态）</p>
</blockquote>
<h4 class="mume-header" id="api-1">API</h4>

<table>
<thead>
<tr>
<th>功能分类</th>
<th>接口名</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>定义定时器</td>
<td>osTimerDef</td>
<td>定义定时器</td>
</tr>
<tr>
<td>获取定时器资源</td>
<td>osTimer</td>
<td>获取定时器资源</td>
</tr>
<tr>
<td>创建定时器</td>
<td>osTimerCreate</td>
<td>创建定时器</td>
</tr>
<tr>
<td>开启定时器</td>
<td>osTimerStart</td>
<td>开启定时器</td>
</tr>
<tr>
<td>停止定时器</td>
<td>osTimerStop</td>
<td>停止定时器</td>
</tr>
<tr>
<td>删除定时器</td>
<td>osTimerDelete</td>
<td>删除定时器</td>
</tr>
</tbody>
</table>
<h3 class="mume-header" id="semaphore">Semaphore</h3>

<p>信号量（Semaphore）是内核IPC通信方式的一种。信号量对象具有一个内部计数器。它支持两种原子操作：<strong>申请（Pend）<strong>和</strong>释放（Post）</strong></p>
<ul>
<li>pend操作申请指定的信号量,如果其值大于0-&gt;对应信号量-1返回成功信号;如果其值等于0,任务阻塞,进入等待状态;</li>
<li>post操作释放指定的信号量,没有任务等待该信号就直接将计数器加一;否则唤起挂起队列中第一个任务.</li>
</ul>
<h4 class="mume-header" id="%E4%BF%A1%E5%8F%B7%E9%87%8F%E8%AE%BE%E8%AE%A1">信号量设计</h4>

<h5 class="mume-header" id="%E5%88%9D%E5%A7%8B%E5%8C%96">初始化</h5>

<ul>
<li>信号量初始化为用户配置的N个信号量申请内存，并把所有的信号量初始化成未使用，并加入到未使用链表中供系统使用。</li>
<li>若为当前用户配置的信号量为0则返回错误码</li>
</ul>
<h5 class="mume-header" id="%E4%BF%A1%E5%8F%B7%E9%87%8F%E5%88%9B%E5%BB%BA%E4%B8%8E%E5%88%A0%E9%99%A4">信号量创建与删除</h5>

<ul>
<li>创建</li>
</ul>
<blockquote>
<p>开始<br>
<strong>入参合法性</strong><br>
关中断<br>
*检测是否有可用信号<br>
有:获取信号量修改其状态标志设置处置处置,开中断返回成功   没有:开中断返回失败<br>
结束</p>
</blockquote>
<ul>
<li>删除<br>
同上 *除改为检查是否有任务阻塞与当前信号<br>
有:开中断 返回失败 无:修改标志加入未使用链表 开中断返回成功</li>
</ul>
<h4 class="mume-header" id="%E4%BF%A1%E5%8F%B7%E9%87%8F%E7%94%B3%E8%AF%B7">信号量申请</h4>

<p>任务需要Pend信号量，判断信号量计数是否大于0，若大于0则减1后成功返回，否则</p>
<ul>
<li>无阻塞模式：直接返回pend失败。</li>
<li>永久阻塞模式：该任务进入阻塞态，系统切换到就绪任务中优先级最高者继续执行。任务进入阻塞态后，直到有其他任务释放该信号量，阻塞任务才会重新得以执行。</li>
<li>定时阻塞模式：同上,或是用户指定时间超时后，阻塞任务才会重新得以执行(超时后直接进行任务超时处理)。</li>
</ul>
<p>注意:用户需要自行保证不在中断线程中使用永久阻塞和定时阻塞</p>
<h4 class="mume-header" id="%E4%BF%A1%E5%8F%B7%E9%87%8F%E7%9A%84%E9%87%8A%E6%94%BE">信号量的释放</h4>

<table>
<thead>
<tr>
<th>功能分类</th>
<th>接口名</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>信号量的创建和删除</td>
<td>LOS_SemCreate</td>
<td>创建信号量。</td>
</tr>
<tr>
<td></td>
<td>LOS_SemDelete</td>
<td>删除指定的信号量。</td>
</tr>
<tr>
<td>信号量的申请和释放</td>
<td>LOS_SemPend</td>
<td>申请指定的信号量。</td>
</tr>
<tr>
<td></td>
<td>LOS_SemPost</td>
<td>释放指定的信号量。</td>
</tr>
</tbody>
</table>
<h3 class="mume-header" id="mutex">Mutex</h3>

<p>互斥锁是一种用于<strong>线程之间互相排斥</strong>的装置。它通过对公共临界区的原子操作来实现保护共享数据结构的目的。</p>
<h4 class="mume-header" id="%E4%BA%92%E6%96%A5%E9%94%81%E7%8A%B6%E6%80%81">互斥锁状态</h4>

<ul>
<li>解锁</li>
<li>加锁</li>
</ul>
<h4 class="mume-header" id="%E4%BA%92%E6%96%A5%E9%94%81%E8%AE%BE%E8%AE%A1">互斥锁设计</h4>

<h5 class="mume-header" id="%E5%88%9D%E5%A7%8B%E5%8C%96-1">初始化</h5>

<ul>
<li>
<p>申请N个互斥锁的内存</p>
</li>
<li>
<p>加入<strong>未使用的信号量链表</strong></p>
</li>
<li>
<p>创建和删除</p>
<ul>
<li>创建:从未使用的互斥锁列表中获取一个互斥锁资源设定初值</li>
<li>删除:将正在使用的互斥锁置空并挂回链表</li>
</ul>
</li>
</ul>
<h5 class="mume-header" id="%E4%BA%92%E6%96%A5%E9%94%81%E7%94%B3%E8%AF%B7">互斥锁申请</h5>

<ul>
<li>互斥锁申请任务需要Pend互斥锁，判断互斥锁计数是否为0，若为0则加1后成功返回，若不为0，则判断持有该互斥锁的任务是否和申请该互斥锁的任务为同一个任务，若为同一个任务，则加1后成功返回,,否则:
<ul>
<li>无阻塞模式：直接返回pend失败。</li>
<li>永久阻塞模式：该任务进入阻塞态，系统切换到就绪任务中优先级最高者继续执行。任务进入阻塞态后，直到有其他任务释放该信号量，阻塞任务才会重新得以执行。</li>
<li>定时阻塞模式：同上,或是用户指定时间超时后，阻塞任务才会重新得以执行(超时后直接进行任务超时处理)。</li>
</ul>
<blockquote>
<p>和信号量申请对比:互斥锁是有锁不能分配,即计数量==0是可以得到锁;而同一任务多次请求则在当前锁上加1,其他任务不饿能相加</p>
</blockquote>
</li>
</ul>
<h5 class="mume-header" id="%E4%BA%92%E6%96%A5%E9%94%81%E9%87%8A%E6%94%BE">互斥锁释放</h5>

<p>互斥锁释放操作是互斥锁申请操作的相反过程，过程中需要判断是否有任务阻塞于这个互斥锁。</p>
<ul>
<li>如果有任务阻塞于指定互斥锁，则唤醒最早被阻塞的任务
<ul>
<li>如果被唤醒的任务被其他线程suspend，那么该任务会从互斥锁和suspend双阻塞态和转到suspend态</li>
<li>如果被唤醒的任务没有被其他线程suspend，那么该任务进入就绪态，并进行调度</li>
</ul>
</li>
<li>如果没有任务阻塞于指定互斥锁，则对互斥锁计数加1，返回成功。</li>
</ul>
<h4 class="mume-header" id="api-2">API</h4>

<table>
<thead>
<tr>
<th>功能分类</th>
<th>接口名</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>互斥锁的创建和删除</td>
<td>LOS_MuxCreate</td>
<td>创建互斥锁</td>
</tr>
<tr>
<td></td>
<td>LOS_MuxDelete</td>
<td>删除指定的互斥锁。</td>
</tr>
<tr>
<td>互斥锁的申请和释放</td>
<td>LOS_MuxPend</td>
<td>申请指定的互斥锁。</td>
</tr>
<tr>
<td></td>
<td>LOS_MuxPost</td>
<td>释放指定的互斥锁。</td>
</tr>
</tbody>
</table>
<h4 class="mume-header" id="%E6%B3%A8%E6%84%8F">注意:</h4>

<p>所有互斥同步的资源创建与删除都需在处理之前关中断,结束之前再开中断</p>
<h3 class="mume-header" id="messagequeue">MessageQueue</h3>

<h4 class="mume-header" id="%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86">基础知识</h4>

<p>LiteOS中使用队列数据结构实现任务通讯和同步工作，具有如下特性：</p>
<ol>
<li>
<p>消息以先进先出方式排队，循环使用，读写支持同步工作方式；</p>
</li>
<li>
<p>发送消息类型为一个整形或者指针大小（不可用栈上内存）；</p>
</li>
<li>
<p>一个任务能够从任一个队列接受和发送消息；</p>
</li>
<li>
<p>多个任务能够从同一个消息队列接受和发送消息；</p>
</li>
<li>
<p>当队列使用结束后，如果动态申请的内存，可以通过删除回收内存。</p>
</li>
</ol>
<h4 class="mume-header" id="%E8%BF%90%E4%BD%9C%E6%9C%BA%E5%88%B6">运作机制</h4>

<p>由队列实现:维护头指针与尾指针</p>
<ul>
<li>Head:已经写入的第一个可用的消息</li>
<li>Tail:可以写入的空闲的消息单元</li>
</ul>
<blockquote>
<p>Tail每次写入向后移动<br>
Tail如果指向队列末尾则重新指向队列头<br>
当Tail与head重合时说明队列已满,任务(写消息Task)被挂起或者写失败<br>
对空队列读操作也导致挂起或写失败</p>
</blockquote>
<h5 class="mume-header" id="%E6%B6%88%E6%81%AF%E5%8F%91%E9%80%81">消息发送</h5>

<p>要点:</p>
<ul>
<li>判断是否存在可用节点</li>
<li>写入消息队列后判断是都存在Task挂起在队列</li>
<li>放入就绪队列<br>
如果有挂起
<ul>
<li>挂起的队列是否等待Timer</li>
<li>从Timer链表中移除Task</li>
</ul>
</li>
</ul>
<h5 class="mume-header" id="%E6%B6%88%E6%81%AF%E6%8E%A5%E6%94%B6">消息接收</h5>

<h5>队列删除</h5>
<p>删除ID-&gt;入参合法性检查-&gt;合法:是否有Task等待;否:返回-&gt;是:是否有消息在队列中;否:返回-&gt;是:删除队列释放内存;否:返回</p>
<h4>mail 使用</h4>
<p>与Message不同的是：</p>
<ul>
<li>内存分配/释放<br>
Mail在创建的同时已经分配的内存，用户可以通过Mail的内存分配、释放接口来获取、释放内存。</li>
<li>任务等待内存分配<br>
Mail在申请内存时，会因为没有内存而被挂起（在中断中不允许等待），发送时则不会引起任务等待；</li>
</ul>
      </div>
    </body>
