---
layout: lesson
root: ../..
title: Scientific Workflow Management - DAGMAN 
---
<div class="objectives" markdown="1">

#### Objectives
*   Learn what is DAGMAN?
*   Learn how to write a DAGMAN script   
</div>

<h2> Overview </h2> 


In scientific computing, one may have to perform several computational tasks or 
data manipulations that are releted one another in some order. Workflow management 
systems help to deal with such tasks or data manipulations. DAGMan is one of the 
early workflow managment system developed for distributed high throughput 
computing. DAGMan (Directed Acyclic Graph Manager) handles computational jobs 
that are mapped as a directed acyclic graph. In this section, we will learn how to 
apply DAGMan to run a long time scale molecular dynmaics (MD) simulation. 

<h2> Running Long Time Scale MD Simulation with DAGMAN   </h2> 

At present, the recomended calculation time to run jobs on OSG is about 2-3 hours. Jobs
requiring more than 2-3 hours, need to be submitted with the restart files. Manually 
submitting small jobs repeatedly with restart files may not be practical in many 
situations. DAGMan offers an elegant and simple solutions to run set of jobs. With 
DAGMan scripts one could run a long time scale MD simulations of biomolecules. 

As an example, we will break the MD simulation in four steps and run it through the 
DAGMan script. For the sake of simplicity, the simulations are run only for few 
integration steps and they  will not consume much computational time. 

Say we have four MD jobs: *A0*, *A1*, *A2* and *A3*. Some of the output files from the 
job *A0* serves as an input for the job *A1* and so forth.  The input and output 
dependencies of the jobs are such that they need to be linearly progressing 
from *A0-->A1-->A2---A3*. These set of jobs clearly represents an acyclic graph. In 
DAGMan language, job *A0* is parent of job *A1*,  job *A1* is parent of 
*A2* and job *A3* is parent of *A4*. In DAGMan script, this is expressed as 

~~~

######DAG file######    //comment
Job A0 namd_run_dag0.submit  //Job keyword, Job Name, Condor Job submision script.
Job A0 namd_run_dag0.submit  //Job keyword, Job Name, Condor Job submision script.
Job A0 namd_run_dag0.submit  //Job keyword, Job Name, Condor Job submision script.
Job A0 namd_run_dag0.submit  //Job keyword, Job Name, Condor Job submision script.
PARENT A0 CHILD A1  //Inter Dependency between Job A0 and A1
PARENT A1 CHILD A2  //Inter Dependency between Job A1 and A2 
PARENT A2 CHILD A3  //Inter Dependency between Job A2 and A3
~~~

The first four lines after the comment are the listing of the condor jobs  
with names A0, A1, A2 and A3. The next three lines describe the 
relation between the four jobs. The script files namd_run_dag0.submit, 
namd_run_dag1.submit...  are the condor job submit files. 

The above DAGMan script and the neccessary files are available to the user 
by invoking the *tutorial* command. 
~~~
tutorial namd-DAGMan
cd ~/osg-namd-DAGMan
~~~

The directory "~/osg-name-DAGMan" contains the all the neccessary files including, the 
"linear.dag" the DAGMan script, the condor job submission, job executation and input 
files for NAMD.  

Now we submit the DAGMan file on OSG. 

~~~
$ condor_submit_dag linear.dag 

-----------------------------------------------------------------------
File for submitting this DAG to Condor           : linear.dag.condor.sub
Log of DAGMan debugging messages                 : linear.dag.dagman.out
Log of Condor library output                     : linear.dag.lib.out
Log of Condor library error messages             : linear.dag.lib.err
Log of the life of condor_dagman itself          : linear.dag.dagman.log

Submitting job(s).
1 job(s) submitted to cluster 1317501.
-----------------------------------------------------------------------

~~~

We can check the job status, by typing

~~~
$ condor_q username

-- Submitter: login01.osgconnect.net : <192.170.227.195:48781> : login01.osgconnect.net
 ID      OWNER            SUBMITTED     RUN_TIME ST PRI SIZE CMD               
1317646.0   dbala          10/30 17:27   0+00:00:28 R  0   0.3  condor_dagman     
1317647.0   dbala          10/30 17:28   0+00:00:00 I  0   0.0  namd_run_dag0.sh  

2 jobs; 0 completed, 0 removed, 1 idle, 1 running, 0 held, 0 suspended
~~~~

There are two jobs runing. One is the dagman job that executes the shell "namd_run_dag0.sh" that corresponds to the condor job "namd_run_dag0.submit". 


