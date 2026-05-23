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

## Analog EDA Tools QuickStart Guide
- Check [this resource page](https://github.com/silicon-vlsi/SI-2026-AnalogIC#resources) to get started with Analog EDA tools (xschem, ngspice, magic)

# RESOURCES
