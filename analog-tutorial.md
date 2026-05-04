# Analog Design Tutorial

This is a short tutorial for first-time users of the **xschem** and **ngspice** to get them started right away.

## Resources

- **xschem**:
  - [Xscheme Tutorial](https://xschem.sourceforge.io/stefan/xschem_man/tutorial_run_simulation.html).

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
  - Insert a new instance by selecting `Tools` >> `Insert Symbol` and selecting the folder where newly created symbol resides (say `inv1.sym`)

- Create a new schematic as follows.

>> `VDD`, `VIN`: `vsource.sym`

>> `vdd`, `vin`, `vout`: `lab_pin.sym`

![](images/3.4-07-insert_symbol.png)

- Next, setup the library and simulation options as follows.

![](images/3.4-08-setup_library_and_simulation.png)

#### 6. Run NGSpice in Batch Mode

- Click on `Simulation` >> `Configure simulators & tools`, selecting `Ngspice batch` and `Gaw viewer`, then click on `Accept, no Save and Close`.

![](images/3.4-09-batch_mode.png)

#### 7. Generate Netlist and Simulate

- Click on `Netlist` button to generate the netlist

- Click on `Simulation` >> `Edit Netlist` to view the netlist

- Click on `Simulate` button to start the simulation

![](images/3.4-10-generate_netlist_and_simulate.png)

#### 8. View the Waveform

We use `Xscheme-GAW` to view the waveform.

- Click on `Waves` button and select `External viewer`

![](images/3.4-11-external_viewer.png)

- In `GAW GUI` opened, click on a panel first, then click on the signal you want to display

![](images/3.4-12-select_signal.png)



#### 6. Schematic capture - Set up a simulation

To set up the simulation, insert a code symbol and enter the simulation commands

- Create a code symbol by `xscheme_library/devices` >> `code.sym` >> `OK` >> Click on the schematic to place it

![](images/3.1-18-create_simulation_symbol.png)

![](images/3.1-19-simulation_symbol_view.png)

- Select the code symbol and press `q`; change the `name` into `STIMULI` and `value` to:

```spice
".tran 10n 2000u uic
.save all"
```

![](images/3.1-20-insert_simulation_command.png)

![](images/3.1-21-final_view.png)

### Note

- The double quote is required

#### 7. Simulate the design using NGSpice 

- Xscheme uses NGSpice as the default simulator

- To create the design's netlist, click `Netlist` button to generate the spice file

![](images/3.1-22-generate_netlist.png)

- To view the netlist, select `Simulation` >> `Edit netlist`

![](images/3.1-23-edit_netlist.png)

- To simulate, click `Simulate` button to run the simulation

![](images/3.1-24-simulate_design.png)

#### 8. Plot the waveform in NGSpice

- Plot the waveform in NGSpice by enter `plot a b c` in NGSpice terminal

![](images/3.1-25-ngspice_simulation.png)




