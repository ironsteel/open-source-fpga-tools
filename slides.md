---
title:
- From Verilog to Bitstream
subtitle:
- with free and open source tools
author:
- Rangel Ivanov
theme:
- Copenhagen
date:
- June 8 2019 TuxCon
geometry: margin=1cm
---

# Agenda

+ What is an FPGA?
+ FPGA flow process
+ Vendor tooling
+ Open source tools
+ Supported devices
+ Demos

# What is an FPGA?

+ Field Programmable Gate Array (FPGA)
+ Integrated curcuit
+ Reconfigurable/programmable digital logic
+ Composed of logic elements and routing fabric
+ A way to create custom hardware
    - HDMI capture
    - Custom CPU
    - Motor controller
+ Parallel in nature
+ Lots of IO
+ Fast and flexible (depending on the device size)
+ "Programmed" using Verilog/VHDL
    - Describe a digital curcuit design using high level constructs

# Simplified FPGA internals

![FPGA internals](fpga_internals.png)

# A small FPGA

![ICE Zero](icezero.jpg)

# FPGA Flow

![](fpga_flow.png)

# Example blinky verilog code

```verilog
module blink(input clk_25Mhz, output led);
    // 25 000 000 hz / (2 ^ 23 - 1) hz = 1 hz
    reg [23:0] counter = 0;
    reg led_status = 0;

    always @(clk_25Mhz) begin
        counter <= counter + 1;
        // Every 1 Hz MSB toggles
        // MSB is the least changing bit
        led_status = counter[23];
    end

    assign led = led_status;
end module
```


# Vendor Tools

- Every device vendor has it's own tooling
- Device architecture differ from vendor to vendor
- Huge gigabyte downloads
- Close source/propriatary
- No "GCC" equivalent



# Enter Project iCEStorm

- The open source verilog-to-bitstream flow for FPGA's
- Targets Lattice's iCE40 family
    - Small to Medium size FPGA's
    - 4 Input LUT architecture
    - Approx max 7800k Logic elements
    - Relatavely simple architecture
- Released in 2015
- Fully documented bitstream format
- Mostly done by Clifford Wold


# Project iCEStorm

- Yosys - verilog parsing and synthesis
- Arachne PnR - place and route
- IcePack - bitstream generation
- IceProg - FPGA download/programming tool
- IceTime - Timing analysis
- [Website](http://www.clifford.at/icestorm/)

# Demo

- NES implementation on an iCE40 FPGA
    - Olimex iCE40HX8K FPGA
- VGA output
- SRAM interfacing
- Game loaded from SPI Flash
- My first big experience with FPGA's

# Simulation and verification

- Verilator - https://www.veripool.org/wiki/verilator
- Icarus Verilog - http://iverilog.icarus.com/
- SymbiYosys - https://symbiyosys.readthedocs.io/
    - Formal methods/verification
    - https://zipcpu.com/blog/2017/10/19/formal-intro.html
    - Developed by https://www.symbioticeda.com/

# 2018 - a new beggining for opensource PnR

- Arachne PnR is not timing driven
- Arachne PnR is specific to iCE40
    - We want to add support for new devices
    - We want a PnR tool that abstracts the target archecture

# Enter nextPnR

- Aims to be vendor neutral
- Timing driven
- Algorithm and architecture exploration
- Bigger and faster devices
- https://github.com/YosysHQ/nextpnr


# NextPnR screenshot

![nextpnr](nextpnr.png)


# Lattice ECP5 support

- Project Trellis - https://github.com/SymbiFlow/prjtrellis
    - Dave Shah
- Up to 85K logic elements
- High speed DDR
- 2-4 SERDES lines
- Max 180 DSP's
- Potential use cases
    - HDMI capture
    - 1G Ethernet adapter
    - Software Defined Radio
    - ML Accelerator

# Demo

- 50Mhz RISC-V linux capable SoC
- RV32 with MMU support
- 64MBytes of RAM
- VexRiscV - https://github.com/SpinalHDL/VexRiscv
- Built using Litex
    - https://github.com/enjoy-digital/linux-on-litex-vexriscv


# Other bitstream documentation efforts

- SymbiFlow - https://symbiflow.github.io/
    - The umbrella project for all FOSS flows
- Xilinx 7 series in progress
- Lattice MachXO2 in progress

