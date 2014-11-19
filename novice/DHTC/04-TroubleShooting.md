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

<h3> Diagnostics with condor_q  </h3> 
The *condor_q* command has several tools that can be used to diagnose why jobs are not running. We could use 
the options " -analyze" and "-better-analyze" that allow the users to get detailed information. 

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
 
# Produces a long ouput. 
# The following lines are part of the output regarding the job requirements.  
 
         Slots
Step    Matched  Condition
-----  --------  ---------
[0]           0  OpSys == "LINUX"
[1]           0  OpSysMajorVer == 10                 \\Requirement is wrong and needs correction.
[3]           0  TARGET.Arch == "X86_64"
[5]           0  TARGET.Disk >= RequestDisk
[7]           0  TARGET.Memory >= RequestMemory
[9]           0  TARGET.HasFileTransfer
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

<h3> Held jobs and condor_release </h3>

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


<h3> Retries with periodic_release </h3>

Often we want to automatically find out about the job failure and re-submit the failed jobs without 
manual intervention. Condor supports this automatic deduction and retries.  This is done in two steps.
In the first step, a job is forced to an held state if the exit code is not zero (incomplete job). The 
last step is to re-submit the job at the held state for a specified number of times. In the condor 
submit file, we include the following lines

~~~
# Send the job to Held state on failure. 
on_exit_hold = (ExitBySignal == True) || (ExitCode != 0)  

# Periodically retry the jobs for 3 times with an interval 1 hour.   
periodic_release =  (NumJobStarts < 3) && ((CurrentTime - EnteredCurrentStatus) > (60*60))
~~~


<div class="keypoints" markdown="1">
#### Keypoints
*    *condor_q -better-analyze JOB-ID* command is useful to diagnose failed jobs. 
*    Automatically deduct and retry the filed jobs with *on_exit_old* and *periodic_release* in the job submit file. 
</div>


