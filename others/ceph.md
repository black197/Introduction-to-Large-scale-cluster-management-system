## 516030910273
## Vendors or types or technologies
### Features
* Ceph delivers unified storage, supporting File, Block and Object. 
* By shedding design assumptions like allocation lists found in nearly all existing systems, we maximally separate data from metadata management, allowing them to scale independently. 
* Ceph does striping of individual files across multiple nodes to achieve higher throughput, similar to how RAID0 stripes partitions across multiple hard drives. Adaptive load balancing is supported whereby frequently accessed objects are replicated over more nodes.
### Pros
* The obvious point of File, Block and Object in the same wrapper. It might be an obvious point, but it’s a pretty damn important one.
* Better transfer speed and lower latency, because traffic to and from the Swift cluster goes through proxy servers which slow it down.
* Swift can have further latency problems as replicas are not necessarily updated at the same time, so requesters retrieving data can access old wrong/outdated versions.
### Cons
* Ceph’s multi-region support – usually touted as an advantage, is in a master slave configuration, but as replication is only possible from master to slave, in a deployment with 2+ regions you can get uneven load distribution. 
* There can also be a security issue as RADOS clients on the Cloud compute node communicate directly with the RADOS servers over the same network Ceph uses for unencrypted replication traffic. So, potentially, if Ceph client node is compromised, the attacker can see all traffic on the storage network.
## Key indicators
### What, how to measure, scope
* OSD Throughput,Write Latency,Data Distribution and Scalability
* Metadata Update Latency,Metadata Read Latency, Metadata Scaling
## Your own comment
To be honest, Ceph isn’t perfect for everyone. It’s not the most efficient at using flash or CPU (but it’s getting better), the file storage feature isn’t fully mature yet, and it is missing key efficiency features like deduplication and compression. And some customers just aren’t comfortable with open-source or software-defined storage of any kind. But every release of Ceph adds new features and improved performance, while system integrators build turnkey Ceph appliances that make it easy to deploy and come with integrated hardware and software support.

