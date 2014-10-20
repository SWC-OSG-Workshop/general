---
layout: lesson
root: ../..
title: HTCondor - job scripts 
---
<div class="objectives" markdown="1">

#### Objectives
*   Job script  
*   Job submission  
*   Job control and monitor  
</div>

This is a quick start page which should take only a few minutes to complete.
Login to OSG Connect
If not already registered to OSG Connect, go to the registration site and follow instructions there.
Once registered you are authorized to use login.osgconnect.net (the Condor submit host) and stash.osgconnect.net (the data host), in each case authenticating with your network ID (netid) and password:
$ ssh netid@login.osgconnect.net
Set up the tutorial
You may perform the examples in the tutorial by typing them in from the text below, or by using tutorial files already on login.osgconnect.net. It's your choice; the tutorial is the same either way.
Pretyped setup
To save some typing, you can install the tutorial into your home directory from login.osgconnect.net. This is highly recommended to ensure that you don't encounter transcription errors during the tutorials.
$ tutorial 
usage: tutorial name-of-tutorial 
       tutorial info name-of-tutorial

Available tutorials: 
quickstart     Basic HTCondor job submission tutorial
Now, run the quickstart tutorial:
$ tutorial quickstart 
$ cd ~/osg-quickstart 
Manual setup
Alternatively, if you want the full manual experience, create a new directory for the tutorial work:
$ mkdir osg-quickstart 
$ cd osg-quickstart 
Tutorial jobs
Job 1: A simple, nonparallel job
Create a workload
Inside the tutorial directory that you created or installed previously, let's create a test script to execute as your job:
$ nano short.sh
file: short.sh
#!/bin/bash
# short.sh: a short discovery job
printf "Start time: "; /bin/date
printf "Job is running on node: "; /bin/hostname
printf "Job running as user: "; /usr/bin/id
printf "Job is running in directory: "; /bin/pwd
echo
echo "Working hard..."
sleep ${1-15}
echo "Science complete!"
Icon
To close nano, hold down Ctrl and press X. Press Y to save, and then Enter
Now, make the script executable.
$ chmod +x short.sh 
If you used the tutorial command, all files are already in your workspace.
Run the job locally
When setting up a new job type, it's important to test your job outside of Condor before submitting into the grid.
$ ./short.sh
Start time: Wed Aug 21 09:21:35 CDT 2013
Job is running on node: login01.osgconnect.net
Job running as user: uid=54161(netid) gid=1000(users) groups=1000(users),0(root),1001(osg-connect),1002(osg-staff),1003(osg-connect-test),9948(staff),19012(osgconnect)
Job is running in directory: /home/netid/quickstart
Working hard...
Science complete!
Create an HTCondor submit file
So far, so good! Let's create a simple (if verbose) HTCondor submit file.
$ nano tutorial01
file: tutorial01
# The UNIVERSE defines an execution environment. You will almost always use VANILLA. 
Universe = vanilla 

# EXECUTABLE is the program your job will run It's often useful 
# to create a shell script to "wrap" your actual work. 
Executable = short.sh 

# ERROR and OUTPUT are the error and output channels from your job
# that HTCondor returns from the remote host.
Error = job.error
Output = job.output

# The LOG file is where HTCondor places information about your 
# job's status, success, and resource consumption. 
Log = job.log

# +ProjectName is the name of the project reported to the OSG accounting system
+ProjectName="ConnectTrain"

# QUEUE is the "start button" - it launches any jobs that have been 
# specified thus far. 
Queue 1
Choose the Project Name
It is very important to set a project name using the +ProjectName = "project" parameter. A job without a ProjectName will fail with a message like:
No ProjectName ClassAd defined!
Please record your OSG project ID in your submit file.
  Example:  +ProjectName = "OSG-CO1234567"

Based on your username, here is a list of projects you might have 
access to:
ConnectTrain
OSG-Staff
To see the projects you belong to, you can use the command osgconnect_show_projects:
 
Example
sh$ osgconnect_show_projects
Based on username (dgc), here is a list of projects you might have
access to:
con-atlas
con-cmts
con-evotheory
con-staff
ConnectTrain
You can join projects after you login at https://portal.osgconnect.net/ . Within minutes of joining and being approved for a project, you will have access via condor_submit as well. To define a new project, see the ConnectBook section for Principal Investigators.
 
