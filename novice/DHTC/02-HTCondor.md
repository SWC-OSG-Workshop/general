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



<h2> ClassAd Mechanism </h2> 

2.3 Matchmaking with ClassAds

Before you learn about how to submit a job, it is important to understand how HTCondor allocates resources. Understanding the unique framework by which HTCondor matches submitted jobs with machines is the key to getting the most from HTCondor's scheduling algorithm.

HTCondor simplifies job submission by acting as a matchmaker of ClassAds. HTCondor's ClassAds are analogous to the classified advertising section of the newspaper. Sellers advertise specifics about what they have to sell, hoping to attract a buyer. Buyers may advertise specifics about what they wish to purchase. Both buyers and sellers list constraints that need to be satisfied. For instance, a buyer has a maximum spending limit, and a seller requires a minimum purchase price. Furthermore, both want to rank requests to their own advantage. Certainly a seller would rank one offer of $50 dollars higher than a different offer of $25. In HTCondor, users submitting jobs can be thought of as buyers of compute resources and machine owners are sellers.

All machines in a HTCondor pool advertise their attributes, such as available memory, CPU type and speed, virtual memory size, current load average, along with other static and dynamic properties. This machine ClassAd also advertises under what conditions it is willing to run a HTCondor job and what type of job it would prefer. These policy attributes can reflect the individual terms and preferences by which all the different owners have graciously allowed their machine to be part of the HTCondor pool. You may advertise that your machine is only willing to run jobs at night and when there is no keyboard activity on your machine. In addition, you may advertise a preference (rank) for running jobs submitted by you or one of your co-workers.

Likewise, when submitting a job, you specify a ClassAd with your requirements and preferences. The ClassAd includes the type of machine you wish to use. For instance, perhaps you are looking for the fastest floating point performance available. You want HTCondor to rank available machines based upon floating point performance. Or, perhaps you care only that the machine has a minimum of 128 Mbytes of RAM. Or, perhaps you will take any machine you can get! These job attributes and requirements are bundled up into a job ClassAd.

HTCondor plays the role of a matchmaker by continuously reading all the job ClassAds and all the machine ClassAds, matching and ranking job ads with machine ads. HTCondor makes certain that all requirements in both ClassAds are satisfied.

2.3.1 Inspecting Machine ClassAds with condor_status

Once HTCondor is installed, you will get a feel for what a machine ClassAd does by trying the condor_status command. Try the condor_status command to get a summary of information from ClassAds about the resources available in your pool. Type condor_status and hit enter to see a summary similar to the following:

Name               OpSys      Arch   State     Activity LoadAv Mem   ActvtyTime

amul.cs.wisc.edu   LINUX      INTEL  Claimed   Busy     0.990  1896  0+00:07:04
slot1@amundsen.cs. LINUX      INTEL  Owner     Idle     0.000  1456  0+00:21:58
slot2@amundsen.cs. LINUX      INTEL  Owner     Idle     0.110  1456  0+00:21:59
angus.cs.wisc.edu  LINUX      INTEL  Claimed   Busy     0.940   873  0+00:02:54
anhai.cs.wisc.edu  LINUX      INTEL  Claimed   Busy     1.400  1896  0+00:03:03
apollo.cs.wisc.edu LINUX      INTEL  Unclaimed Idle     1.000  3032  0+00:00:04
arragon.cs.wisc.ed LINUX      INTEL  Claimed   Busy     0.980   873  0+00:04:29
bamba.cs.wisc.edu  LINUX      INTEL  Owner     Idle     0.040  3032 15+20:10:19
...
The condor_status command has options that summarize machine ads in a variety of ways. For example,

condor_status -available
shows only machines which are willing to run jobs now.
condor_status -run
shows only machines which are currently running jobs.
condor_status -long
lists the machine ClassAds for all machines in the pool.
Refer to the condor_status command reference page located on page [*] for a complete description of the condor_status command.

