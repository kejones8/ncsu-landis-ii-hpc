# ncsu-landis-ii-hpc

## [Google drive folder with](https://drive.google.com/drive/u/0/folders/1O3R2hk0_2052-l0Vql-mpotOMDTtdu23) 
1) example LANDIS-ii input data
2) successfully built and run .sif
3) .sdef used to build the .sif

# Steps for LANDIS-II on NCSU HPC (Henry2)
***Note***: at the time this .README was created, NCSU is in the process of migrating to a new cluster: Hazel. Some steps may differ.

# 0. Get Access to HPC
1. ***Faculty*** must create project OR add you to existing project.   
    * Credentials become active @ 6:30pm the day you're added.   
    * **Supposedly** need computer with approved IP address to access HPC.   
2.  From approved machine, access login node. Tons of HPC resources, [here](https://hpc.ncsu.edu/main.php).  

3.  Confirm access to OR request access to a /usr/local/usrapps/[hpcprojectnamefolder]  
    * This is where your singularity image (.sif) will be stored.  
  
# 1. Build a Singularity Image (.sif)

If intersted in only NECN 6.9 & SCRPPLE 3.2, the image in the google drive folder should work. Otherwise, steps below to help build your own image with customized extensions. In the google drive folder, the .sdef provided was used to build the .sif.

## Getting Singularity Installed & .sdef formatted
a) An .sdef file is used to build the .sif.

b) Building the .sif from an .sdef requires installing singularity, which must occur on a LINUX machine with access to /root. Options for building a singularity image on local Windows/Mac and Linux below. 

### Local Machine - Windows/Mac (requires virtual machine set up)

a) To do this, singularity needs to be installed on a virtual LINUX machine (virtual machine hosted on GIS-lab Windows machine. Reasoning for using virtual machine on GIS-lab comp - see 0.c.). 

* Steps for installing singularity on virtual machine, [here](https://docs.sylabs.io/guides/3.5/admin-guide/installation.html#singularity-vagrant-box).

b) The steps above build the virtual machine (Vagrant) in whatever directory you 'mkdir vm-singularity'.

c) Once virtual machine created, confirm singularity is installed. (Terminal should be in vagrant@vagrant)  

```
#confirm singularity installed 
singularity version

#might need to run these commands for next steps
sudo apt-get update
sudo apt install vagrant
```

* If you exit Vagrant (VM), to return, open up local machine cmd:

 ```
 cd [pathto]/vm-singularity
 vagrant up
 vangrant ssh
 ```
   
d) When first enter vagrant@vagrant, directory is /home/vagrant. You can pwd & cd around to explore. 
```
cd ..
cd .. 
cd vagrant 
ls
#vagrant is the folder on the virtual machine that "touches" your vm-singularity directory on your local machine

```
e) On local machine, make sure .sdef is in the /vagrant folder (not /home/vagrant) so it can be seen by virtual machine.

f) Confirm .sdef is formatted correctly (urls point to correct LANDIS-ii extensions, building of extensions references correct libraries, etc.). 

* This is a "small" step in the instructions but is likely where you will spend a great deal of time customizing your LANDIS-ii build to have the necessary extensions. This is the equivalent of downloading and installing extensions, just with a few more commands of how to unzip & build the extensions. See 1.5 for troubleshooting information.


### Local Machine - Linux

See instructions, [here](https://docs.sylabs.io/guides/3.5/admin-guide/installation.html#installation-on-linux). 

a) Confirm .sdef is formatted correctly (urls point to correct LANDIS-ii extensions, building of extensions references correct libraries, etc.). 

* This is a "small" step in the instructions but is likely where you will spend a great deal of time customizing your LANDIS-ii build to have the necessary extensions. This is the equivalent of downloading and installing extensions, just with a few more commands of how to unzip & build the extensions. See 1.5 for troubleshooting information.


## Code to Build Image

```
sudo singularity build [whateveryouwanttonameyoursif].sif [thenameofyoursdef].sdef

```
* Print outs of the .sif build process will show up in the terminal. If build is successful, [whateveryouwanttonameyoursif].sif will be in /vm-singularity directory on local machine (& in /vagrant directory on VM). 
 
# 1.5 Troubleshooting .sif build  
a) Extensions (beyond NECN 6.9 & SCRPPLE 3.2 that Kate already modified) WILL require modifying the /src/[extensionname].csproj. [LANDIS-II-Foundation/Core-Model-v7-LINUX](https://github.com/LANDIS-II-Foundation/Core-Model-v7-LINUX) has instructions for how to [modify each extension](https://github.com/LANDIS-II-Foundation/Core-Model-v7-LINUX#landis-ii-v7-extensions).  

b) The extension release .zip url that is listed in the .sdef ***should*** pull down an extension release that matches the version of the extension you intend to pull down. This will be dependent on whether or not the extension repository releases are up to date - if releases have not kept current with extension updates, ask repository owner for more information.

The urls should point to the extension releases you want to download:

![extension_urls](https://github.com/kejones8/ncsu-landis-ii-hpc/blob/main/imgs_for_readme/extension_urls.PNG)

In the following section of the .sdef where the extensions are unzipped, make sure decimal places (if needed) are used appropriately after each extension. For example, if only 1 v1.0 is unzipped, no decimal places are necessary. If 2 v1.0 extensions are unzipped, the second should be "v1.0.zip1", and a third "v.1.0zip2".

![unzip_extensions](https://github.com/kejones8/ncsu-landis-ii-hpc/blob/main/imgs_for_readme/unzip_extension.PNG)

If the .sdef fails building a certain extension, make sure: 
* Line 1-4 : the following lines for each extension reflect the version of the extension unzipped (i.e. 6.9)
* Line 1 : reference the correct version of Support-Library_Dlls needed
* Line 2 : include any additional libraries necessary for the extension (i.e. Succession v8)  
* Not pictured: uses the correct climate library versions (if applicable)
* Line 4 : text file (with correct version numbering) exists in /deploy/installer/ in the extension source files  

![extension_examp](https://github.com/kejones8/ncsu-landis-ii-hpc/blob/main/imgs_for_readme/necn_ex.PNG)

# 2. Put Singularity Image on HPC
With a successfully built singularity image (.sif) that has all necessary LANDIS-ii extensions for running your model, need to put it up on the HPC. Log into the HPC from an approved IP address in sftp mode (for file transfer) instead of the normal ssh mode. To do so:  

  a) Open cmd prompt
 
 ```
 sftp [yourunityid]@login.hpc.ncsu.edu
 
 #It will require Duo Dual authentication + your password.
 
 ```
 b) After entering your credentials, you'll be on a login node (this is different than a compute node where you will run the model, more on this later).   
 c) In sftp mode, you have access to your local directories and hpc directories. The following code will place your image on the HPC:  
 
 ```
 pwd #shows /home/[unityid] when first logging on to HPC
 lpwd #shows current working directory on your local machine 
 
 cd .. 
 cd ..
 cd usr/local/usrapps/[hpcprojectnamefolder] #whatever project you have access to in step 0.e.
 ls #see what other files are here
 mkdir [subfolderyoumightwanttomaketostoreyourimage] #not required
 
 lcd [pathtoyourvmsingularityfolder]/vm-singularity
 lls #confirm your .sif is in this folder on your local machine 
 
 put [nameofimageyoubuilt].sif
 
 #a progress bar showing the .sif uploading to the hpc will appear
 
 ls #confirm .sif is in usr/local/usrapps/[hpcprojectnamefolder] on hpc
 
 #if so
 
 exit
 
 ```

# 3. Put LANDIS-II Input Data on HPC

Now that the .sif is stored on the HPC in the usr/local/usrapps where user maintained applications can be accessed, we need to actually put LANDIS-ii input data on the HPC to initiate a model run. You can use sftp (like we did to put the .sif on the HPC), but generally, when transferring larger (or more) files, ***it's best to use Globus (file transfer system).*** 

* Globus must be installed on the local machine (install [here](https://www.globus.org/globus-connect-personal)). 
* Info about accessing Globus using NCSU credentials and connecting to existing HPC directories can be found [here](https://docs.globus.org/how-to/get-started/).

The appropriate directory  to store LANDIS-ii inputs on the HPC is: 

```
/share/[hpcprojectnamefolder]/[unityid]/[whateversubdirectoriesyouwanttomake]
```

***All LANDIS-ii input files should be formatted like the example model data included in the [linked google drive folder](https://drive.google.com/drive/u/0/folders/1O3R2hk0_2052-l0Vql-mpotOMDTtdu23). Specifically, reference the*** 

* scenario_landscape.txt & 
* Sapps_scrapple_V1_5.txt  

***for filepaths (/landis/ needs to be added to all files due to the nature of the .sif build) & any additional spacing/new lines in the text files, etc.***  

# 4. Run LANDIS-II on HPC

Once LANDIS-ii inputs and the .sif are uploaded to the HPC, we can iniate a model run. To run any process on the HPC: 

***YOU HAVE TO BE ON A COMPUTE NODE***

But first, navigate to the folder where you will initiate the model (where you've stored your input data):

```
cd ..
cd ..
cd /share/[hpcprojectnamefolder]/[unityid]/[whateversubdirectoriesyouwanttomake]

#Once in the correct folder, compute nodes can be accessed via method 1 or 2 below.
```

## Method 1
Run LANDIS-ii in serial (not parallel) interavtively, not initatied with a batch script. Code below: 

```
bsub -n 1 -W 240 -q sif -Is -eo err_sing1 tcsh #see below for link to reference argument documentation

```
HPC documentation for individual arguments, [here](https://hpc.ncsu.edu/Documents/LSF.php). But most importantly: 

* -n: number of cores (running jobs in serial - as we are here - always request 1 core
* -W: wall clock time in minutes, your job will be terminated if it exceeds the time limit
* -q: because we're using a singularity image, our runs must go in the .sif queue
* Is: run interactively

After you're on a compute node, modify the text below in [braces] to start a LANDIS-ii simulation: 

```
singularity exec --bind ${PWD}:/landis --home ${PWD}:/home/[unityid] --cleanenv /sr/local/usrapps/[hpcprojectnamefolder]/[nameofimageyoubuilt].sif dotnet /Core-Model-v7-LINUX-7/build/Release/Landis.Console.dll /landis/scenario_landscape.txt

```

## Method 2
Run LANDIS-ii still in serial, but initated via a .csh (batch) script.

```
### INSTRUCTIONS COMING.
```

# 5. Outstanding Questions

a) Building. sif & calling R & LANDIS-ii interactively on HPC  
b) How to save output LANDIS-ii data to different fodlers than inputs  

   * Does Sam's method work on HPC?  
   
c) Releases of LANDIS-ii extensions NEED to be in sync with most updated versions of extensions.   

