# Introduction-to-Large-scale-cluster-management-system
Technical reports about Borg(Google), Omega(Google), Apollo(Microsoft), Sigma(Alibaba)

吴幸融 516030910253 Brog
叶东諴 516030910020 Sigma
吴怜颐 516030910252 Omega
张宇航 516030910273 Apollo


# Borg

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