# ncsu-landis-ii-hpc

# Steps for LANDIS-II on NCSU HPC (Henry2)
***Note***: at the time of generating this readme, NCSU is in the process of migrating to new cluster: Hazel. Some steps may differ.
# 0. Get Access to HPC
a) ***Faculty*** must create project OR add you to existing project.   
b) Credentials become active @ 6:30pm the day you're added.   
c) **Supposedly** need computer with approved IP address to access HPC.   
d) From approved machine, access login node. Tons of HPC resources, [here](https://hpc.ncsu.edu/main.php).  
e) Confirm access to OR request access to a /usr/local/usrapps/[hpcprojectnamefolder]  
  * This is where your singularity image (.sif) will be stored.  
# 1. Build a Singularity Image (.sif)
a) An .sdef file is used to build the .sif. The [.sdef used]() is stored in this repo.
b) Building the .sif from .sdef must occur on LINUX machine with access to /root.   
c) To do this, Vagrant (virtual machine) was installed and used on GIS-lab Windows machine. (Reasoning for using virtual machine on GIS-lab comp - see 0.c.)
* Steps for using Vagrant, [here](https://sloopstash.com/blog/how-to-build-vm-on-windows-10-using-virtualbox-vagrant-git-bash.html).
* Went this route to assure Windows machine (once image built on virtual machine transferred to host Windows machine) would connect to HPC. 
d) Once Vangrant installed, need to build virtual LINUX on host (Windows/Mac) machine. 

 

# 2. Put Singularity Image on HPC

# 3. Put LANDIS-II Data on HPC

# 4. Run LANDIS-II on HPC

# 5. Outstanding Questions

