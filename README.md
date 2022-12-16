# ncsu-landis-ii-hpc

# Steps for LANDIS-II on NCSU HPC (Henry2)
***Note***: at the time this .readme was created, NCSU is in the process of migrating to new cluster: Hazel. Some steps may differ.
# 0. Get Access to HPC
a) ***Faculty*** must create project OR add you to existing project.   
b) Credentials become active @ 6:30pm the day you're added.   
c) **Supposedly** need computer with approved IP address to access HPC.   
d) From approved machine, access login node. Tons of HPC resources, [here](https://hpc.ncsu.edu/main.php).  
e) Confirm access to OR request access to a /usr/local/usrapps/[hpcprojectnamefolder]  
  * This is where your singularity image (.sif) will be stored.  
# 1. Build a Singularity Image (.sif)
a) An .sdef file is used to build the .sif. The [.sdef used](https://github.com/kejones8/ncsu-landis-ii-hpc/blob/main/working_hpc_landis-ii.sdef) is stored in this repo.   
b) Building the .sif from .sdef requires installing singularity, which must occur on LINUX machine with access to /root.  If on Linux, see instructions, [here](https://docs.sylabs.io/guides/3.5/admin-guide/installation.html#installation-on-linux). Likely jump to 1.g. code section?   
c) To do this, installed singularity on a virtual machine (Hosted on GIS-lab Windows machine. Reasoning for using virtual machine on GIS-lab comp - see 0.c.).  
* Steps for installing singularity on virtual machine, [here](https://docs.sylabs.io/guides/3.5/admin-guide/installation.html#singularity-vagrant-box).    

d) This builds the virtual machine (Vagrant) in whatever directory you 'mkdir vm-singularity'.  
e) Once virtual machine created, confirm singularity installed. (Terminal should be in vagrant@vagrant)  

```
#confirm singularity installed 
singularity version

#might need to run these commands for next steps
sudo apt-get update
sudo apt install vagrant
```
* If you exit Vagrant, to return, open up local machine cmd:

 ```
 cd [pathto]/vm-singularity
 vagrant up
 vangrant ssh
 ```
   
f) When first enter vagrant@vagrant, directory is /home/vagrant. You can pwd & cd around to explore. 
```
cd ..
cd .. 
cd vagrant 
ls
#vagrant is the folder on the virtual machine that "touches" your vm-singularity directory on your local machine

```
g) On local machine, make sure .sdef is in the /vagrant folder (not /home/vagrant) so it can be seen by virtual machine.

h) Confirm .sdef urls/subsequent .sdef commands are in correct format. 

Then: 

```
sudo singularity build [whateveryouwanttonameyoursif].sif [thenameofyoursdef].sdef

```
i) Print outs of the .sif build process will show up in the terminal. If build is successful, [whateveryouwanttonameyoursif].sif will be in /vm-singularity directory on local machine (& in /vagrant directory in VM). 
 
# 1.5 Troubleshooting .sif build  
a) Extensions (beyond NECN 6.9 & SCRPPLE 3.2 that Kate already modified) WILL require modifying the /src/[extensionname].csproj. [LANDIS-II-Foundation/Core-Model-v7-LINUX](https://github.com/LANDIS-II-Foundation/Core-Model-v7-LINUX) has instructions for how to [modify each extension](https://github.com/LANDIS-II-Foundation/Core-Model-v7-LINUX#landis-ii-v7-extensions).  
b) The extension release .zip url that is listed in the .sdef ***should*** pull down the most recent version of the extension. This will be dependent on whether or not the extension repository releases are up to date. If not, should ask the repository owner to make a new release.

# 2. Put Singularity Image on HPC
With a successfully built image/.sif that has all necessary LANDIS-ii extensions for running your model, need to put it up on the HPC. 

# 3. Put LANDIS-II Data on HPC

# 4. Run LANDIS-II on HPC

# 5. Outstanding Questions

a) Building. sif & calling R & LANDIS-ii interactively on HPC
b) How to save output LANDIS-ii data to different fodlers than inputs
* Does Sam's method work on HPC?
c) Releases of LANDIS-ii extensions NEED to be in sync with most updated versions of extensions. 