The following shows a portion of a machine ClassAd for a single machine: turunmaa.cs.wisc.edu. Some of the listed attributes are used by HTCondor for scheduling. Other attributes are for information purposes. An important point is that any of the attributes in a machine ClassAd can be utilized at job submission time as part of a request or preference on what machine to use. Additional attributes can be easily added. For example, your site administrator can add a physical location attribute to your machine ClassAds.

Machine = "turunmaa.cs.wisc.edu"
FileSystemDomain = "cs.wisc.edu"
Name = "turunmaa.cs.wisc.edu"
CondorPlatform = "$CondorPlatform: x86_rhap_5 $"
Cpus = 1
IsValidCheckpointPlatform = ( ( ( TARGET.JobUniverse == 1 ) == false ) || 
 ( ( MY.CheckpointPlatform =!= undefined ) && 
 ( ( TARGET.LastCheckpointPlatform =?= MY.CheckpointPlatform ) || 
 ( TARGET.NumCkpts == 0 ) ) ) )
CondorVersion = "$CondorVersion: 7.6.3 Aug 18 2011 BuildID: 361356 $"
Requirements = ( START ) && ( IsValidCheckpointPlatform )
EnteredCurrentActivity = 1316094896
MyAddress = "<128.105.175.125:58026>"
EnteredCurrentState = 1316094896
Memory = 1897
CkptServer = "pitcher.cs.wisc.edu"
OpSys = "LINUX"
State = "Owner"
START = true
Arch = "INTEL"
Mips = 2634
Activity = "Idle"
StartdIpAddr = "<128.105.175.125:58026>"
TargetType = "Job"
LoadAvg = 0.210000
CheckpointPlatform = "LINUX INTEL 2.6.x normal 0x40000000"
Disk = 92309744
VirtualMemory = 2069476
TotalSlots = 1
UidDomain = "cs.wisc.edu"
MyType = "Machine"

<h2> Running a Job: the Steps To Take </h2> 

The road to using HTCondor effectively is a short one. The basics are quickly and easily learned.

Here are all the steps needed to run a job using HTCondor.

Code Preparation.
A job run under HTCondor must be able to run as a background batch job. HTCondor runs the program unattended and in the background. A program that runs in the background will not be able to do interactive input and output. HTCondor can redirect console output (stdout and stderr) and keyboard input (stdin) to and from files for the program. Create any needed files that contain the proper keystrokes needed for program input. Make certain the program will run correctly with the files.
The HTCondor Universe.
HTCondor has several runtime environments (called a universe) from which to choose. Of the universes, two are likely choices when learning to submit a job to HTCondor: the standard universe and the vanilla universe. The standard universe allows a job running under HTCondor to handle system calls by returning them to the machine where the job was submitted. The standard universe also provides the mechanisms necessary to take a checkpoint and migrate a partially completed job, should the machine on which the job is executing become unavailable. To use the standard universe, it is necessary to relink the program with the HTCondor library using the condor_compile command. The manual page for condor_compile on page [*] has details.
The vanilla universe provides a way to run jobs that cannot be relinked. There is no way to take a checkpoint or migrate a job executed under the vanilla universe. For access to input and output files, jobs must either use a shared file system, or use HTCondor's File Transfer mechanism.

Choose a universe under which to run the HTCondor program, and re-link the program if necessary.

Submit description file.
Controlling the details of a job submission is a submit description file. The file contains information about the job such as what executable to run, the files to use in place of stdin and stdout, and the platform type required to run the program. The number of times to run a program may be included; it is simple to run the same program multiple times with multiple data sets.
Write a submit description file to go with the job, using the examples provided in section 2.5 for guidance.

Submit the Job.
Submit the program to HTCondor with the condor_submit command.
Once submitted, HTCondor does the rest toward running the job. Monitor the job's progress with the condor_q and condor_status commands. You may modify the order in which HTCondor will run your jobs with condor_prio. If desired, HTCondor can even inform you in a log file every time your job is checkpointed and/or migrated to a different machine.

