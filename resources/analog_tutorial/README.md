# Analog Design Tutorial

This is a short tutorial for first-time users of the **xschem**. **ngspice** and **magic** to get them started right away.

## Resrouces 

- [Open-Source Analog Design Flowing using EFabless and SKY130nm (PDF)](Thater-OpenSource-AnalogDesgnFlow-Efabless-SKY130.pdf): Detail end-to-end design flow by by Joshua Thater.


- **IIC-OSIC-TOOLS**
  - [OSIC-TOOLS GitHub Page](https://github.com/iic-jku/iic-osic-tools)
  
- **xschem**:
  - [Official Site](https://xschem.sourceforge.io/stefan/index.html)
  - [Tutorial: Run a simulation with xschem](https://xschem.sourceforge.io/stefan/xschem_man/tutorial_run_simulation.html): a quick step-by-step html guide from the creator Stefan Schippers.
  - [xschem displaying simulation waveform](https://www.youtube.com/watch?v=bP9w3zm1qv4): a 10min video on embedded graphs by Stephan Schippers.  
  - [Viewing Simulation Data with xschem](http://repo.hu/projects/xschem/xschem_man/graphs.html): html guide on the official site.
  - Three part *tutorial videos* using xschem and ngspice with GF180MCU
    - [Part-1](https://youtu.be/MdywD87-DVg) | [Part-2](https://youtu.be/DLvZSsLAbho) | [Part-3](https://youtu.be/nBnR8Nm_B_I)

- **ngspice**
  - [Official Site](https://ngspice.sourceforge.io/)
  - [ngspice Manual](https://ngspice.sourceforge.io/docs/ngspice-manual.pdf)

- **Klayout** 
  - [Tutorial using KLayout with gf180mcu (part 4)](https://www.youtube.com/watch?v=vamfMryYPS4)

# Schematic-entry and Simulation using xschem and ngspice

It is assumed that you have already installed the opensource design tools using [IIC OSIC TOOLS](https://github.com/iic-jku/iic-osic-tools) container.

- Run Xscheme inside the OSIC-TOOLS docker image:
  - `$ xschem`
- Create a new instance by selecting `Tools -> Insert Symbol` or, `(Shift + i)`
- Two essential libraries:
  - `.../xschem_library/devices` : technology independent primitive devices eg.:
    - Linear elements: `res.sym`, `capa.sym`, `ind.sys`
    - Stimulus sources: `vsource.sym`, `isource.sym`
    - Controled sources: `vcvs.sym`, `vccs.sym`, etc. 
  - `/foss/pdks/gf180mcuD/libs.tech/xschem/symbols` : GF180MCUD devices:
    - FETS: `nfet_03v3.sym`, `pfet_03v3.sym`, `nfet_05v0.sym`, `pfet_05v0.sym`
- After instating devices, wire them, change properties and add ports.
  - To change properties: select the devices and press `q` and change the value.
  - Place pointer on the port to start *wiring*  press `w` to start a wire connection. To jog/bend the wire, press `w` Or, click and drag the end of the wire to continue wiring.
  - Select and press `c` to *copy* an instance and left click to place it.
  - Select and press `m` or *left-click* and drag to move an instance.
  - While moving an instance, you can rotate or flip by:
    - `Shift+r` to *rotate*
    - `Shift+f` to *flip*
- To view the waveform, *label* essential nodes by instatiating `lab_pin.sym` from the `.../xschem_library/devices` library. Place the pin on the wire, *double-click* or `q` the pin and add a succint but descriptive wire name.
- If this is a block you want to create a symbol, you need to add ports eg. `vin`, `vout`, etc.
  - Place ports from the `.../xschem_library/devices` library:
    - `ipin.sym` : Input port
    - `opin.sym` : Output port
    - `iopin.sym` : Input/Output port
  - *Note* that ngspice (or any spice) does not recognize port type. This is for schemtic level checks.

- **Create a symbol** by selecting `Symbol` >> `Make symbol from schematic`
  - Click `OK` on the dialog `Do you want to make symbol view`
  - A new file (say `inv1.sym`) will be create in the same directory
  - Edit the symbol:
    - Click on `File` >> `Open` then select `inv1.sym` in the open dialog

- You might need to choose the correct directory containing the symbol file first.

- **Draw your desired shape**
  - Delete the rectangle by selecting the lines and press `delete`.
  - Use the line/circle/etc to draw your shape
  - Move the pins to apprpirate place on the shape. 

- **Create a Testbench** 
  - Create a new schematic by selecting `File` >> `Create new window/tab`
  - Insert the new instance created (say `inv1.sym`)
  - Instatiate the voltage sources: `vsource.sym` from the `../devices` library. The *value* field of vsource.sym for the supply and the input are:
    - `PAR_VDD` : parameter for the fixed suply voltage .
    - `0 PULSE("PAR_VDD" 0 PAR_DEL PAR_SLEW PAR_SLEW "0.5*PAR_PER" "1.0*PAR_PER"` : *parameterized* pulse source for the input.
  - Instatiate a capacitance (`.../devices` -> `capa.sym`) and you can enter a fixed value or a parameter eg. `PAR_CLOAD`.
  - A global ground `..../devices -> gnd` is required.
  - After wiring up, we need to instatiate two blocks for spice code:
    - For model files:

``` spice
name=MODELS only_toplevel=true  
format="tcleval( @value )" 
value="
.include $::180MCU_MODELS/design.ngspice
.lib $::180MCU_MODELS/sm141064.ngspice typical
.lib $::180MCU_MODELS/smbb000149.ngspice typical
"
```

    - For parameters, measure statements, and simulation commands:

``` spice
name=NGSPICE only_toplevel=true  
value="
** PARAMETERS
.PARAM PAR_VDD=3.3
.PARAM PAR_CLOAD=100f
.PARAM PAR_SLEW=100p
.PARAM PAR_PER=10n
.PARAM PAR_DEL='0.1*PAR_PER'

** Rise/Fall 10-90%
.MEASURE TRAN tr1090 TRIG v(vout) VAL='0.1*PAR_VDD' RISE=1 TARG v(vout) VAL='0.9*PAR_VDD' RISE=1
.MEASURE TRAN tf9010 TRIG v(vout) VAL='0.9*PAR_VDD' FALL=1 TARG v(vout) VAL='0.1*PAR_VDD' FALL=1

** Delay Rise Fall
.MEASURE TRAN tdrise TRIG v(vin)  VAL='0.5*PAR_VDD' FALL=1 TARG v(vout) VAL='0.5*PAR_VDD' RISE=1
.MEASURE TRAN tdfall TRIG v(vin)  VAL='0.5*PAR_VDD' RISE=1 TARG v(vout) VAL='0.5*PAR_VDD' FALL=1

**Leakage current and average current
.MEASURE TRAN iavg AVG vsup#branch FROM=PAR_DEL TO='PAR_DEL+PAR_PER'
.MEASURE TRAN ileak AVG vsup#branch FROM='PAR_DEL+0.4*PAR_PER' TO='PAR_DEL+0.45*PAR_PER'

.control
save all
OP
TRAN 1p 21n
write TB_inv1.raw
plot v(vin) v(vout)
.endc

"
```

- **Generate Netlist and Simulate**
  - Click on `Netlist` button to generate the netlist
  - Click on `Simulation` >> `Edit Netlist` to view the netlist
  - Click on `Simulate` button to start the simulation

- **View the Waveform**

  - Easiest way to plot is using the ngspice using the `plot` command in the simulation commands.
  - We can use `Xscheme-GAW` to view the waveform.
    - Click on `Waves` button and select `External viewer`
    - In `GAW GUI` opened, click on a panel first, then click on the signal you want to display
    - You can also use embedded plots in xschem. References can be found in the *resource* on top.

