# Introduction to Large scale cluster management system
Technical reports about [Borg](#borg)(Google), [Omega](#omega)(Google), [Apollo](#apollo)(Microsoft) and [Sigma](#sigma)(Alibaba).

# Borg
By 吴幸融 516030910253

## Characteristics

+ a cluster manager that runs multiple jobs, applications across multiple machines
   + combining admission control, efficient task-packing, over-commitment, and machine sharing with process-level performance isolation
   + runtime features that minimize fault-recovery time
   + scheduling policies that reduce the probability of correlated failures
   + offering a declarative job specification language, name service integration, real-time job monitoring, and tools to analyze and simulate system behavior
   + operating at a large scale, with a high degree of resiliency and completeness

## Pros & Cons

### Pros
1. hides the details of resource management and failure handling so its users can focus on application development instead
2. operates with very high reliability and availability, and supports applica-tions that do the same
3. lets us run workloads across tens of thousands of machines effectively

### Cons
1. jobs are restrictive as the only grouping mechanism for tasks
2. one IP address per machine complicates things.
3. optimizing for power users at the expense of casual ones

## Comment

Borg is Google's first try to create its own cluster manager to give the globe its various services across many servers as effectively as possible. Only giants like Google would have the initiative to build such a system and to improve it while using.

Borg approaches the problem by defining a lot of concepts from the user perspective first, like clusters and cells, and jobs and tasks. After that Borg uses policies such as divide and conquer and pays attention to properties like scalability and safety to implement and complete its system.

The route to develop a useful system like Borg gives us a lesson.
# Omega
By 吴怜颐 516030910252

## Characteristics

+ shared-state scheduling
   + no central resource allocator
   + appropriate permissions and priority of scheduler
   + no inter-scheduler head of line blocking
   + typically incremental transactions, sometimes all-or-nothing transaction
   + individual schedulers is limited by configuration settings 
   + lock-free optimistic concurrency 

## Pros & Cons

### Pros
1. eliminate the limited parallelism due to pessimistic concurrency control and restricted visibility of resources
   + schedule all jobs in the workload
   + the lines for batch and service jobs are independent
2. can support schedulers with a high one-off per-job decision time, and ones with a large per-task decision time
3. scale well, and tolerate large decision times on real cluster workloads

### Cons
1. no central policy-enforcement, which means complete fairness is hand to be achieved, and starving may exist
2. the performance is determined by the frequency of conflicts and their costs, which means omega will show poor performance as the conflicts increase
3. since the scheduler has the appropriate permission and priority, it will be dangerous if there is any spiteful scheduler trying to commit unnecessary transactions which cause many conflicts

## Comment

As far as I concerned, Omega is a effective scheduler architesture in both implementation extensibility and performance scalability, which shows its high efficiency in scaling the workload and load-balancing the batch scheduler.

However, it should be still improved in how to decrease conflicts and achieve fairness in the worst situation.

Maybe it can get high points in Google's internal environment, but in my opinion, there is still much uncertainty when it is used in the bigger world, which contains several totally different environments.

# Apollo
By 张宇航 516030910273
## Characteristics

   + adopt a distributed and (loosely) coordinated scheduling framework
   + schedule each task on a server that minimizes the task completion time
   + introduce a lightweight hardware-independent mechanism to advertise load on servers
   + a series of correction mechanisms that dynamically adjust and rectify suboptimal decisions at runtime
   + introduce opportunistic scheduling: regular tasks and opportunistic tasks. 
   + support staged rollout to production clus- ters and validation at scale

## Pros & Cons

### Pros
1. can balance scalability and scheduling quality
    + avoids the suboptimal (and often conflicting) decisions by independent schedulers of a completely decentralized archi- tecture, while removing the scalability bottleneck and single point of failure of a centralized design.
2. can achieve high-quality scheduling decisions
    + the estimation model incorporates a variety of factors and allows a scheduler to perform a weighted decision, rather than solely considering data locality or server load
    + the data parallel nature of computation allows Apollo to refine the estimates of task execu- tion time continuously based on observed runtime statistics from similar tasks during job execution.
3. can supply individual schedulers with cluster information
4. can present a unique deferred correction mechanism that allows resolving conflicts between independent schedulers only if they have a significant impact
5. can drive high cluster utilization while maintaining low job latencies
6. can ensure no service disruption or performance re- gression when we roll out Apollo to replace a previ- ous scheduler deployed in production

### Cons
1. the overall increase in I/O pressure caused a latency impact and interfered with tasks even running in regular mode
2. as the conflicts increase, it may result in poor performance
3. No good defense against any malicious scheduler trying to commit an unnecessary transaction that caused many conflicts


## Comment

In my opinion, Apollo is a scalable and coordinated scheduling framework for cloud-scale comput- ing. Apollo adopts a distributed and loosely coordinated scheduling architecture that scales well without sacrificing scheduling quality. However, it should be still improved in achieve fairness against the worst situation.

Although Apollo has been scored high in Microsoft's internal environment, there is still much uncertainty when it is used in a bigger platform, which is a totally different environments for Apollo.

# Sigma
By 叶东諴 516030910020

## 特点
Sigma是阿里巴巴全集团范围的Pouch容器调度系统，阿里全网所有机房在线服务管控的核心角色。2017年Sigma第一次正式参与双11，在双11期间成功撑了全集团所有容器（交易线中间件、数据库、告等多业务）的调配，使双11IT成本降低50%。

+ 线上线下混部
+ 复杂约束下的批量调度优化
+ 混合云+弹性

## 优劣
### 优点
+ 灵活可配置的调度策略
   + 支持多样化的应用场景
   + 业务团队开发出新的策略，可立即配置生效，不需要代码发布
+ 批量优化
   + Scheduling optimization module
+ 大规模快速建站
### 缺点
+ 应用间干扰
+ 异构计算需要优化

## 点评
在过去，阿里各个部门资源池相互独立，存在多套调度系统，资源分配不均，资源利用率较低。Sigma调度与集群管理系统应运而生。此后经历不断开发，最关键的应该是sigma与fuxi系统形成混部架构，这使CPU的日均利用率从10%提升到了40+%。
这样的一个调度与集群管理系统能够使工作所需的硬件大大减少，节省可观的开销，也避免频繁、大量地增减硬件。从上述提到的几个优点来看，这套庞大的系统也同时考虑到了易用性，能够热配置、一键建站。阿里依靠这套系统，能够应付双11庞大的访问量，足见其强大。
