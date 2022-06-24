##### Table of Contents  
[Headers](#headers)
[Emphasis](#emphasis)  
...snip...    
<a name="headers"/>
<a name="emphasis"/>



# Introduction


# RTL to GDS flow 
At first we will have design specification, which comes from the customer end. Customer writes down the specification of the chip basically the functionality which he wants to develop in a chip. Once we have this specification we convert this into a RTL code.
## Synthesis 
This is the process where we convert RTL code into a circuit. It transforms simple RTL design into a gate-level netlist with all the constraints as specified by the designer.
## Floor Planning 
In floorplanning, e defie the size and shape of chip, place IO pins/pads, macros and blockages in the core or chip area in order to effectively find the rouing space between them. At floorplanning we reserve space for the placement of standard cells. 
## Power Planning
Power planning is stage typically part of the floorplanning stage, in which power grid network is created to distrubute the power uniformly throuhout chip
## Placement
Placement is the process of determining the locations of circuit devices on a die surface.
## Clock Tree Synthesis
Clock Tree Synthesis is a process for distributing the clock equally among all sequential part of a design so that we reduce skew and delay.
## Routing
Making physical connections using metal layers are called Routing. Routing is the stage after CTS and optimization where exact paths for the interconnection of standard cells and macros and I/O pins are determined.
## SignOff
Layout will be ready after routing stage. Some checks we have to perform soon after the completion of layout to check whether our layout works as designed. These checks are known as Signoff checks.

![](test1/github_rtl_to_gds_flow.PNG)

# OpenLANE
The purpose of this lab is to familiarise you with the efabless OpenLane VLSI design flow and the Skywater 130nm PDK. OpenLane is an open-source VLSI flow built around open-source tools. That is, it’s a collection of scripts that run these tools, in the right order, transforming their inputs and outputs as appropriate, and organising the results.


# Direcory structure
Following is the directory structure we have 
![](test1/github0.1.PNG)
List of all the designs present
![](test1/github0.2.PNG)
List of files present in one of the designs
![](test1/github0.3.PNG)
contents of config.tcl file 
![](test1/github0.4.PNG)

# Design preparation
 Once you are in run area run "docker" command
 source  ./flow.tcl in interactive session
![](test1/github1.PNG)

Import all the packages required for flow

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



# Bullet points
1. this is for numbering bullet
* this is for non numbering bullet





