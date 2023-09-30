# INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK

### CMOS inverter schematic and layout design and analysis utilizing the sky130 pdk and numerous open source tools

This project has only one goal: to experiment with the operation of an inverter and to comprehend all of its parameters. The design will make use of the models included with the skywater 130nm pdk along with numerous open source tools such as Xschem, NGSPICE, MAGIC, Netgen, and so on.

1. Tools and PDK setup
1.1 Tools setup
For the design and simulation of our Inverter.

Spice netlist simulation - Ngspice
Layout Design and DRC - Magic
LVS - Netgen
Schematic Capture - Xschem

1.1.1 Ngspice
image

Ngspice is the open source spice simulator for electric and electronic circuits. Ngspice is an open project, there is no closed group of developers.

Ngspice Reference Manual: Complete reference manual.

Steps to install Ngspice -
Don't use the version that comes with linux distribution, since it is dated and sometimes misses crucial updates

Follow this video for just the ngspice installation. DO NOT use this video to install xschem and skywater-pdk.

The above video should be fine but if it does not work for you, Follow the instructions given inside the INSTALL and README file that comes inside the git clone of the repository of ngspice

1.1.2 Magic
image

Magic is a VLSI layout tool.

Steps to install Magic - Follow the instructions on the Opencircuitdesign Install section. I would suggest to use a couple options for the configuration file.

1.1.3 Netgen
image

Netgen is a tool for comparing netlists, a process known as LVS, which stands for "Layout vs. Schematic". This is an important step in the integrated circuit design flow, ensuring that the geometry that has been laid out matches the expected circuit.

Steps to install Netgen - Open the terminal and type the following to insatll Netgen.

$  git clone git://opencircuitdesign.com/netgen
$  cd netgen
$  ./configure
$  sudo make
$  sudo make install 
1.1.4 Xschem
image

Xschem is a schematic capture program that allows to interactively enter an electronic circuit using a graphical and easy to use interface. When the schematic has been created a circuit netlist can be generated for simulation.

Steps to install Xschem Follow the instructions given here.

1.2 PDK setup
A process design kit (PDK) is a set of files used within the semiconductor industry to model a fabrication process for the design tools used to design an integrated circuit. The PDK is created by the foundry defining a certain technology variation for their processes. It is then passed to their customers to use in the design process.

The PDK we are going to use for this BGR is Google Skywater-130 (130 nm) PDK. image

Steps to download PDK - Open the terminal and type the following to download sky130 PDK.

$  git clone https://github.com/RTimothyEdwards/open_pdks.git
$  cd open_pdks
$  ./configure [options]
$  make
$  [sudo] make install
You should follow the instructions given at this link
