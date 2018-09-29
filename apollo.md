# Introduction-to-Large-scale-cluster-management-system
Technical reports about Borg(Google), Omega(Google), Apollo(Microsoft), Sigma(Alibaba)

吴幸融 516030910253 Brog
叶东諴 516030910020 Sigma
吴怜颐 516030910252 Omega
张宇航 516030910273 Apollo


# Apollo

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