When your program completes, HTCondor will tell you (by e-mail, if preferred) the exit status of your program and various statistics about its performances, including time used and I/O performed. If you are using a log file for the job (which is recommended) the exit status will be recorded in the log file. You can remove a job from the queue prematurely with condor_rm.


2.4.1 Choosing an HTCondor Universe

A universe in HTCondor defines an execution environment. HTCondor Version 8.3.1 supports several different universes for user jobs:

Standard
Vanilla
Grid
Java
Scheduler
Local
Parallel
VM
The universe under which a job runs is specified in the submit description file. If a universe is not specified, the default is vanilla, unless your HTCondor administrator has changed the default. However, we strongly encourage you to specify the universe, since the default can be changed by your HTCondor administrator, and the default that ships with HTCondor has changed.

The standard universe provides migration and reliability, but has some restrictions on the programs that can be run. The vanilla universe provides fewer services, but has very few restrictions. The grid universe allows users to submit jobs using HTCondor's interface. These jobs are submitted for execution on grid resources. The java universe allows users to run jobs written for the Java Virtual Machine (JVM). The scheduler universe allows users to submit lightweight jobs to be spawned by the program known as a daemon on the submit host itself. The parallel universe is for programs that require multiple machines for one job. See section 2.9 for more about the Parallel universe. The vm universe allows users to run jobs where the job is no longer a simple executable, but a disk image, facilitating the execution of a virtual machine.


2.4.1.1 Standard Universe

In the standard universe, HTCondor provides checkpointing and remote system calls. These features make a job more reliable and allow it uniform access to resources from anywhere in the pool. To prepare a program as a standard universe job, it must be relinked with condor_compile. Most programs can be prepared as a standard universe job, but there are a few restrictions.

HTCondor checkpoints a job at regular intervals. A checkpoint image is essentially a snapshot of the current state of a job. If a job must be migrated from one machine to another, HTCondor makes a checkpoint image, copies the image to the new machine, and restarts the job continuing the job from where it left off. If a machine should crash or fail while it is running a job, HTCondor can restart the job on a new machine using the most recent checkpoint image. In this way, jobs can run for months or years even in the face of occasional computer failures.

Remote system calls make a job perceive that it is executing on its home machine, even though the job may execute on many different machines over its lifetime. When a job runs on a remote machine, a second process, called a condor_shadow runs on the machine where the job was submitted. When the job attempts a system call, the condor_shadow performs the system call instead and sends the results to the remote machine. For example, if a job attempts to open a file that is stored on the submitting machine, the condor_shadow will find the file, and send the data to the machine where the job is running.

To convert your program into a standard universe job, you must use condor_compile to relink it with the HTCondor libraries. Put condor_compile in front of your usual link command. You do not need to modify the program's source code, but you do need access to the unlinked object files. A commercial program that is packaged as a single executable file cannot be converted into a standard universe job.

For example, if you would have linked the job by executing:

% cc main.o tools.o -o program
Then, relink the job for HTCondor with:

% condor_compile cc main.o tools.o -o program
There are a few restrictions on standard universe jobs:


Multi-process jobs are not allowed. This includes system calls such as fork(), exec(), and system().

Interprocess communication is not allowed. This includes pipes, semaphores, and shared memory.

Network communication must be brief. A job may make network connections using system calls such as socket(), but a network connection left open for long periods will delay checkpointing and migration.

Sending or receiving the SIGUSR2 or SIGTSTP signals is not allowed. HTCondor reserves these signals for its own use. Sending or receiving all other signals is allowed.

Alarms, timers, and sleeping are not allowed. This includes system calls such as alarm(), getitimer(), and sleep().

Multiple kernel-level threads are not allowed. However, multiple user-level threads are allowed.

Memory mapped files are not allowed. This includes system calls such as mmap() and munmap().

