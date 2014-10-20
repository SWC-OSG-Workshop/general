---
layout: lesson
root: ../..
title: Introduction to Open Science Grid 
---
<div class="objectives" markdown="1">

#### Objectives
*   What is Open Science Grid?
*   What resources are open to acadamic researchers?
*   Computation that is a good match for OSG Connect
*   Learn how to use your SSH key

</div>

<h2> Introduction to Open Science Grid (OSG)  </h2> 



<h2> Computation that is a good match for OSG Connect </h2> 

High throughput workflows with simple system and data dependencies are a good fit for OSG Connect. The HTCondor manual has an overview of high throughput computing.
Jobs submitted into the OSG Connect will be executed on machines at several remote physical clusters. These machines may differ in terms of computing environment from the submit node. Therefore it is important that the jobs are as self-contained as possible by generic binaries and data that can be either carried with the job, or staged on demand. Please consider the following guidelines:
Software should preferably be single threaded, using less than 2 GB memory and each invocation should run for 1-12 hours. There is some support for jobs with longer run time, more memory or multi-threaded codes. Please contact the support listed below for more information about these capabilities.
Only core utilities can be expected on the remote end. There are no standard versions of software such as 'gcc', 'python', 'libblas' or others on the grid. Consider using Distributed Environment Modules to manage software dependencies, or read our Developing High-Throughput Applications guide.
Input and output data for each job should be < 10 GB to allow them to be pulled in by the jobs, processed and pushed back to the submit node. Note that the OSG Connect Virtual Cluster does not currently have a global shared file system, so jobs with such dependencies will not work.
No shared filesystem. Jobs must transfer all executables, input data, and output data. HTCondor can transfer the files for you, but you will have to identify and list the files in your HTCondor job description file.


<h2> Computation that is NOT a good match for OSG Connect </h2> 

The following are examples of computations that are NOT good matches for OSG Connect:
Tightly coupled computations, for example MPI based communication, will not work well on OSG Connect due to the distributed nature of the infrastructure.
Computations requiring a shared file system will not work, as there is no shared filesystem between the different clusters on OSG Connect.
Computations requiring complex software deployments are not a good fit. There is limited support for distributing software to the compute clusters, but for complex software, or licensed software, deployment can be a major task.
How to get help using OSG Connect
Please contact user support staff at email connect-support@uchicago.eduuchicago.edu. 

