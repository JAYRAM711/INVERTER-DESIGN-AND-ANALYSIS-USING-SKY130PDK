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

[Netgen](http://opencircuitdesign.com/netgen/) is a tool for comparing netlists, a process known as LVS, which stands for "Layout vs. Schematic". This is an important step in the integrated circuit design flow, ensuring that the geometry that has been laid out matches the expected circuit.

#### 1.2 PDK setup
A process design kit (PDK) is a set of files used within the semiconductor industry to model a fabrication process for the design tools used to design an integrated circuit. The PDK is created by the foundry defining a certain technology variation for their processes. It is then passed to their customers to use in the design process.

The PDK we are going to use for this BGR is Google Skywater-130 (130 nm) PDK.
![image](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/2022c249-f797-4a6b-a4ab-738fb96001a6)


## 1.3 Installing all the mentioned tools: 
### [edaBundle_whyRD](https://github.com/rajdeep66/edaBundle_whyRD)

opensource EDA tool for VLSI design : this script is in its initial phase and usefull if you just starting your journey to open source EDA tool , all ./configure command use default configuration and instrtucted to configure as per your requirement afer you get expertise to any specific tool till then please follow these steps to install yosys,xschem,ngspice,magic,netgen,openPDK and sky130nmpdk Note: we are assuming you have the linux environment up


Step 1:copy whyrd_eda_bundle.sh and install_openPDK.sh in your area Step 2: run source whyrd_eda_bundle.sh -->for most case it will run for few hour will ask you occasionally password and finist the process.
But recomended to run it step wise(steps are mensioned in the code itself , and download only the tool you need.
Step3:copy each line at a time of install_openPDK.sh and ensure no error is there. If its erroring out try to debug the reason using google search.

Step 4: To run Xschem with sky130nm PDK do following thing cd to /usr/local/share/pdk/sky130A/libs.tech/xschem folder and type xschem press enter done , or alternatively copy xschemrc file from /usr/local/share/pdk/sky130A/libs.tech/xschem to any of your desire folder and run xschem.
step 5: to run magic with sky13nm pdk type "magic -T sky130A" press enter from any location
step 6: run ngspice,yosys,netgen with simply typeing there name and enter
step 7: detail tutorial are available in tool's official site.

***For a step-by-step procedure explanation for the installations follow this [video](https://www.youtube.com/watch?v=VCuyO7Chvc8&list=PL0E9jhuDlj9r-XIIgx5PPJpogx7ThS5CB&index=1)***\

---

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

#### 2.1.1 NMOS SCHEMARTIC
![1 NMOS CHARACTERISTICS](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/597b9758-3f74-41e6-8924-af729531de60)

The components used are:\
`nfet_01v8.sym` - from xschem_sky130 library\
`vsource.sym` - from xschem devices library\
`code_shown.sym` - from xschem devices library\
`gnd` - from xschem devices library\
`lab_pin` - from xschem devices library\
`code_shown` - from xschem devices library

> .LIB -> include a library

I used the above to plot the basic characteristic plots for an NMOS Transistor, That is ***Ids vs Vds*** and ***Ids vs Vgs***. To do that, just save the above circuit with the above mentioned specifications and component placement. After this just hit Netlist then Simulate. ngspice would pop up and start doing the simulation based calculations. It will take time as all the libraries need to be called and attached to the simulation spice engine. Once that is done, you need to write a couple commands in the ngspice terminal:

`display` - This would display all the vectors available for plotting and printing.\
`setplot` - This would list all the set of plots available for this simulation.\
after this choose a plot by typing *'''setplot <plot_name>'''. for example '''setplot tran1'''*\
`plot` - to choose the vector to plot.
example : plot -vds#branch

#### 2.1.2 NMOS Characteristics
using `plot -vds#branch`
- Ids vs Vds
> Code_shown window\
".lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt\
.dc Vgs 0 1.8 .1m Vds 0 2 .3\
.save all \
.end"
> ![NMOS_IDS VS VDS](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/ebc2eff8-d4c0-493d-800d-55d7b361fb92)



- Ids vs Vgs
> Code_shown window\
".lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt\
.dc Vds 0 1.8 .1m Vgs 0 2 .3\
.save all \
.end"
> ![NMOS_IDS VS VGS](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/30c35b35-39dc-4b2f-8dd5-16cc3cb1251e)


#### 2.2.1 PMOS SCHEMARTIC
![PMOS CHARACTERISTICS](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/3712f495-db17-4d29-860a-f1af38d47d57)
*Repeat the same steps similar to that of NMOS Characteristics*

#### 2.2.2 PMOS Characteristics
using `plot -vds#branch`
- Ids vs Vds
> Code_shown window\
".lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt\
.dc Vgs 0 1.8 .1m Vds 0 2 .3\
.save all \
.end"
> ![PMOS_IDS_VDS](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/f155b6bb-fc64-471e-99d4-0019af639988)


- Ids vs Vgs
> Code_shown window\
".lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt\
.dc Vds 0 1.8 .1m Vgs 0 2 .3\
.save all \
.end"
> ![PMOS_IDS_VGS](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/81bfec6e-a5b1-4d9e-a622-d5e64f197d0b)
--- 

## 3. CMOS Inverter Design and Analysis

Before I begin with the CMOS inverter, I believe it is important to define an inverter. An *inverter* is a device that inverts. It is commonly explained in electronics as something that executes NOT logic, that is, something that complements the input. As a result, a HIGH(1.8V) becomes a LOW(0V) and vice versa. The output should ideally follow the input, with no delay or circuit propagation difficulties. In fact, though, an inverter can be a true piece of work. It can have several isseus, such as how rapidly it can react to changes in the input, how much load it can withstand before its output breaks, and many more, such as noise, delay, and so on.

All these parameters are what will always plague any analog design or design with transistor in general. Hence, with inverter many like to explore them all. So it justifies why Inverter is referred to as **Hello World! of transistor level design**

I first start with a schematic diagram, and then found the output for that particular design and then Designed a symbol using the schematic diagram and then designed a testbench circuit that is capable of evaluating all the parameters(like noise, delay and power), by measuring them, experimentin with them and reaching a conclusive value and finding the reasons for there issues in the above parameters.

#### 3.1.1 Inverter Schematic diagram

![1 INVERTER SCHEMATIC](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/a5eb939d-4248-4df4-ac1c-d2438751776d)

#### 3.1.2 Inverter output
![3 INVERTER SCHEMATIC OUTPUT](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/b849c3ee-752e-4ec0-bb1d-40caf98cbdff)

![4 SPIKES DUE TO PARASATIC CAPACITANCE BW INP   OUT](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/650f33ad-07d1-48f3-8e70-003192a1a95e)


#### 3.1.3 Inverter symbol
![2 SYMBOL](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/9db11317-19c5-450c-953f-c56a7acfe892)

#### 3.1.3 Inverter Testbench 
![3 INEVRTER TESTBENCH](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/7dfe04c4-bd7f-4ad5-adfd-458af3015d51)


## 4.VTC curve

![4 INVERETR OUTPUT WHEN PMOS WIDTH=1](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/ff468dac-9840-453a-b4f3-c420a089534d)

![5 INVERETR OUTPUT WHEN PMOS WIDTH=2](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/740e750b-a9a8-41bb-a2b4-35cef020620c)

![6 INVERETR OUTPUT WHEN PMOS WIDTH=3](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/ed87ad29-5431-4ab8-b86e-074a46b06bba)
