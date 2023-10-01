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

***For a step-by-step procedure explanation for the installations follow this [video](https://www.youtube.com/watch?v=VCuyO7Chvc8&list=PL0E9jhuDlj9r-XIIgx5PPJpogx7ThS5CB&index=1)***

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

> .lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt\
> .lib -> include a library

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
![NMOS_IDS VS VDS](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/ebc2eff8-d4c0-493d-800d-55d7b361fb92)



- Ids vs Vgs
> Code_shown window\
".lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt\
.dc Vds 0 1.8 .1m Vgs 0 2 .3\
.save all \
.end"
![NMOS_IDS VS VGS](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/30c35b35-39dc-4b2f-8dd5-16cc3cb1251e)


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
![PMOS_IDS_VDS](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/f155b6bb-fc64-471e-99d4-0019af639988)


- Ids vs Vgs
> Code_shown window\
".lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt\
.dc Vds 0 1.8 .1m Vgs 0 2 .3\
.save all \
.end"
![PMOS_IDS_VGS](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/81bfec6e-a5b1-4d9e-a622-d5e64f197d0b)

---

## 3. CMOS Inverter Design and Analysis

Before I begin with the CMOS inverter, I believe it is important to define an inverter. An *inverter* is a device that inverts. It is commonly explained in electronics as something that executes NOT logic, that is, something that complements the input. As a result, a HIGH(1.8V) becomes a LOW(0V) and vice versa. The output should ideally follow the input, with no delay or circuit propagation difficulties. In fact, though, an inverter can be a true piece of work. It can have several isseus, such as how rapidly it can react to changes in the input, how much load it can withstand before its output breaks, and many more, such as noise, delay, and so on.

All these parameters are what will always plague any analog design or design with transistor in general. Hence, with inverter many like to explore them all. So it justifies why Inverter is referred to as **Hello World! of transistor level design**

I first start with a schematic diagram, and then found the output for that particular design and then Designed a symbol using the schematic diagram and then designed a testbench circuit that is capable of evaluating all the parameters(like noise, delay and power), by measuring them, experimentin with them and reaching a conclusive value and finding the reasons for there issues in the above parameters.

### 3.1.1 Inverter Schematic diagram

So I designed a Schematic of the Inverter, using a PMOS of (W/L) Aspect ratio =(1/0.15) and NMOS of (W/L) Aspect ratio =(1/0.15) where the gates are interconnected and given to Vin(input voltage) and the sources are also interconnected and given to Vout(output voltage) then finally the drains are connected to vdd and gnd respectively.

Also from now on, (W/L) would be mentioned as S or Aspect Ratio Simply will be furture changed for the future analysis(Transient and DC).

![INVERTER SCHEMATIC](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/a5eb939d-4248-4df4-ac1c-d2438751776d)

### 3.1.2 Inverter output

Here you can observe the plotted input and output waveforms. To plot this **transient analysis** has been employed as it performs Time dependent DC analysis.As per the definition we can able to observe that the *output is an inverted version of the input signal.*

To perform transient analysis the following code will be used
> Code_shown window\
".lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt\
.tran 0.1n 100n\
.save all \
.end"

>syntax -> . tran tstep tstop

![INVERTER OUTPUT](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/b849c3ee-752e-4ec0-bb1d-40caf98cbdff)

On observing clearly, you will be able see some spikes on the output waveform. This irregularites i.e spikes are the cause of Parasatic capatitance b/w input and output.

![4 SPIKES DUE TO PARASATIC CAPACITANCE](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/650f33ad-07d1-48f3-8e70-003192a1a95e)


### 3.1.3 Inverter symbol

Then using the schematic diagram we'll be designing a symbol for the CMOS Inverter. The main advantage of this symbol is portability so it can be added in added in any other circuit and operated easily.

![SYMBOL](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/9db11317-19c5-450c-953f-c56a7acfe892)

### 3.1.4 Inverter Testbench 

Then finally, we will be designing the testbench where we place the power supplies. We would use the following testbecnch for future analysis.(Transient and DC)

Here the Vdd is provided with the 1.8V which is the Max voltage supported by the NMOS and PMOS devices then the Vin is provided with 0V which is then varied in the Code_shown window.

> Code_shown window\
".lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt\
.dc vin 0 2 1m\
.save all \
.end"

>syntax -> dc srcnam vstart vstop vincr

![INEVRTER TESTBENCH](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/7dfe04c4-bd7f-4ad5-adfd-458af3015d51)

--- 

## 3.2 DC Analysis of VTC curve

A voltage transfer characteristics paints a plot that shows the behavior of a device when it's input is changed(full swing). It shows what happens to the output as input changes. In our case, for an inverter we can see a plot that is like a square wave(non ideal), that changes it's nature around 0.9(ideally) volts of input which is known as the ***Threshold voltage(Vm)***. Deviating so much from this threshold voltage might cause Noise margin issue. One can say that there are like 3 regions in the VTC curve, the portion where output is high, the place of transistion and the one where the output goes low. But actually there are five regions of operation and they are based on the working of inverter constituents, that is the NMOS and the PMOS transistors with respect to the change in the input potential. which can be observed from the below picture.

![VTC curve](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/ddebedbc-a497-4c05-b775-0547ebaa12b5)

**DC analysis would be used to plot a Voltage Transfer Characteristics (VTC) curve for the circuit**. It will sweep the value of Vin from high to low to determine the working of circuit with respect to different voltage levels in the input. The following plot is observed when simulated :

![INVERETR OUTPUT WHEN PMOS & NMOS WIDTH=1](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/ff468dac-9840-453a-b4f3-c420a089534d)

The above plot is observed when the widths of both PMOS and NMOS are given as 1.  
Here both the Input and output are crossing at a point where the output changes it's nature w.r.to input signal which is known as ***"Threshold voltage"*** represented using ***"Vm"***. Which can be measured using the following code in the NGSPICE:

`meas dc vm when vin=vout`

the above observed Threshold value is `vm=0.838`\
which is near to the ideal value(0.9) but it can be improved by increasing the sizes of the PMOS and NMOS devices.


![INVERETR OUTPUT WHEN PMOS WIDTH=2 & NMOS WIDTH=1](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/740e750b-a9a8-41bb-a2b4-35cef020620c)

The above plot is observed when the widths of PMOS=2 and NMOS=1.\
the observed Threshold value is `vm=0.869` which is very near to the ideal value(0.9).

![INVERETR OUTPUT WHEN PMOS WIDTH=3 & NMOS WIDTH=1](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/ed87ad29-5431-4ab8-b86e-074a46b06bba)

The above plot is observed when the widths of PMOS=3 and NMOS=1.\
the observed `vm=0.893` which is equal to the ideal value(0.9).

**So, from the above observations we can conclude that the Threshold voltage of the CMOS inverter can be varied by improving the widths of the CMOS devices.** 

---

## 3.3 Noise Margin Analysis
Noise margins are defined as the range of values for which the device can work noise free or with high resistance to noise. Noise margin does makes sure that any signal which is logic ‘1’ with finite noise added to it, is still recognized as logic ‘1’ and not logic ‘0’. For example a noise signal maybe 0.5 is added to the logic ‘0’ giving a input 0.5 < 0.9 so the Not gate(inverter) should consider it as logic ‘0’ and should be providing a logic ‘1’ at the output.

So, it is neccessary to find the gain of the output signal at 3 regions.\
As we already know the gain formula = dVout/dVin.

 This can be measured using the following code in the NGSPICE:

`deriv(vout)` 
by using this above code we get the following waveform:

![gain of vout](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/ab099654-3dcc-4dcc-831f-2ca24cae4a82)

The overall noise margin can be simulated using 
```
let gain =  (abs(deriv(vout))) >= 1) * 1.8;
plot gain vout
```

Now our NMOS has S = 1/0.15 and PMOS has S = 2/0.15. Below is it's simulation result using the same testbench.

![Noise margin](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/591c6e14-7ec8-4433-9d40-dde068f6db48)


- VIH - Maximum input voltage that can be interpreted as logic '0'.
- VIL - Minimum input voltage that can be interpreted as logic '1'.
- VOH - Maximum output voltage when it is logic '1'.
- VOL - Minimun output voltage when it is logic '0'.

- To find VIL:
`meas VIL dc find vin when gain=1 cross=1` 

which is given as `VIL= 0.743`.

- To find VIH:
`meas VIH dc find vin when gain=1 cross=last` 

which is given as `VIH= 0.98`.

![margin parameters](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/bb227133-d360-4be1-be41-d4ba806efb23)

This range is also referred to as Noise Immunity. There are two such values of Noise margins for a binary system:
NML(Noise Margin for Low) - VIL - VOL = (0.74 - 0) = 0.74V
NMH(Noise Margin for HIGH) - VOH - VIH = (1.8 - 0.98) = 0.82V

**So for our calculated values, the device would have, NML = 0.74V and NML = 0.82V.**

---

## 3.4 Delay Analysis

### 3.4.1 Propagation delay

![image](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/198ada4f-66b1-4aa4-95ba-0981682d169c)

The propagation delay of a logic gate e.g. inverter is the difference in time (calculated at 50% of input-output transition), when output switches, after application of input.

The propagation delay high to low (tpHL) is the delay when output switches from high-to-low, after input switches from low-to-high. Similarly for the tpLH. The delay is usually calculated at 50% point of input-output switching, as shown in above figure.

**Transient analysis would be used perform dealy analysis as delay is a form of time and transient analysis performs Time dependancy analysis**
>code_shown window\
>".lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt\
.tran 0.02 10n\
.save all\
.end"


![PROPAGATION DELAY WHEN RT=0 3n   fT=0 3n](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/44d2c9a3-7956-4116-a2c5-f46da43b4f0d)

CASE-1\
in here Vin is provided with Pulsed input:\
`pulse (0 1.8 0 .3n .3n 3n 6.6n 3)`
> syntax -> PULSE ( V1 V2 TD TR TF PW PER NP )

So, after plotting the waveform the propagation delay is measured at 50% of Vin and Vout transition.
```
meas tran vin50 when vin=.9 RISE=2
meas tran vout50 when vout=.9 RISE=2
```
for the above calculation the outputs are given by\
`vin50= 6.75n
vout50= 6.7748n`

then the 
```
Propagation Delay= vout50 - vin50= 0.0248ns
```
![PROPAGATION DELAY WHEN RT=0 3n   FT=0 3n](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/b446b486-22df-45d8-a558-89f56b6f6b27)


CASE-2 -> Analysing that Propagation delay gets reduced with reduction in input\
in here Vin is provided with Pulsed input:\
`pulse (0 1.8 0 .1n .1n 3n 6.2n 3)`

for the above calculation the outputs are given by\
`vin50= 6.25n, 
vout50= 6.26835n`

then the 
```
Propagation Delay= vout50 - vin50= 0.01835ns
```
![PROPAGATION DELAY WHEN RT=0 1n   FT=0 1n, so PD depends on input ](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/a4f57c96-f80a-4503-9156-a87cc60e5ab0)


**So, from the above observations indicates that the Propagation delay reduces with the reduction in input signal from which we can conclude that Propagation delay depends on the input signal.**

### 3.4.2 Rise time & Fall time

Rise time (tr) is the time, during transition, when output switches from 10% to 90% of the maximum value. Fall time (tf) is the time, during transition, when output switches from 90% to 10% of the maximum value. Many designs could also prefer 30% to 70% for rise time and 70% to 30% for fall time. It could vary upto different designs.

The Rise time calculated from the following code= 3.477^-11\

```
meas tran tout10 when vout=.18 RISE=1

meas tran tout90 when vout=1.6 RISE=1

print tout90 - tout10
```
![RISE TIME](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/4768c553-2548-4557-9a4e-c485feedfe9f)


The Fall time calculated from the following code= 3.142^-11\

```
meas tran tout10 when vout=.18 Fall=2

meas tran tout90 when vout=1.6 Fall=2

print tout90 - tout10
```
![FALL TIME](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/92e64c8d-c8ac-4feb-8c7c-23768e0bf52f)


### 3.4.3 Techniques to reduce Delay

The delay which we are finding is the *Unloaded delay* which is due to the delay caused by the internal capatitance even when the load capatitance is not connected. The internal capatitance is fixed by the foundry.\

This kind of unloaded delay can be reduced by 3 ways namely,\
- Increase the power supply\
- Increase the size of device\
- Reduce the Load CAPACITANCE.\

*For observing the difference in delay we'll take the `Rise Time=  3.477^-11` as the reference delay and compare the changes the delay to it.*

#### Technique-1: Increase the power supply

By increasing the power supply to the cicuit, the speed of the increases which inturn reduces the delay.\

We can understand this by an example, as the source voltage is already at its max voltage i.e. 1.8V, let us reduce the voltage to 1V so the delay should be increasing(considering the reverse condition). If the delay is increased by a reduction in voltage it would mean inturn that delay will be reduced with the Increase in voltage.

![Rise time when vdd=1](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/ea3bfeca-50b6-48e7-b021-a723a5774e20)

So, from the above figure we can observe the increase in the Rise time i.e. from 3.477e^-11 to 6e-11 with the reduction in the voltage source from 1.8V to 1V. **So, we can conclude that with the increase in the power supply the Delay time will be reduced.**


#### Technique-2: Increase the size

Theoretically, with the increase in the size causes Resistance to reduce with inturn increases current conduction and then intutrn increases the capacitor conduction, so due to this there will be fast response and less delay.\

- case-1\
Now we'll test this thoery by increasing the Aspect ratio(W/L) of PMOS from (2/0.15) to (4/0.15) and the Aspect ratio(W/L) of NMOS from (1/0.15) to (2/0.15).\
Here we get a `Rise delay of 3.4608^-11`

![rise time when Vdd=1.8, PMOS WIDTH=4  NMOS WIDTH=2](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/9c412334-1110-4bc3-986e-9f4fbe64f616)

- case-2\
Now we'll test this thoery by increasing the Aspect ratio(W/L) of PMOS from (4/0.15) to (8/0.15) and the Aspect ratio(W/L) of NMOS from (2/0.15) to (4/0.15).\
Here we get a `Rise delay of 3.4546^-11`

![rise time when Vdd=1 8, PMOS WIDTH=8  NMOS WIDTH=4 ](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/6b89e38f-276b-4b75-9f87-e93a0a8772f5)

**So, from the above figure we can observe there is reduction in Delay which is very minimal.** This is due to as it is a unloaded delay where delay is due to internal capacitance. So, when there is an increase in size of device the internal delay will also be increasing.\

#### Technique-3: Reducing Load Capacitance 

For better understanding we'll include the Load capacitor which will be connected externally to the inverter circuit.\

![including load capacitance](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/4acc633d-976e-42a9-a9ad-a6f74b5405ce)

![vout when load capatitance=1p](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/aac3806d-65b4-4fca-bf71-ee8a748a8eb6)

Here we observe that, when CL=1pf the capacitor is charging and discharging rapidly so that the values won't be setteling at any point. In such case we can *increase the clock period.*

![rise time when clockperiod is increased](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/8db723f6-ebdc-4c07-bf24-2622ce6c8431)

from the above design `Rise time= 1.2566^-9`

![when load capatitance is further reduced i e 0 5pf](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/9f6872bc-7c42-4559-933f-fbb2912f4a7a)

from the above design `Rise time= 6.33198^-10`\

**So, we can conclude that by reducing the Load capacitance the delay will be reducing bu a great extent.**

---
## 3.5 Power simulation

![Power formula](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/59d09714-aefe-43a3-a951-0068c2fd2e3f)

As we have already known that Power= current * voltage\ 

Any digital circuit will be having 3 power components namely dynamic,static and short circuit. all these  coponents are analyzed by using integration as shown in the above formula.  
The current is from Vdd as the current generated from the Vin is almost to zero so it will be neglected.\
The current from vdd can be plotted using `plot vdd#branch`

![current vs output voltage](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/57a8bba1-8167-4bb6-8467-aeb035a8a47b)

The calculation to identify the power by using the above formula is given in below figure\

#### 3.5.1 power reducing techniques
- Reducing Load capacitance values
- Reducing Size of circuit elements
- Reducing input power supply
- Reducing no.of switching cycles

### Technique-1 Reducing Load capacitance values
The power can be reduced by reducing the Load capacitor values\
Here the power has been calculated when CL=.5pf
`power= 8.148w`
![power calculated when c= 5pf](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/32379947-c156-4d3c-8f6b-26b721b43e0b)

Here the power has been calculated when CL=.1pf
`power= 1.666w`
![power calculated when c= 1pf](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/f5361e61-22ff-4a99-ab7d-3a23ecc5bcc2)

### Technique-2 Reducing Size of circuit elements

and another factor that the power depends is the size of the circuit elements
by increasing the size i.e. Aspect Ratio of PMOS from (2/0.15) to (4/0.15) and the Aspect ratio(W/L) of NMOS from (1/0.15) to (2/0.15).

![power calculated when increase in nmos and pmos](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/047d4c6a-99d7-44b8-b92b-339e9657c5f1)
From the above circuits we can observe that by increasing the size of circuit elements increases the consumption of power more.\

### Technique-3 Reducing input power supply

By reducing the input power supply to 1V we can identify the decrease in the power consumption.

![power calculated when reduction in vdd power supply vdd=1](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/c6a617db-edcf-4c9a-9a0b-75192d949aa4)