Icon
Note that project names are case sensitive.
You have two ways to set the project name for your jobs:
Add the +ProjectName="MyProject" line to the HTCondor submit file. Remember to quote the project name!
Create in your home directory a file with your default project name: $HOME/.osg_default_project
If you do not set a project name, or you use a project that you're not a member of, then your job submission will fail.
Submit the job
Submit the job using condor_submit.
$ condor_submit tutorial01
Submitting job(s). 
1 job(s) submitted to cluster 823.
Check job status
The condor_q command tells the status of currently running jobs. Generally you will want to limit it to your own jobs:
$ condor_q netid
-- Submitter: login01.osgconnect.net : <128.135.158.173:43606> : login01.osgconnect.net
 ID      OWNER            SUBMITTED     RUN_TIME ST PRI SIZE CMD
 823.0   netid           8/21 09:46   0+00:00:06 R  0   0.0  short.sh
1 jobs; 0 completed, 0 removed, 0 idle, 1 running, 0 held, 0 suspended
Icon
If you want to see all jobs running on the system, use condor_q without any extra parameters.
You can also get status on a specific job cluster:
$ condor_q 823
-- Submitter: login01.osgconnect.net : <128.135.158.173:43606> : login01.osgconnect.net
 ID      OWNER            SUBMITTED     RUN_TIME ST PRI SIZE CMD
 823.0   netid           8/21 09:46   0+00:00:10 R  0   0.0  short.sh
1 jobs; 0 completed, 0 removed, 0 idle, 1 running, 0 held, 0 suspended
Note the ST (state) column. Your job will be in the I state (idle) if it hasn't started yet. If it's currently scheduled and running, it will have state R (running). If it has completed already, it will not appear in condor_q.
Let's wait for your job to finish – that is, for condor_q not to show the job in its output. A useful tool for this is watch – it runs a program repeatedly, letting you see how the output differs at fixed time intervals. Let's submit the job again, and watch condor_q output at two-second intervals:
$ condor_submit tutorial01
Submitting job(s). 
1 job(s) submitted to cluster 824
$ watch -n2 condor_q netid 
... 
When your job has completed, it will disappear from the list.
Icon
To close watch, hold down Ctrl and press C.
Job history
Once your job has finished, you can get information about its execution from the condor_history command:
$ condor_history 823
 ID      OWNER            SUBMITTED     RUN_TIME ST   COMPLETED CMD
 823.0   netid            8/21 09:46   0+00:00:12 C   8/21 09:46 /home/netid/
Icon
You can see much more information about your job's final status using the -long option.
Check the job output
Once your job has finished, you can look at the files that HTCondor has returned to the working directory. If everything was successful, it should have returned:
a log file from Condor for the job cluster: jog.log
an output file for each job's output: job.output
an error file for each job's errors: job.error
Read the output file. It should be something like this:
$ cat job.output
Start time: Wed Aug 21 09:46:38 CDT 2013
Job is running on node: appcloud01
Job running as user: uid=58704(osg) gid=58704(osg) groups=58704(osg)
Job is running in directory: /var/lib/condor/execute/dir_2120
Sleeping for 10 seconds...
Et voila!
Job 2: Submitting jobs concurrently
What do we need to do to submit several jobs simultaneously? In the first example, Condor returned three files: out, error, and log. If we want to submit several jobs, we need to track these three files for each job. An easy way to do this is to add the $(Cluster) and $(Process) macros to the HTCondor submit file. Since this can make our working directory really messy with a large number of jobs, let's tell HTCondor to put the files in a directory called log. Here's what the second (less verbose) submit file looks like:
file: tutorial02
Universe = vanilla 
Executable = short.sh 
Error = log/job.error.$(Cluster)-$(Process) 
Output = log/job.output.$(Cluster)-$(Process) 
Log = log/job.log.$(Cluster) 
+ProjectName="ConnectTrain"
Queue 10 
Before submitting, we also need to make sure the log directory exists.
$ mkdir -p log
You'll see something like the following upon submission:
$ condor_submit tutorial02
Submitting job(s)..........
10 job(s) submitted to cluster 837.
Job 3: Passing arguments to executables
Sometimes it's useful to pass arguments to your executable from your submit file. For example, you might want to use the same job script for more than one run, varying only the parameters. You can do that by adding {Arguments to your submission file. Let's try that with tutorial03.
We want to run many more instances for this example: 100 instead of only 10. To ensure that we don't collectively overwhelm the scheduler let's also dial down our sleep time from 15 seconds to 5.
file: tutorial03
Universe = vanilla 
Executable = short.sh 
Arguments = 5 # to sleep 5 seconds 
Error = log/job.err.$(Cluster)-$(Process) 
Output = log/job.out.$(Cluster)-$(Process) 
Log = log/job.log.$(Cluster) 
+ProjectName="ConnectTrain"
Queue 10
And let's submit:
$ condor_submit tutorial03
Submitting job(s)....................................................................................................
10 job(s) submitted to cluster 938. 
Where did jobs run?
When we start submitting many simultaneous jobs into the queue, it might be worth looking at where they run. To get that information, we'll use a couple of condor_history commands. First, run condor_history -long jobid for your first job. Again the output is quite long:
$ condor_history -long 938