File locks are allowed, but not retained between checkpoints.

All files must be opened read-only or write-only. A file opened for both reading and writing will cause trouble if a job must be rolled back to an old checkpoint image. For compatibility reasons, a file opened for both reading and writing will result in a warning but not an error.
A fair amount of disk space must be available on the submitting machine for storing a job's checkpoint images. A checkpoint image is approximately equal to the virtual memory consumed by a job while it runs. If disk space is short, a special checkpoint server can be designated for storing all the checkpoint images for a pool.

On Linux, the job must be statically linked. condor_compile does this by default.

Reading to or writing from files larger than 2 GBytes is only supported when the submit side condor_shadow and the standard universe user job application itself are both 64-bit executables.
2.4.1.2 Vanilla Universe

The vanilla universe in HTCondor is intended for programs which cannot be successfully re-linked. Shell scripts are another case where the vanilla universe is useful. Unfortunately, jobs run under the vanilla universe cannot checkpoint or use remote system calls. This has unfortunate consequences for a job that is partially completed when the remote machine running a job must be returned to its owner. HTCondor has only two choices. It can suspend the job, hoping to complete it at a later time, or it can give up and restart the job from the beginning on another machine in the pool.

Since HTCondor's remote system call features cannot be used with the vanilla universe, access to the job's input and output files becomes a concern. One option is for HTCondor to rely on a shared file system, such as NFS or AFS. Alternatively, HTCondor has a mechanism for transferring files on behalf of the user. In this case, HTCondor will transfer any files needed by a job to the execution site, run the job, and transfer the output back to the submitting machine.

Under Unix, HTCondor presumes a shared file system for vanilla jobs. However, if a shared file system is unavailable, a user can enable the HTCondor File Transfer mechanism. On Windows platforms, the default is to use the File Transfer mechanism. For details on running a job with a shared file system, see section 2.5.3 on page [*]. For details on using the HTCondor File Transfer mechanism, see section 2.5.4 on page [*].

2.4.1.3 Grid Universe

The Grid universe in HTCondor is intended to provide the standard HTCondor interface to users who wish to start jobs intended for remote management systems. Section 5.3 on page [*] has details on using the Grid universe. The manual page for condor_submit on page [*] has detailed descriptions of the grid-related attributes.

2.4.1.4 Java Universe


A program submitted to the Java universe may run on any sort of machine with a JVM regardless of its location, owner, or JVM version. HTCondor will take care of all the details such as finding the JVM binary and setting the classpath.

2.4.1.5 Scheduler Universe


The scheduler universe allows users to submit lightweight jobs to be run immediately, alongside the condor_schedd daemon on the submit host itself. Scheduler universe jobs are not matched with a remote machine, and will never be preempted. The job's requirements expression is evaluated against the condor_schedd's ClassAd.

Originally intended for meta-schedulers such as condor_dagman, the scheduler universe can also be used to manage jobs of any sort that must run on the submit host.

However, unlike the local universe, the scheduler universe does not use a condor_starter daemon to manage the job, and thus offers limited features and policy support. The local universe is a better choice for most jobs which must run on the submit host, as it offers a richer set of job management features, and is more consistent with other universes such as the vanilla universe. The scheduler universe may be retired in the future, in favor of the newer local universe.


2.4.1.6 Local Universe

The local universe allows an HTCondor job to be submitted and executed with different assumptions for the execution conditions of the job. The job does not wait to be matched with a machine. It instead executes right away, on the machine where the job is submitted. The job will never be preempted. The job's requirements expression is evaluated against the condor_schedd's ClassAd.


2.4.1.7 Parallel Universe

The parallel universe allows parallel programs, such as MPI jobs, to be run within the opportunistic HTCondor environment. Please see section 2.9 for more details.

2.4.1.8 VM Universe

HTCondor facilitates the execution of VMware and Xen virtual machines with the vm universe.
Please see section 2.11 for details.

