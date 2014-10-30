---
layout: lesson
root: ../..
title: Introduction to Open Science Grid 
---
<div class="objectives" markdown="1">

#### Objectives
*   Get to know what is Open Science Grid
*   What resources are open to acadamic researchers
*   Computation that is a good match for OSG Connect
*   Computation that is NOT a good match for OSG Connect

</div>

## Introduction to Open Science Grid (OSG)  

The Open Science Grid (OSG) is a consortium of research communities who promote 
science via sharing of computing resources. We enable a framework of distributed 
computing and storage resources, a set of services and methods that enable better 
access to ever increasing computing resources for researchers and communities, 
and principles and software that enable distributed high thoroughput 
computing (DHTC) for users and communities at all scales.

## Computation that is a good match for OSG Connect 

High throughput workflows with simple system and data dependencies are a good 
fit for OSG Connect. Typically these workflows can be decomposed into multiple
tasks that can be carried out independently.  Ideally, these tasks will download 
data for input, run some computation on it and then return results (which may be 
used by future tasks).

Jobs submitted into the OSG Connect will be executed on machines at several 
remote physical clusters. These machines may differ in terms of computing 
environment from the submit node. Therefore it is important that the jobs are 
as self-contained as possible by generic binaries and data that can be either 
carried with the job, or staged on demand. Please consider the following 
guidelines:
<ul>
<li>   Software should preferably be single threaded, using less than 2 GB memory and 
    each invocation should run for 1-12 hours (optimally under 4 hours). There is 
    some support for jobs with longer run time, more memory or multi-threaded codes. 
    Please contact the support listed below for more information about these 
    capabilities.</li>
<li>   Only core utilities can be expected on the remote end. There are no standard 
    versions of software such as 'gcc', 'python', 'BLAS' or others on the grid. 
    Consider using Distributed Environment Modules to manage software dependencies, 
    or read our Developing High-Throughput Applications guide.</li>
<li>   Input and output data for each job should be < 10 GB to allow them to be 
    transferred in by the jobs, processed and returned to the submit node. Note 
    that the OSG Connect Virtual Cluster does not have a global shared file 
    system, so jobs with such dependencies will not work.</li>
<li>   No shared filesystem. Jobs must transfer all executables, input data, and 
    output data. HTCondor can transfer the files for you, but you will have to 
    identify and list the files in your HTCondor job description file. </li>
</ul>

## Computation that is NOT a good match for OSG Connect 

The following are examples of computations that are NOT good matches for 
OSG Connect:
<ul>
<li>   Tightly coupled computations, for example MPI based communication, will 
    not work well on OSG Connect due to the distributed nature of the infrastructure.</li>
<li>   Computations requiring a shared file system will not work, as there is 
    no shared filesystem between the different clusters on OSG Connect.</li>
<li>   Computations requiring complex software deployments or proprietary software 
    are not a good fit.  There is limited support for distributing software to 
    the compute clusters, but for complex software, or licensed software, 
    deployment can be a major task.</li>
</ul>
How to get help using OSG Connect
Please contact user support staff at [connect-support@uchicago.edu][connect-support@uchicago.edu].

