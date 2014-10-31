---
layout: lesson
root: ../..
title: Data Storage Solution - Stash 
---
<div class="objectives" markdown="1">

#### Objectives
*   Understand how to utilize *stash*.  
*   Learn the existing tools for  data transfer.    
</div>


<h2> Overview </h2>
We will learn how to transfer data from your laptop to OSG-Connect.  In particular, 
our interest is to deal with big data of more than 100 GB. Say for example, you 
want to do analytics on the data base of Chicago Tribune readers with the OSG-connect 
resources. Doing analytics on big data is efficient when the input data base 
resides on *stash* and not on the OSG login node.  Data on *stash* is quickly 
accessible to the worker nodes.  A detailed description about the mechanism 
of *stash* can be found in the OSG connectbook. Here we will focus on how to 
transfer the files from your laptop to *stash*.  

<h2> Where is stash? </h2> 
Stash is mounted on your account at login.osgconnect.edu.  You can login with 
the  secured shell (ssh) protocol by typing 

~~~
ssh username@login.osgconnect.net //Connect to the remote host with your username
passwd:       // your password
cd ~/data    // This is where the *stash* is mounted for you. You can keep the data here for a quick access by worker machines.
~~~
{:class="in"}

You can make the data on *stash* publically available on the WWW. The data copied or moved to the directory #~/data/public# is available on WWW as 

~~~
http://stash.osgconnect.net/+yourusername.
~~~


<h2> File Transfer to Stash </h2> 

We can transfer files from the local machine to *stash* with scp, rsync, or 
globus. Globus is the most reliable approach for transferring a large amount of data. 
 
To transfer a file called "BigData.tar.gz" via scp from your local laptop to *stash*

~~~
scp BigData.tar.gz username@login.osgconnect.net:~/data/.  //Transfers the file "BigData.tar.gz" using secured copy.
~~~
{:class="in"}

You can do the same transfer via rsync, as

~~~
rsync -v -e ssh BigData.tar.gz username@login.osgconnect.net:~/data/.
~~~
{:class="in"}

In case of broken connection, rsync would start the file transfer from where it is stopped through recursive synchronization. This is an advantage of rsync over scp. 


<h2> Utilizing Stash </h2> 

Let us do an example calculation to understand the use of *stash*. We will peform 
molecular dynamics simulation of a small protien in implicit water. To get the
necessary files, we use the *tutorial* command on OSG. 

Log in OSG
~~~
$ ssh username@login.osgconnect.net
~~~

type 

~~~
$ tutorial namd
$ cd ~/osg-namd-stash
~~~

~~~
namd_run.submit //Condor job submission script file.
namd_run.sh //Job execution script file.
ubq_gbis_eq.conf //Input configuration for NAMD.
ubq.pdb //Input pdb file for NAMD.
ubq.psf //Input file for NAMD.
par_all27_prot_lipid.inp //Parameter file for NAMD.
~~~

The parameter file "par_all27_prot_lipid.inp" required for the NAMD simulations. This 
is a common data file for the MD simulations. It is better to keep the parameter file
in the *stash* for easy access.  

~~~
mv par_all27_prot_lipid.inp ~/data/public/.  //moving the parameter file from the local directory ~/osg-namd-stash  to ~/data/public 
~~~

You can view the parameter file appear on WWW

~~~
http://stash.osgconnect.net/+yourusername
~~~

Now we want the parameter file available on the execution (worker) machine when the 
simulation starts to run. As mentioned early, the data on the *stash* is available to 
the execution machines. This means the execution machine can transfer the data from 
*stash* as a part of the job execution. So we have to script this in the job execution 
script. 

You can see that the job execution script has the following lines. 

~~~
#!/bin/bash  
source /cvmfs/oasis.opensciencegrid.org/osg/modules/lmod/5.6.2/init/bash //sourcing a shell specific file that adds the module command to your environment
module load namd/2.9  //loading the namd module
wget --no-check-certificate http://stash.osgconnect.net/+dbala/Namd_param/par_all27_prot_lipid.inp // Getting the data from the *stash* before executing the NAMD simulation
namd2 ubq_gbis_eq.conf > ubq_gbis_eq.log //Executing the NAMD simulation
~~~

In the above script, the data is transfered from the *stash* using wget command. 

We submit the NAMD job. 

~~~
$ condor_submit namd_run.submit 
~~~


 
<div class="keypoints" markdown="1">

#### Key Points
* Data on *stash* is quickly accessed by the worker machines. 
* *stash* is located at ~/data on login.osgconnect.net. 
* *scp* and *rsync* are useful tools for data transfer.
</div>

