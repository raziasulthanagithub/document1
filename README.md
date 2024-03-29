
# Physical Design Workshop using openlane.

# Table of Contents

- [Introduction.](#introduction)
- [RTL to GDS flow.](#rtl-to-gds-flow)
   - [Synthesis.](#synthesis)
   - [Floor Planning](#floor-planning)
   - [Power Planning](#power-planning)
   - [Placement](#placement)
   - [Clock Tree Synthesis](#clock-tree-synthesis)
   - [Routing](#routing)
   - [SignOff](#signoff)
- [OpenLane](#openlane) 
- [Directory structure](#directory-structure)
- [Design preparation](#design-preparation)
- [Design setup stage](#design-setup-stage)
- [Running Synthesis](#running-synthesis)
  - [Flop ratio](#flop-ratio)
- [Running Floor plan](#running-floor-plan)
- [Few shortcuts for using magic tool](#few-shortcuts-for-using-magic-tool)
- [Running Placement](#running-placement)
- [Runnning git clone](#runnning-git-clone)
- [Extracting spice netlist](#extracting-spice-netlist)
- [Creating LEF for a custom cell using Magic tool](#creating-lef-for-a-custom-cell-using-magic-tool)
- [Adding custom LEF in openlane flow](#adding-custom-lef-in-openlane-flow)
- [Steps that can be done to fix slack at synthesis stage](#steps-that-can-be-done-to-fix-slack-at-synthesis-stage)
- [Steps to run floorplan without errors](#steps-to-run-floorplan-without-errors)
- [Steps to confiure openSTA post synthesis timing analysis](#steps-to-confiure-opensta-post-synthesis-timing-analysis)
- [Running CTS](#running-cts)
- [Running genpdn](#running-genpdn)
- [Running Routing](#running-routing)


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
![](test1/github_openlane_asic_flow.PNG)

# Directory structure
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

# Running Synthesis
**run_synthesis** in interactive session will start synthesizing the respective RTL file. Once it is completed we see message **sythesis was sucessful**. Files related to synthesis are present in **synthesis** folder in logs, reports, results directories.
We get info like chip area, numer of different cells present in design from the yosys.log file

![](test1/github_synth_success.PNG)
 
## Flop ratio
flop ratio is number of of dff and total number of cells in design i.e 1613/ 14876 = 10.8%
![](test1/github_dff.PNG)
![](test1/github_total_cells.PNG)

# Running Floor plan
Following are done in Floor plan
1. Define width and height of core and Die: 
Define the width and height of a core. 
2. Define locations of preplaced cells:
We arrange IP's in a chip at user defined locations, thus they are placed before automated placement and routing. These are called pre-placed cells
3. Placing Decoupling capacitors:
Decoupling capacitors are placed near to preplaced cells so that the POWER signal comes to these cells without any distortion
4. POWER and GROUND Routing:
Power and Ground routing is done throughout the chip inorder to provide the signal without any distortion
5. Pin Placement:
Pins are placed based on the location of cells. So that the distance between them is less. All input ports are placed on one side and output ports are placed on other side.
6. Logic cell placement blockage:
Once pin placement is done we have to make sure none of the cells are placed in this area durig automated placement. Thus we block this area.

**run_floorplan** will perform florplan. 
![](test1/github_floorplan_Sucess.PNG)

Once floorplan is completed you can see results and there wiill be a def file generated in results/floorplan folder.

![](test1/github_die_Area.PNG)

Die area is (660685/1000u)m x (671405/1000)um

We use magic tool to view floorplan layout or def file. Open magic using following command. In this command we use def file that is created after floorplan stage.
![](test1/github9.PNG)

Once we run this command we see following layout
![](test1/github7.PNG)

# Few shortcuts for using magic tool
1. Z-for zooming in
2. ^Z-for Zooming out
3. v- for Zoom Fit
4. S- for selection
5. Once we select an item, we go to tckon window and type **what** It will show info about selected item

![](test1/github_magic_layout_1.PNG)

Visit http://opencircuitdesign.com/magic/index.html for more info on magic tool.


# Running Placement
1.	Bind Netlist with physical cells:
We Select a particular shape for a logic gate from library. 
3.	Placement:
At this stage we have preplaced cells which are placed in floorplan stage. Place the cells based on there input and output locations, based on switching of gates. 
3. Optimize placement:
In this step we decide where all we need buffers and repeaters so that signal reaches 

**run_placement** is used in interactive session to run placement. Once it is completed a new def is created in results/placement directory

using magic command to open def after placement

![](test1/github_magic_placement.PNG)

view of Layout after placement

![](test1/github_placement.PNG)

Zoom in view of layout after placement. Once placement is done we can see more standard cells, more pins compared to def file created after floorplan. Also we have power and ground nets, vias defined in this def which are not present in def created after floorplan.
![](test1/github_placement_zoom.PNG)


# Runnning git clone 
How to build an inverter lef from scratch is described in following github page.
https://github.com/nickson-jose/vsdstdcelldesign

![](test1/github_gitclone.PNG)

magic command for viewing inverter layout

![](test1/github_magic_lauout_inverter.PNG)

Inverter Layout viewed using magic
![](test1/github_magic_lauout_inverter1.PNG)


Select any item on layout and then do **what** It wil give info about item. For eg: in below snapshot we can see nwell is selected.
![](test1/github_magic_lauout_inverter2.PNG)

# Extracting spice netlist 

Once we have layout we can extract netlist using following commands, and then convert it to spice. 

1.extract all

extract command incrementally extracts the root cell and all its children into separate .ext files.

2. ext2spice cthresh 0 rthresh 0 

ext2spice command converts the hierarchical extracted netlist information produced by the extract command in a series of .ext files into a flattened representation in SPICE format, used for detailed analog simulation.

**rthresh [value]**
Set resistance threshold value. Lumped resistances below this value will not be written to the output. The value is in ohms, or may be the keyword infinite to prohibit writing any lumped resistances to the output.

**cthresh [value]**
Set capacitance threshold value. The value is in femtofarads, or may be the keyword infinite to prohibit writing any parasitic capacitances to the output.


3. ext2spice

![](test1/github_extract_netlist.PNG)

![](test1/github_extract_netlist1.PNG)

Spice file that is created. 
![](test1/github_spice_file.PNG)

After generating spice file we include libs, voltage sources and then perform transient analysis. Following is the output of transient analysis. 
![](test1/github_spice_simulation_result.PNG)

![](test1/github_trans_graph.PNG)

# Creating LEF for a custom cell using Magic tool 

Once we open respective mag file use command **lef write** to create LEF file 

![](test1/github_creating_lef2.PNG)

# Adding custom LEF in openlane flow

Do following changes in config file to add custom lef file 

![](test1/github_include_new_cell1.PNG)

Add following commands in openlane flow to make sure that we are adding custom lef

    set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
  
    add_lefs -src $lefs

![](test1/github_openlane_commands_to_add_lef.PNG)

run_synthesis and check whther our custom cell is added or not. Following snapshot says 1554 instances of this cell are added. 

![](test1/instances_of_custom_cell.PNG)

zoomed snapshot of def file showing instances of custom cell. 

![](test1/github_magic_layout_after_including_cell.PNG)

# Steps that can be done to fix slack at synthesis stage
SYNTH_STRATEGY is the variable we can change which tries to optimize between Area and Slack. 
SYNTH_SIZING which enables cell sizing


when SYNTH_STRATEGY AREA 0 and SYNTH_SIZING 0

![](test1/area1.PNG)

![](test1/slack1.PNG)

when SYNTH_STRATEGY AREA 1 and SYNTH_SIZING 1 

![](test1/area2.PNG)

![](test1/slack2.PNG)

# Steps to run floorplan without errors

   init_floorplan
   
   place_io
   
   global_placement_or
   
   tap_decap_or
   
   
# Steps to confiure openSTA post synthesis timing analysis

![](test1/pre_sta.PNG)

![](test1/my_base_sdc.PNG) 

use command sta pre_sta.conf then we will see slack similar to what had obtained in run_synthesis. Based on the report we can understand what is causing this slack. 
We can reduce slack by chaging maximun fanout by variable SYNTH_MAX_FANOUT. 
We can reduce slack by upsizing buffer 

slack after several such iterations has reduced to following

![](test1/reduced_slack.PNG)

# Running CTS

use **run_cts** command in openlane 
In cts stage buffers get added, netlist will be modified. So a new <design_name_cts>.v file is seen in results/syntesis folder which has clock buffers now. raed_liberty 

# Running genpdn
**gen_pdn** will power distribution network. 

![](test1/github_genpdn.PNG)


![](test1/gen_pdn_output.PNG)


![](test1/github_stdrails.PNG)


# Running Routing
**run_routing** command is used for routing purpose. 

![](test1/routing_output.PNG)

![](test1/routing_output1.PNG)

viewing layout through magic after Routing is done

![](test1/magic_after_routing.PNG)

![](test1/zoomed_magic_after_routing.PNG)










