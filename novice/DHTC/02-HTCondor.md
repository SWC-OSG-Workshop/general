---
layout: lesson
root: ../..
title: HTCondor - job schedular
---
<div class="objectives" markdown="1">

#### Objectives
*   Basics of HTCondor 
*   ClassAd mechanism  
</div>

<h2> Basics of HTCondor </h2> 
HTCondor is a specialized workload management system for compute-intensive 
jobs. Like other full-featured batch systems, HTCondor provides a job 
queueing mechanism, scheduling policy, priority scheme, resource monitoring, 
and resource management. Users submit their serial or parallel jobs to 
HTCondor schedular. HTCondor places them into a queue, chooses when and 
where to run the jobs based upon a policy, carefully monitors their 
progress, and ultimately informs the user upon completion.

While providing functionality similar to that of a more traditional batch 
queueing system, HTCondor's novel architecture allows it to succeed in areas 
where traditional scheduling systems fail. HTCondor can be used to manage a 
cluster of dedicated compute nodes (such as a "Beowulf" cluster). In 
addition, unique mechanisms enable HTCondor to effectively harness wasted 
CPU power from otherwise idle desktop workstations. For instance, HTCondor 
can be configured to only use desktop machines where the keyboard and mouse are idle. Should HTCondor detect that a machine is no longer available (such as a key press detected), in many circumstances HTCondor is able to transparently produce a checkpoint and migrate a job to a different machine which would otherwise be idle. HTCondor does not require a shared file system across machines - if no shared file system is available, HTCondor can transfer the job's data files on behalf of the user, or HTCondor may be able to transparently redirect all the job's I/O requests back to the submit machine. As a result, HTCondor can be used to seamlessly combine all of an organization's computational power into one resource.

The ClassAd mechanism in HTCondor provides an extremely flexible and expressive framework for matching resource requests (jobs) with resource offers (machines). Jobs can easily state both job requirements and job preferences. Likewise, machines can specify requirements and preferences about the jobs they are willing to run. These requirements and preferences can be described in powerful expressions, resulting in HTCondor's adaptation to nearly any desired policy.

HTCondor can be used to build Grid-style computing environments that cross administrative boundaries. HTCondor's "flocking" technology allows multiple HTCondor compute installations to work together. HTCondor incorporates many of the emerging Grid and Cloud-based computing methodologies and protocols. For instance, HTCondor-G is fully interoperable with resources managed by Globus.

HTCondor is the product of years of research by the Center for High Throughput Computing in the Department of Computer Sciences at the University of Wisconsin-Madison (UW-Madison), and it was first installed as a production system in the UW-Madison Department of Computer Sciences over 15 years ago. This HTCondor installation has since served as a major source of computing cycles to UW-Madison faculty and students. Additional HTCondor installations have been established over the years across our campus and the world. Hundreds of organizations in industry, government, and academia have used HTCondor to establish compute installations ranging in size from a handful to many thousands of workstations.




<h2>Matchmaking with ClassAds</h2> 

Before you learn about how to submit a job, it is important to understand how HTCondor allocates resources. Understanding the unique framework by which HTCondor matches submitted jobs with machines is the key to getting the most from HTCondor's scheduling algorithm.

HTCondor simplifies job submission by acting as a matchmaker of ClassAds. HTCondor's ClassAds are analogous to the classified advertising section of the newspaper. Sellers advertise specifics about what they have to sell, hoping to attract a buyer. Buyers may advertise specifics about what they wish to purchase. Both buyers and sellers list constraints that need to be satisfied. For instance, a buyer has a maximum spending limit, and a seller requires a minimum purchase price. Furthermore, both want to rank requests to their own advantage. Certainly a seller would rank one offer of $50 dollars higher than a different offer of $25. In HTCondor, users submitting jobs can be thought of as buyers of compute resources and machine owners are sellers.

All machines in a HTCondor pool advertise their attributes, such as available memory, CPU type and speed, virtual memory size, current load average, along with other static and dynamic properties. This machine ClassAd also advertises under what conditions it is willing to run a HTCondor job and what type of job it would prefer. These policy attributes can reflect the individual terms and preferences by which all the different owners have graciously allowed their machine to be part of the HTCondor pool. You may advertise that your machine is only willing to run jobs at night and when there is no keyboard activity on your machine. In addition, you may advertise a preference (rank) for running jobs submitted by you or one of your co-workers.

Likewise, when submitting a job, you specify a ClassAd with your requirements and preferences. The ClassAd includes the type of machine you wish to use. For instance, perhaps you are looking for the fastest floating point performance available. You want HTCondor to rank available machines based upon floating point performance. Or, perhaps you care only that the machine has a minimum of 128 Mbytes of RAM. Or, perhaps you will take any machine you can get! These job attributes and requirements are bundled up into a job ClassAd.

HTCondor plays the role of a matchmaker by continuously reading all the job ClassAds and all the machine ClassAds, matching and ranking job ads with machine ads. HTCondor makes certain that all requirements in both ClassAds are satisfied.

