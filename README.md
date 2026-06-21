# Chipathon 2026, Silicon University, Odisha
Chipathon 2026 resources for Silicon University, Odisha participant

**IMPORTANT LINKS**

- [**Official Chipathon 2026** Repo](https://github.com/sscs-ose/sscs-chipathon-2026)
- [**Zoom Link** for **Official Chipathon Meeting** every **Friday at 8:30PM IST**](https://us06web.zoom.us/j/87694732928?pwd=gjUePaAEKDJB2G3f2d4iPIqyYe0qBx.1)
- [**Zoom Link** for **SILICON team** meeting every (**Sat at 3:30pm**)](https://zoom.us/j/96686100775?pwd=tAuGcLBVNUlQEMXa4Qszu53cYF8BaK.1)
- [**SILICON_BBI_D15 ISSUE CHIAPTHON 2025**](https://github.com/sscs-ose/sscs-chipathon-2025/issues/50)

# TEAMS

## A28 SILICON_BGR

- Title: Compact Low-Power Self-Biased Band Gap Reference Generator with Layout Optimization and Corner Analysis
- [Issue#98](https://github.com/sscs-ose/sscs-chipathon-2026/issues/98)

## A29 SILICON_DAC 

- Title: 8-Bit Low Power Current Steering DAC
- [Issue#90](https://github.com/sscs-ose/sscs-chipathon-2026/issues/90)

## A30 SILICON_RISC-V

- Title: 32-bit RISC-V (RV32I) microcontroller using TL-Verilog and Librelane-ORFS Flow
- [Issue#95](https://github.com/sscs-ose/sscs-chipathon-2026/issues/95)
- **RESOURCES**
  - [TinyQV RISCV on TinyTapeout-10](https://tinytapeout.com/chips/ttihp25a/tt_um_MichaelBell_tinyQV): A real tiny RISCV core using QSPI Flash, PSRAM and GPIO, UART interface.
  - [KIANV uSoC runnign Linux](https://tinytapeout.com/chips/tt06/tt_um_kianV_rv32ima_uLinux_SoC) A simple Tinytapeout design using QSPI flash and PSRAM with SPI, GPIO, UART interface.

## A31 SILICON_SAR-ADC

- Title: Low Power 8-bit SAR ADC with Capacitive DAC and Dyamic Comaprator
- [Issue#97](https://github.com/sscs-ose/sscs-chipathon-2026/issues/97)


# RESOURCES

## Literature

**PAPERS**
- Kinget, P. R. “Device Mismatch and Tradeoffs in the Design of Analog Circuits.” IEEE Journal of Solid-State Circuits 40, no. 6 (2005): 1212–24. ([Link to PDF](https://www.researchgate.net/profile/Peter-Kinget-2/publication/2982858_Device_mismatch_and_tradeoffs_in_the_design_of_analog_circuits/links/546094640cf27487b450f6b8/Device-mismatch-and-tradeoffs-in-the-design-of-analog-circuits.pdf)) : Very good reference on how to apply Pelgrom's mismatch model to analog circuits.
- Pelgrom, M. J. M., A. C. J. Duinmaijer, and A. P. G. Welbers. “Matching Properties of MOS Transistors.” IEEE Journal of Solid-State Circuits 24, no. 5 (1989) ([Link to PDF](http://class.ece.iastate.edu/djchen/EE501/2016/MOS%20TransistorMatching%20pelgrom89.pdf)): Classic paper from Pelgrom orginally formualting the mismatch model.

**NOTES**
- Rout S. "Notes on Calculating current mismatch in current mirrors.", 2026 ([Link to PDF](https://www.dropbox.com/scl/fi/43msh3ic73igy0jbj1rzy/Rout_Notes_MismatchCurrentMirror.pdf?rlkey=2timr84iabzvwukh4vvnbfzaa&dl=0))

## Process PDK
- [Official GF180MCU PDK page](https://gf180mcu-pdk.readthedocs.io) and some popular sections:
  - [Model Parameters](https://gf180mcu-pdk.readthedocs.io/en/latest/analog/model_parameters/LV/LV.html)
    - [Mismatch Modeling](https://gf180mcu-pdk.readthedocs.io/en/latest/analog/model_parameters/LV/LV_9_4.html#v-nmos-and-pmos-mismatch-verification-plots)
    - [Flciker Noise - 3.3V](https://gf180mcu-pdk.readthedocs.io/en/latest/analog/model_parameters/LV/LV_8_1.html#i-f-noise-characteristics)
  - [SRAM IP/Macros](https://gf180mcu-pdk.readthedocs.io/en/latest/IPs/SRAM/sram.html)
    - [Open Circuit Design 3.3V SRAM macros for GlobalFoundries GF180MCU open PDK](https://github.com/RTimothyEdwards/gf180mcu_ocd_ip_sram/) by Tim Edwards
  - [I/O IP/Macros](https://gf180mcu-pdk.readthedocs.io/en/latest/IPs/IO/io.html)
- [Chipathon 2026 resource integration page](https://github.com/sscs-ose/sscs-chipathon-2026/blob/add-glayout-intro/resources%2FIntegration%2FREADME.md): This resource page contains the spcific PDK options for Chipathon 2026 including Metal Stack, MIM, poly-resistor, etc.
- [Datasheet summary and absic design rules](https://github.com/sscs-ose/sscs-chipathon-2026/tree/add-glayout-intro/resources/Analog/pdk)

**PROCESS SUMMARY**
- Process: 0.18um 3.3V/(5V) 6V (with MIM) with deep-nwell
- Operating Voltage: LV=3.3V, HV=5V or 6V
- BEOL Stack: 1P5LM
- Top Metal: 11kA
- MIM Cap Option: Type B (TM-1 / TM)
- MIM Cap density: 2fF/um^2
- High-res Poly: 1k/sq (the model to be used is: _ppolyf_u_1k_)


# GETTING STARTED

## Installing IIC-OSIC-TOOLS

- One line install script (From Discord chati @Harald Pretl)
  - macOS/Linux: `curl -fsSL https://osic.tools/install.sh | bash`
  - Windows 10/11: `powershell -c "irm https://osic.tools/install.ps1 | iex"`
- The above is suppose to take care of the required isntallation: **WSL, Docker Desktop, Git**
  - During Docker install the select: ✅ Use WSL 2 instead of Hyper-V (recommneded)
- **NOTE** The docker container is a large (>5GB) file so it will take while if the internet connection is slow. Alternatively, you can install the docker from a tar file:
  - `docker install -i <file.tar>`
  - You can create a tarball from an already installed docker image e.g.:
    - `docker save -o iic-osic-tools.tar hpretl/iic-osic-tools:chipathon26`
- Once the docker image installed and you can start the container by running the script `start_chipathon_x.bat` in the IIC-OSIC tools foler.

## RISC-V and TL-Verilog

- [Makerchip IDE](https://makerchip.com/ide/) >> Learn >> **Tutorials**/Courses: lot of resources from 1 hr tutorial to 30-hr MYTH workshop.
- [Building a RISC-V Core](https://www.edx.org/course/building-a-risc-v-cpu-core) on EdX
  - [Companion GitHUb Page](https://github.com/stevehoover/LF-Building-a-RISC-V-CPU-Core)
- [MYTH TLV Workshop](https://github.com/stevehoover/RISC-V_MYTH_Workshop)
- [TLV Projects](https://github.com/TL-X-org/TL-V_Projects): good starting point for those interested in TLV projects.

## Analog EDA Tools QuickStart Guide
- Check [this resource page](https://github.com/silicon-vlsi/SI-2026-AnalogIC#resources) to get started with Analog EDA tools (xschem, ngspice, magic)
