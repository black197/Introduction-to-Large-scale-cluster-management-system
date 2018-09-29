# Introduction-to-Large-scale-cluster-management-system
Technical reports about Borg(Google), Omega(Google), Apollo(Microsoft), Sigma(Alibaba)

吴幸融 516030910253 Brog
叶东諴 516030910020 Sigma
吴怜颐 516030910252 Omega
张宇航 516030910273 Apollo


# Omega

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
