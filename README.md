# INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK

## CMOS inverter schematic and layout design and analysis utilizing the sky130 pdk and numerous open source tools

This project has only one goal: to experiment with the operation of an inverter and to comprehend all of its parameters. The design will make use of the models included with the skywater 130nm pdk along with numerous open source tools such as Xschem, NGSPICE, MAGIC, Netgen, and so on.

## 1. Tools and PDK setup
### 1.1 Tools setup
For the design and simulation of our Inverter.

Schematic Capture - [Xschem](http://repo.hu/projects/xschem/)\
Terminal Emulator - [Xterm](https://zoomadmin.com/HowToInstall/UbuntuPackage/xterm)\
Spice netlist simulation - [Ngspice](https://ngspice.sourceforge.io/)\
Layout Design and DRC - [Magic](http://opencircuitdesign.com/magic/)\
LVS - [Netgen](http://opencircuitdesign.com/netgen/)


#### 1.1.1 Xschem

![image](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/12aa20e2-623d-4bf3-b22d-55c7e7fa3fae)

[Xschem](http://repo.hu/projects/xschem/xschem_man/xschem_man.html) is a schematic capture program that allows to interactively enter an electronic circuit using a graphical and easy to use interface. When the schematic has been created a circuit netlist can be generated for simulation.


#### 1.1.2 Ngspice

![image](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/ae09b04c-3353-44c2-8f65-2968a84488ab)


[Ngspice](https://ngspice.sourceforge.io/devel.html) is the open source spice simulator for electric and electronic circuits. Ngspice is an open project, there is no closed group of developers.

Ngspice Reference Manual: [Complete reference manual](https://ngspice.sourceforge.io/docs/ngspice-manual.pdf).


#### 1.1.3 Magic

![image](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/34757c88-ccd1-4fe7-a5d4-b18477bdefbe)

[Magic](http://opencircuitdesign.com/magic/) is a VLSI layout tool.


#### 1.1.4 Netgen

![image](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/e4bb4c9c-3ab1-46f6-a10f-e3df6bbfb26c)

Netgen is a tool for comparing netlists, a process known as LVS, which stands for "Layout vs. Schematic". This is an important step in the integrated circuit design flow, ensuring that the geometry that has been laid out matches the expected circuit.

#### 1.2 PDK setup
A process design kit (PDK) is a set of files used within the semiconductor industry to model a fabrication process for the design tools used to design an integrated circuit. The PDK is created by the foundry defining a certain technology variation for their processes. It is then passed to their customers to use in the design process.

The PDK we are going to use for this BGR is Google Skywater-130 (130 nm) PDK.

## Installing all the mentioned tools: 
### [edaBundle_whyRD](https://github.com/rajdeep66/edaBundle_whyRD)

opensource EDA tool for VLSI design : this script is in its initial phase and usefull if you just starting your journey to open source EDA tool , all ./configure command use default configuration and instrtucted to configure as per your requirement afer you get expertise to any specific tool till then please follow these steps to install yosys,xschem,ngspice,magic,netgen,openPDK and sky130nmpdk Note: we are assuming you have the linux environment up


Step 1:copy whyrd_eda_bundle.sh and install_openPDK.sh in your area Step 2: run source whyrd_eda_bundle.sh -->for most case it will run for few hour will ask you occasionally password and finist the process.
But recomended to run it step wise(steps are mensioned in the code itself , and download only the tool you need.
Step3:copy each line at a time of install_openPDK.sh and ensure no error is there. If its erroring out try to debug the reason using google search.

Step 4: To run Xschem with sky130nm PDK do following thing cd to /usr/local/share/pdk/sky130A/libs.tech/xschem folder and type xschem press enter done , or alternatively copy xschemrc file from /usr/local/share/pdk/sky130A/libs.tech/xschem to any of your desire folder and run xschem.
step 5: to run magic with sky13nm pdk type "magic -T sky130A" press enter from any location
step 6: run ngspice,yosys,netgen with simply typeing there name and enter
step 7: detail tutorial are available in tool's official site.

For a step-by-step procedure explanation for the instalations follow this [video](https://www.youtube.com/watch?v=VCuyO7Chvc8&list=PL0E9jhuDlj9r-XIIgx5PPJpogx7ThS5CB&index=1) 

## 2. Analysis of MOSFET Characteristics

- step-1: Making a new directory and invoking Xschemrc file to connect to the sky130 pdk.

```
mkdir INV_TUTORIAL
cp /usr/local/share/pdk/sky130B/libs.tech/xschem/xschemrc .
```
- step-2: we will be working on Xschem which will be invocked using Xterm terminal.

Some types of SPICE simulations provided in Ngspice : 
>AC Analysis -> Mainly used for Analog circuits and it is Frequency dependent
> 
>DC Analysis -> Mainly used for Digital circuits and it is Time independent
>
>Transient Analysis -> Mainly used for Digital circuits and it is Time dependent(DC Analysis + Time dependancy)

### 2.1 General MOS Analysis
This section begins with our examination of the MOSFET models included in the sky130 pdk. I used the 1.8v transistor models, but you can certainly use and explore with the others. The schematic I made in Xschem is shown below.

#### NMOS Schematic

The components used are:\
`nfet_01v8.sym` - from xschem_sky130 library\
`vsource.sym` - from xschem devices library\
`code_shown.sym` - from xschem devices library

I used the above to plot the basic characteristic plots for an NMOS Transistor, That is ***Ids vs Vds*** and ***Ids vs Vgs***. To do that, just save the above circuit with the above mentioned specifications and component placement. After this just hit Netlist then Simulate. ngspice would pop up and start doing the simulation based calculations. It will take time as all the libraries need to be called and attached to the simulation spice engine. Once that is done, you need to write a couple commands in the ngspice terminal:

`display` - This would display all the vectors available for plotting and printing.\
`setplot` - This would list all the set of plots available for this simulation.\
after this choose a plot by typing *'''setplot <plot_name>'''. for example '''setplot tran1'''*\
`plot` - to choose the vector to plot.
example : plot -vds#branch
