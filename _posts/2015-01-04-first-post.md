---
layout: post
title: Consistency Models in distributed systems
tags: [random, exciting-stuff]
---

#### Background : Perspectives and Consistency Models  
There are two main perspectives on consistency: data- centric and client-centric consistency [MbaaS-7]. While data- centric consistency guarantees focus on the internal state of a storage system, i.e., all replicas being identical, client-centric consistency guarantees focus on the consistency properties visible to a client having only black box access to a storage system. Therefor, data-centric consistency guarantees are important to system developers while to application developers only client-centric guarantees matter  
Both data-centric and client-centric consistency guarantees have two dimensions: ordering and staleness3. Staleness describes how much a given replica is lagging behind, either expressed in terms of time (t-visibility) or versions (k-staleness)  

##### Data- centric models (Server-side consistency models)  
1.Strong consistency  
A system adopting a strong consistency model is in a consistent state all times.
The strong consistency (also known as single-copy consistency, or linearizability) model is not suitable for mobile applications dealing with cloud data due to the availability and performance issues as mandated by CAP theorem.  

2.Sequential consistency **(SC)**  
Sequential Consistency (SC) is a slightly weaker form of the strong consistency with the condition that all requests are serialized in the same order on all replicas and that requests by the same client are executed in the order that they are received by the storage system. While this model does not guarantee anything about the recentness of values read by clients, it mandates that all updates become visible to clients in the same order.      

3.Causal consistency **(CC)**    
In a system adapting the Causal Consistency (CC) , all requests that have a causal relationship to another request must be serialized (i.e., executed) in the same order on all replicas while unrelated requests may be serialized in arbitrary order.  
4.Grouping operations  
This model deals with handling the cases of, series of read and write operations. The Grouping Operation model allows raising the level of granularity to span multiple reads and writes, into atomically executed unit.  

##### Client- centric models (Client-side consistency models)  
1.Weak  consistency  
A weak consistency model does not guarantee that subsequent accesses will return the updated value.The period between the update and the moment when it is guaranteed that any observer will always see the updated value is referred as the inconsistency window [].  

2.Eventual  consistency **(EC)**  
Eventual consistency is a form of Weak consistency with the additional guarantee that when no new updates are made to an object, eventually all replicas will see the last Eventual consistency is a form of Weak consistency with the additional guarantee that when no new updates are made to an object, eventually all replicas will see the last.  
Eventual consistency provides following four main ordering guarantees:  
1.Monotonic read consistency **(MRC)**    
After reading version n, the same client will never again read a version < n.  

2.Read your writes consistency **(RYWC)**   
After writing version n, the same client will never again read a version < n.   
 
3.Monotonic writes consistency **(MWC)**   
All writes by the same client will be serialized in their chronologic order, i.e., if there are two consecutive writes by the same client and a replica has already written the value of the second write, then this value will not be over- written once the first write arrives at the replica.  

4.Write Follows Read Consistency **(WFRC)**   
 It guarantees that an update following a read of version n will only execute on replicas that are at least of version n.  

The literature [] investigates the relationship between data-centric and client-centric consistency models and show that client-centric CC can only be reached if all four client-centric models (MRC, RYWC, MWC and WFRC) are guaranteed.  

**Data-centric Model**        	**MRC**     	  **RYWC** 	            **MWC**            **WFRC**
-----------------------   --------------     --------------        --------------      --------------
Weak Consistency	          N/A    	             N/A                    N/A   	            N/A
Eventual Consistency 	      Often               Often                 Often               Often
Causal Consistency        Single Client 	    Single  Client  	    Single   Client      Single   Client  
Sequential Consistency    Single Client        Single  Client      	Single  Client        Global
Linearizability           Global               Global                Global               Global
-----------------------  --------------     --------------        --------------      --------------
: Relationship Between Data-centric and Client-centric Consistency Models Ordered by the Strictness of their Guarantees.  