MaxHosts = 1
MemoryUsage = ( ( ResidentSetSize + 1023 ) / 1024 )
JobCurrentStartTransferOutputDate = 1377112243
User = "netid@login01.osgconnect.net"
... 
Looking through here for a hostname, we can see that the parameter that we want to know is LastRemoteHost. That's what job slot our job ran on. With that detail, we can construct a shell command to get the execution node for each of our 100 jobs, and we can plot the spread. LastRemoteHost normally combines a slot name and a host name, separated by an @ symbol, so we'll use the UNIX cut command to slice off the slot name and look only at hostnames. We'll cut again on the period in the hostname to grab the domain where the job ran.
For illustration, the author has submitted a thousand jobs for a more interesting distribution output.
$ condor_history -format '%s\n' LastRemoteHost 942 | cut -d@ -f2 | distribution --height=100
Val                    |Ct (Pct)     Histogram
[netid@login01 log]$ condor_history -format '%s\n' LastRemoteHost 959 | cut -d@ -f2 | cut -d. -f2,3 | distribution --height=100
Val          |Ct (Pct)     Histogram
mwt2.org     |456 (46.77%) +++++++++++++++++++++++++++++++++++++++++++++++++++++
uchicago.edu |422 (43.28%) +++++++++++++++++++++++++++++++++++++++++++++++++
local        |28 (2.87%)   ++++
t2.ucsd      |23 (2.36%)   +++
phys.uconn   |12 (1.23%)   ++
tusker.hcc   |10 (1.03%)   ++
...
The distribution program reduces a list of hostnames to a set of hostnames with no duplication (much like sort | uniq -c), but additionally plots a distribution histogram on your terminal window. This is nice for seeing how Condor selected your execution endpoints.
 
There is also condor_plot a command that plots similar information in a HTML page. You can have bar plots, pie charts and more. See View Job distributions.
Workload Analysis
Our Cycleserver website is an analytics tool that users should keep at hand while submitting jobs into OSG Connect.
We'll talk more about this in the next tutorial. For now we'll just note the following:
The Cycleserver URL is http://stash.osgconnect.net:8000/home.s
Removing jobs
On occasion, jobs will need to be removed for a variety of reasons (incorrect parameters, errors in submission, etc.). In these instances, the condor_rm command can be used to remove an entire job submission or just particular jobs in a submission. The condor_rm command accepts a cluster id, a job id, or username and will remove an entire cluster of jobs, a single job, or all the jobs belonging to a given user respectively. E.g. if a job submission generates 100 jobs and is assigned a cluster id of 103, then condor_rm 103.0 will remove the first job in the cluster. Likewise, condor_rm 103 will remove all the jobs in the job submission and condor_rm [username] will remove all jobs belonging to the user. The condor_rm documenation has more details on using condor_rm including ways to remove jobs based on other constraints.
Getting help
If anything here didn't work, please email connect-support@opensciencegrid.org.


<h2> Job script </h2> 


<h2> Job submission </h2> 

<h2> Job control and monitor </h2> 

