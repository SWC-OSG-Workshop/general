---
layout: lesson
root: ../..
title:Data Storage Solution - Stash 
---
<div class="objectives" markdown="1">

#### Objectives
*   Understand how to utilize stash  
</div>

Overview
We will learn how to transfer data from Midway (or your local desktop) to CI-Connect and vice versa. In particular, our interest is to deal with big data of more than 100 GB. Say for example, you want to do analytics on the data base of Chicago Tribune readers with the RCC connect resources. Doing analytics on big data is efficient when the input data base resides on "stash" and not  on Midway login node. Stash is like a scratch space for RCC connect.  Data on stash is quickly accessible to the worker nodes.  A detailed description about the mechanism of stash can be found in the OSG connectbook. Here we will focus on how to transfer the files from Midway to stash.  
Where is stash?
Stash is mounted on your account at login.ci-connect.uchicago.edu.  You can login with the  secured shell (ssh) protocol by typing
ssh username@login.ci-connect.uchicago.edu //Connect to the remote host with your username
passwd:       // your password
cd ~/data    // This is where the stash is mounted for you. You can keep the data here for a quick access by worker machines.
File Transfer to Stash
We can transfer files from the local machine to stash with scp, rsync, or globus. Indeed, globus is the most reliable approach for transferring a large amount of data. 
 
To transfer a file called "BigData.tar.gz" via scp from the midway to stash
scp BigData.tar.gz username@login.ci-connect.uchicago.edu:~/data/.  //Transfers the file "BigData.tar.gz" using secured copy.
You can do the same transfer via rsync, as
rsync -v -e ssh BigData.tar.gz username@login.ci-connect.uchicago.edu:~/data/.
In case of broken connection, rsync would start the file transfer from where it is stopped through recursive synchronization. This is an advantage of rsync over scp. 
 
Globus file transfer is much efficient and highly recommended for file transfer of large amount of data. Go to globus https://www.globus.org/. Click on the "Sign in" tab.  Select the option "InCommon/CILogon". Choose University of Chicago from the list.  You will be redirected to login page where you provide your CNET-ID and password.  Click on the tab "File Tranfer" which takes you to a portal with partitioned double window.  
On the top, you will see the menu for end-points.  The end points need be specified properly. The endpoint for Midway is ucrcc#midway and the endpoint for stash is connect#stash. The directories and files in the endpoints will appear in the listed format. Select the directory or file and click the top arrow for the file transfer. An email will be send to you after the transfer is complete.



<h2> Where is stash? </h2> 

<h2> How to transfer the data to stash </h2> 


