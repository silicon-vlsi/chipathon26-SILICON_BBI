# Chipathon 2026, Silicon University, Odisha
Chipathon 2026 resources for Silicon University, Odisha participant

**IMPORTANT LINKS**

- [**Official Chipathon 2026** Repo](https://github.com/sscs-ose/sscs-chipathon-2026)
- [**Zoom Link** for **Official Chipathon Meeting** every **Friday at 8:30PM IST**](https://us06web.zoom.us/j/87694732928?pwd=gjUePaAEKDJB2G3f2d4iPIqyYe0qBx.1)
- [**Zoom Link** for **SILICON team** meeting at (TBD)]()
- [An Open-Source **AMS SoC** Template for IHP-SG13G2](https://github.com/iic-jku/ihp-sg13g2-ams-chip-template)
- [RISCV-321M tinytapeout uLinux](https://github.com/splinedrive/KianV_rv32ia_uLinux_SoC/)


# GETTING STARTED

## Installing IIC-OSIC-TOOLS

- One line install script (From Discord chati @Harald Pretl)
  - macOS/Linux: `curl -fsSL https://osic.tools/install.sh | bash`
  - Windows 10/11: `powershell -c "irm https://osic.tools/install.ps1 | iex`
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
- [KIANV uSoC runnign Linux](https://tinytapeout.com/chips/tt06/tt_um_kianV_rv32ima_uLinux_SoC) (Tinytapeout RISCV design,, post silicon work)
  - [Companion GitHub page](https://github.com/stevehoover/LF-Building-a-RISC-V-CPU-Core)
- [MYTH TLV Workshop](https://github.com/stevehoover/RISC-V_MYTH_Workshop)
- [TLV Projects](https://github.com/TL-X-org/TL-V_Projects): good starting point for those interested in TLV projects.

## Analog EDA Tools QuickStart Guide
- Check [this resource page](https://github.com/silicon-vlsi/SI-2026-AnalogIC#resources) to get started with Analog EDA tools (xschem, ngspice, magic)

# RESOURCES

## Literature

**PAPERS**
- Kinget, P. R. “Device Mismatch and Tradeoffs in the Design of Analog Circuits.” IEEE Journal of Solid-State Circuits 40, no. 6 (2005): 1212–24. ([Link to PDF](https://www.researchgate.net/profile/Peter-Kinget-2/publication/2982858_Device_mismatch_and_tradeoffs_in_the_design_of_analog_circuits/links/546094640cf27487b450f6b8/Device-mismatch-and-tradeoffs-in-the-design-of-analog-circuits.pdf)) : Very good reference on how to apply Pelgrom's mismatch model to analog circuits.
- Pelgrom, M. J. M., A. C. J. Duinmaijer, and A. P. G. Welbers. “Matching Properties of MOS Transistors.” IEEE Journal of Solid-State Circuits 24, no. 5 (1989) ([Link to PDF](http://class.ece.iastate.edu/djchen/EE501/2016/MOS%20TransistorMatching%20pelgrom89.pdf)): Classic paper from Pelgrom orginally formualting the mismatch model.


**NOTES**
- Rout S. "Notes on Calculating current mismatch in current mirrors.", 2026 ([Link to PDF](https://www.dropbox.com/scl/fi/hr193ji9tlm85501c10am/Rout-Notes-mismatch-currentMirror-CHIPATHON2026.pdf?rlkey=u5vzdwcq7q92n9r3ixjvpjfp5&dl=0))
