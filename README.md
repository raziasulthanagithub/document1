##### Table of Contents  
[Headers](#headers)
[Emphasis](#emphasis)  
...snip...    
<a name="headers"/>
<a name="emphasis"/>
## Introduction


# Introduction
# RTL to GDS flow 
![](test1/githb_rtl_to_gds.PNG)
# Bullet points
1. this is for numbering bullet
* this is for non numbering bullet
# Direcory structure
![](test1/github0.1.PNG)
List of all the designs present
![](test1/github0.2.PNG)
List of files present in one of the designs
![](test1/github0.3.PNG)
contents of config.tcl file 
![](test1/github0.4.PNG)

# Design preparation
* Once you are in run "docker"
* source  ./flow.tcl in interactive session
![](test1/github1.PNG)

* Import all the packages required for flow

![](test1/github2.PNG)

# Design setup stage
**prep design <design_name>** 
![](test1/github3.PNG)

In tihs step lefs are merged and **runs** folder is created
![](test1/github4.PNG)
Directory structure inside **runs** 
![](test1/github5.PNG)
**config.tcl** file which tells us about all the environment variables settings used for this design
![](test1/github6.PNG)

# Synthesis
**run_synthesis** will start synthesizing the respective RTL file. Once it is completed we see message **sythesis was sucessful**
* Files related to synthesis are present in **synthesis** folder in logs, reports, results directories.
##Getting Flop ratio
flop ratio is n. of dff/ no. cells = 1613/ 14876 = 10.8%
Add images of dff, total cells and synthesesis sucessfull
# Floor plan
## Utilization and aspect ratio
**run_floorplan** will perform florplan. Once floorplan is completed you can see results and there wiill be a def file generated. 
add def area and image
we use magic to view floorplan layout or def file. Open magic using following command
## usage of magic tool
Z-for zooming 
S- for selection
what in tckon window will show info about selected item
add images of layout 





