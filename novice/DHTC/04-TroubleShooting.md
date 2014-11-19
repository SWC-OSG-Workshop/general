---
layout: lesson
root: ../..
title: Trouble Shooting
---
<div class="objectives" markdown="1">

#### Objectives
*   Learn how to trouble shoot the failed jobs.
*   Learn how to retry the failed jobs.
</div>

<h2>Overview </h2> 
We will discuss how to check the job failures and ways to correct the failures.  

<h2> Troubleshooting techniques </h2> 

<h3> Condor_q diagnostics </h3> 
The condor_q command has several tools that can be used to diagnose why jobs are not running. We could use 
 -analyze and -better-analyze options that allow the users to get a detailed information. 

~~~
$ condor_q -analyze JOB-ID # JOB-ID is the job indentification number 
~~~

or 

~~~
$ condor_q -better-analyze JOB-ID # JOB-ID is the job indentification number 
~~~



Sometimes the submitted jobs remain in the idle state and don't start for a very long time. In such
a case, it is good idea to double check the job requirements. Say for example, *condor_q -better-analyze* 
indicates the failure of satisfying the job requirement. 

~~~
$ condor_q -better-analyze JOB-ID 
 
 
-- Submitter: login01.osgconnect.net : <192.170.227.195:56174> : login01.osgconnect.net
IndexSet::Init: size out of range: 0
IndexSet::Init: IndexSet not initialized
User priority for username@login01.osgconnect.net is not available, attempting to analyze without it.
---
371156.008:  Run analysis summary.  Of 0 machines,
      0 are rejected by your job's requirements
      0 reject your job because of their own requirements
      0 match and are already running your jobs
      0 match but are serving other users
      0 are available to run your job
 
WARNING:  Be advised:
   No resources matched request's constraints
 
The Requirements expression for your job is:
 
    ( ( OpSys == "LINUX" && OpSysMajorVer == 10 ) ) &&
    ( TARGET.Arch == "X86_64" ) && ( TARGET.Disk >= RequestDisk ) &&
    ( TARGET.Memory >= RequestMemory ) && ( TARGET.HasFileTransfer )
 
Your job defines the following attributes:
 
    DiskUsage = 12
    ImageSize = 12
    RequestDisk = 12
    RequestMemory = 1
 
The Requirements expression for your job reduces to these conditions:
 
         Slots
Step    Matched  Condition
-----  --------  ---------
[0]           0  OpSys == "LINUX"
[1]           0  OpSysMajorVer == 10
[3]           0  TARGET.Arch == "X86_64"
[5]           0  TARGET.Disk >= RequestDisk
[7]           0  TARGET.Memory >= RequestMemory
[9]           0  TARGET.HasFileTransfer
 
Suggestions:
 
    Condition                         Machines Matched    Suggestion
    ---------                         ----------------    ----------
1   target.OpSys == "LINUX"           0                   REMOVE
2   target.OpSysMajorVer == 10        0                   REMOVE
3   ( TARGET.Arch == "X86_64" )       0                   REMOVE
4   ( TARGET.Disk >= 12 )             0                   REMOVE
5   ( TARGET.Memory >= ifthenelse(MemoryUsage isnt undefined,MemoryUsage,1) )
                                      0                   REMOVE
6   ( TARGET.HasFileTransfer )        0                   REMOVE
 
WARNING:  Be advised:
   Request did not match any resource's constraints

~~~

The output clearly indicates that job did not match any resources.  Additionally, by looking through 
the conditions listed, it becomes apparent that the job requires version Scientific Linux 
10 (target.OpSysMajorVer == 10) which won't be matched by available resources since the OSG pool  
of machines have Scientific Linux 6.0 or older.  Looking at the submit file for the job shows the 
following requirement: Requirements = (OpSys == "LINUX" && OpSysMajorVer == 10)
This can be corrected in two different ways.  The entire job is removed and re-submited,

~~~
$ condor_rm JOB-ID #remove the specific job that has problem in matching the resources.

$ nano file.submit # Edit the submit file and insert this line: Requirements '(OpSys == "LINUX" && OpSysMajorVer == 6)'
$ condor_submit file.submit
~~~

or 

~~~
condor_qedit JOB-ID Requirements '(OpSys == "LINUX" && OpSysMajorVer == 6)' #
~~~


<h3> condor_ssh_to_job </h3> 
This command allows the user to ssh to the compute node that is running a specified job_id.  Once the 
command is run, the user will be in the job's working directory and can examine the job's environment 
and run commands.  

<h3> Held jobs </h3>

The jobs go to the held state for several reasons. One of them is due to the failure to transfer the output
files. If this is the case, you might see the output of the analysis something as follows

~~~
$ condor_q -analyze 372993.0
-- Submitter: login01.osgconnect.net : <192.170.227.195:56174> : login01.osgconnect.net
---
372993.000:  Request is held.
Hold reason: Error from glidein_9371@compute-6-28.tier2: STARTER at 10.3.11.39 failed to send file(s) to <192.170.227.195:40485>: error reading from /wntmp/condor/compute-6-28/execute/dir_9368/glide_J6I1HT/execute/dir_16393/outputfile: (errno 2) No such file or directory; SHADOW failed to receive file(s) from <192.84.86.100:50805>
~~~

The important parts are the message indicating that HTCondor couldn't transfer the job 
back (SHADOW failed to receive file(s)) and the part just before this that gives the name of the 
file or directory that HTCondor couldn't find.  This failure is probably due to your application 
encountering an error while executing and then exiting before writing it's output files.  If you think 
that the error is a transient one and won't reoccur, you can run 

~~~
condor_release JOB-ID 
~~~
to requeue the job.  Alternatively, you can use *condor_ssh_to_job* to examine the job environment and investigate further.



#### Keypoints
*    *condor_q* command has some in build diagnostics.    
*    retry the filed jobs. 
</div>


