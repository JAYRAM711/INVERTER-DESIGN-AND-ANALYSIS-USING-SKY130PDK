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

#### Steps to install Xschem 
Follow the instructions given [here](http://repo.hu/projects/xschem/xschem_man/install_xschem.html).

#### 1.1.2 Ngspice

![image](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/ae09b04c-3353-44c2-8f65-2968a84488ab)


[Ngspice](https://ngspice.sourceforge.io/devel.html) is the open source spice simulator for electric and electronic circuits. Ngspice is an open project, there is no closed group of developers.

Ngspice Reference Manual: [Complete reference manual](https://ngspice.sourceforge.io/docs/ngspice-manual.pdf).

#### Steps to install Ngspice -
Don't use the version that comes with linux distribution, since it is dated and sometimes misses crucial updates

Follow [this](https://www.youtube.com/watch?v=jXmmxO8WG8s&t=1032s) video for just the ngspice installation. DO NOT use this video to install xschem and skywater-pdk.

The above video should be fine but if it does not work for you, Follow the instructions given inside the INSTALL and README file that comes inside the git clone of the repository of ngspice

#### 1.1.3 Magic

![image](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/34757c88-ccd1-4fe7-a5d4-b18477bdefbe)

[Magic](http://opencircuitdesign.com/magic/) is a VLSI layout tool.

#### Steps to install Magic 
Follow the instructions on the [Opencircuitdesign](http://opencircuitdesign.com/) Install section. I would suggest to use a couple options for the configuration file.

#### 1.1.4 Netgen

![image](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/e4bb4c9c-3ab1-46f6-a10f-e3df6bbfb26c)

Netgen is a tool for comparing netlists, a process known as LVS, which stands for "Layout vs. Schematic". This is an important step in the integrated circuit design flow, ensuring that the geometry that has been laid out matches the expected circuit.

#### Steps to install Netgen
Open the terminal and type the following to insatll Netgen.

```$  git clone git://opencircuitdesign.com/netgen
$  cd netgen
$  ./configure
$  sudo make
$  sudo make install
```

Steps to install Xschem Follow the instructions given here.

1.2 PDK setup
A process design kit (PDK) is a set of files used within the semiconductor industry to model a fabrication process for the design tools used to design an integrated circuit. The PDK is created by the foundry defining a certain technology variation for their processes. It is then passed to their customers to use in the design process.

The PDK we are going to use for this BGR is Google Skywater-130 (130 nm) PDK.
![image](https://github.com/JAYRAM711/INVERTER-DESIGN-AND-ANALYSIS-USING-SKY130PDK/assets/119591230/b7c776dc-db63-4ce8-9fef-b67b257d8b0f)



#### Steps to download PDK 
Open the terminal and type the following to download sky130 PDK.

```
$  git clone https://github.com/RTimothyEdwards/open_pdks.git
$  cd open_pdks
$  ./configure [options]
$  make
$  [sudo] make install
You should follow the instructions given at this link
```
