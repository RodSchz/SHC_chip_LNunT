<div align="center">

<img src="https://umsousercontent.com/lib_lnlnuhLgkYnZdkSC/hj0vk05j0kemus1i.png" alt="ChipFoundry Logo" height="140" />

# Open-Source Smart Airflow Sensor Platform with Custom SKY130 Silicon

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![ChipFoundry Marketplace](https://img.shields.io/badge/ChipFoundry-Marketplace-6E40C9.svg)](https://platform.chipfoundry.io/marketplace)
[![Project Status](https://img.shields.io/badge/Status-Concept%20%2F%20Development-orange.svg)]()
[![Process](https://img.shields.io/badge/Process-SKY130-0A7EA4.svg)]()
[![Integration](https://img.shields.io/badge/Integration-Caravel%20User%20Project-4C6EF5.svg)]()

</div>

---

## Table of Contents
- [Overview](#overview)
- [Project Motivation](#project-motivation)
- [Current Prototype Baseline](#current-prototype-baseline)
- [System Architecture](#system-architecture)
- [ASIC Scope](#asic-scope)
- [Target Applications](#target-applications)
- [Repository Structure](#repository-structure)
- [Documentation \& Resources](#documentation--resources)
- [Prerequisites](#prerequisites)
- [Starting Your Project](#starting-your-project)
- [Development Flow](#development-flow)
- [GPIO Configuration](#gpio-configuration)
- [Verification Plan](#verification-plan)
- [Local Precheck](#local-precheck)
- [Mechanical, PCB, and Firmware Deliverables](#mechanical-pcb-and-firmware-deliverables)
- [Project Roadmap](#project-roadmap)
- [Checklist for Shuttle Submission](#checklist-for-shuttle-submission)
- [License](#license)
- [Acknowledgments](#acknowledgments)

---

## Overview

This repository contains an **open-source airflow sensing reference design** based on:

- a **hot-wire thermal flow transducer**,
- a **custom SKY130 chip** integrated as a **Caravel user project**,
- a **support PCB**,
- **firmware** for configuration and data collection,
- and a **mechanical flow body / enclosure**.

The project goal is to demonstrate how **custom silicon** can improve a real airflow sensing platform by reducing board complexity, improving configurability, and enabling tighter integration between the transducer, electronics, and mechanical structure.

This project is intended as a **reference design demonstrator** and **open development platform**, not as a finished certified medical device.

---

## Project Motivation

Many practical airflow sensing systems rely on discrete analog front ends. While functional, that approach often increases:

- PCB size,
- tuning effort,
- sensitivity to parasitics,
- assembly variability,
- and difficulty of reproduction.

This project aims to evolve an already working hot-wire airflow prototype into a more integrated and reproducible open platform by moving part of the sensor-interface and control functionality into a custom **SKY130 ASIC**.

The resulting system is intended to serve as:

- a silicon-enabled airflow sensing demonstrator,
- an open hardware development platform,
- and a reusable reference design for future industrial, laboratory, and embedded sensing applications.

---

## Current Prototype Baseline

The present hardware prototype already demonstrates the main sensing concept and system-level integration. The current baseline includes:

- hot-wire airflow sensing principle,
- microfabricated heated transducer,
- discrete analog front-end,
- support PCB,
- packaged flow body,
- and integrated mechanical assembly.

Current prototype targets / baseline characteristics:

- **Supply:** ±5 V
- **Analog output:** 0–5 V
- **Flow range:** 0–160 L/min
- **Mechanical connector concept:** 22 mm interface

> **Note**
> These values describe the current prototype baseline and may evolve during the contest-driven redesign.

### Prototype Image

Add a representative system image here once the repository assets are organized.

```md
![Prototype Assembly](docs/img/prototype_sensor.jpg)
```

---

## System Architecture

The platform is composed of four layers:

### 1. Thermal Flow Transducer
A hot-wire airflow transducer based on a micro-heated element and auxiliary thermal sensing elements.

### 2. Custom Silicon
A SKY130 chip integrated inside the Caravel user area to implement configurable sensor-interface and digital control functions.

### 3. Support Electronics
A PCB containing the blocks intentionally left off-chip in revision 1, such as precision references, final power conditioning, host interface, protection, and test hooks.

### 4. Mechanical Platform
A reproducible flow body and electronics enclosure that supports assembly, airflow testing, and demonstration.

### High-Level Block Diagram

Add a system diagram here when available.

```md
![System Architecture](docs/img/system_architecture.png)
```

Suggested block breakdown:

- hot-wire transducer
- optional RTD / thermal auxiliary channels
- ASIC sensor-control and diagnostics
- external ADC / MCU in revision 1
- power and protection
- mechanical flow body

---

## ASIC Scope

To keep the project realistic for the Caravel + OpenLane contest flow, the ASIC will focus on **high-value, manageable integration blocks** rather than trying to replace the full precision analog chain in the first revision.

### On-Chip Functions

#### Sensor Interface / Mixed-Signal Support
- programmable excitation control for the hot-wire element,
- analog channel selection / multiplexing,
- threshold-based diagnostic comparators,
- programmable trimming and bias control,
- observability and test modes.

#### Digital Functions
- configuration registers,
- SPI or I²C slave interface,
- startup / control state machine,
- diagnostic and fault flags,
- digital calibration and control settings,
- Caravel-facing integration logic.

### Off-Chip Functions in Revision 1
- precision voltage / current reference,
- high-resolution ADC,
- final power conditioning,
- host MCU or external controller,
- system-level protection,
- user interface.

### Why This Partition
This partition is intended to:

- reduce implementation risk,
- fit the Caravel user-project flow more naturally,
- keep verification tractable,
- and maximize the chance of a tapeout-ready submission.

---

## Target Applications

This project is positioned as an **open airflow sensing platform** for:

- industrial airflow monitoring,
- embedded gas-flow instrumentation,
- laboratory setups,
- educational platforms,
- airflow research systems,
- edge sensing and telemetry nodes,
- respiratory-flow experimentation in non-certified environments.

---

## Repository Structure

A successful Caravel project requires the standard directory layout for the automated tools to function. This repository also adds folders for PCB, firmware, and mechanical development.

| Directory | Description |
| :--- | :--- |
| `openlane/` | Configuration files for hardening macros and the wrapper. |
| `verilog/rtl/` | Source RTL for the project. |
| `verilog/gl/` | Gate-level netlists generated after hardening. |
| `verilog/dv/` | Design verification files, including cocotb and Verilog testbenches. |
| `verilog/includes/` | Include lists used by simulation and build scripts. |
| `gds/` | Final GDSII files for fabrication. |
| `lef/` | LEF files for hardened macros. |
| `mag/` | Magic layout artifacts, if generated by the flow. |
| `spi/` | SPI/lvs-related generated data, depending on flow usage. |
| `docs/` | Project documentation, diagrams, images, measurement notes. |
| `docs/img/` | Figures and images used in this README. |
| `firmware/` | Firmware source for configuration, acquisition, and demos. |
| `pcb/` | PCB design sources and manufacturing outputs. |
| `mechanical/` | Mechanical CAD files and enclosure/flow-body design files. |
| `models/` | Sensor and behavioral models, if used for system simulation. |
| `scripts/` | Utility scripts for generation, testing, and automation. |

> **Note**
> Some directories are flow-generated, while others are project-specific. The exact structure can be refined as the repository matures.

---

## Documentation & Resources

For detailed hardware specifications and register maps, refer to the official Caravel resources:

- **[Caravel Datasheet](https://github.com/chipfoundry/caravel/blob/main/docs/caravel_datasheet_2.pdf)**  
  Electrical and physical specifications of the Caravel harness.

- **[Caravel Technical Reference Manual (TRM)](https://github.com/chipfoundry/caravel/blob/main/docs/caravel_datasheet_2_register_TRM_r2.pdf)**  
  Register maps and programming model for the management SoC.

- **[ChipFoundry Marketplace](https://platform.chipfoundry.io/marketplace)**  
  Access additional IP blocks, tools, and shuttle resources.

Project-specific documentation to be added in this repository:

- system architecture notes,
- ASIC block descriptions,
- GPIO allocation notes,
- PCB schematics and layout notes,
- mechanical design notes,
- bring-up instructions,
- airflow characterization and calibration procedures.

---

## Prerequisites

Ensure your environment meets the following requirements:

1. **Docker**  
   [Linux](https://docs.docker.com/desktop/setup/install/linux/ubuntu/) | [Windows](https://docs.docker.com/desktop/setup/install/windows-install/) | [Mac](https://docs.docker.com/desktop/setup/install/mac-install/)

2. **Python 3.8+** with `pip`

3. **Git**

4. Sufficient local disk space for:
   - PDK installation,
   - OpenLane,
   - Caravel-lite dependencies,
   - simulation artifacts.

---

## Starting Your Project

### 1. Repository Setup

Create a new repository based on the `caravel_user_project` template and clone it locally:

```bash
git clone <your-github-repo-URL>
pip install chipfoundry-cli
cd <project_name>
```

### 2. Project Initialization

> [!IMPORTANT]
> Run this first.

```bash
cf init
```

This creates `.cf/project.json` with project metadata. It must be run before any other commands such as:

- `cf setup`
- `cf gpio-config`
- `cf harden`
- `cf precheck`
- `cf verify`

### 3. Environment Setup

Install the ChipFoundry CLI tool and set up the local environment:

```bash
cf setup
```

The `cf setup` command installs:

- Caravel Lite
- management core
- OpenLane
- SKY130 PDK
- timing scripts for STA

### 4. Add Project-Specific Files

After initialization, start populating the repository with:

- project RTL in `verilog/rtl/`
- verification in `verilog/dv/`
- macro configs in `openlane/`
- docs and images in `docs/`
- PCB files in `pcb/`
- firmware in `firmware/`
- mechanical CAD in `mechanical/`

---

## Development Flow

### Hardening the Design

Hardening is the process of synthesizing your RTL and running place-and-route to generate a manufacturable GDSII layout.

#### Macro Hardening

Create a subdirectory for each custom macro under `openlane/`, each containing its configuration.

```bash
cf harden --list
cf harden <macro_name>
```

Examples of project-specific macro names may include:

- `sensor_ctrl`
- `diag_mux`
- `cfg_regs`
- `user_project_wrapper`

#### Integration

Instantiate your module(s) in:

```text
verilog/rtl/user_project_wrapper.v
```

Update the wrapper OpenLane configuration to reference your macros:

- `VERILOG_FILES_BLACKBOX`
- `EXTRA_LEFS`
- `EXTRA_GDS_FILES`

#### Wrapper Hardening

Once integration is complete:

```bash
cf harden user_project_wrapper
```

---

## GPIO Configuration

Configure the power-on default configuration for each GPIO using the interactive CLI tool.

```bash
cf gpio-config
```

This command will:

- configure GPIO pins interactively,
- update `verilog/rtl/user_defines.v`,
- generate GPIO defaults for simulation.

### GPIO Notes for This Project

This airflow-sensing project will likely require a mix of:

- digital control pins,
- optional serial interface pins,
- diagnostic outputs,
- analog-capable pins where permitted by the selected integration strategy.

Example future pin usage may include:

- SPI / I²C interface
- interrupt / status
- comparator outputs
- analog test nodes
- debug observability

> **Note**
> Final GPIO allocation should be documented in a dedicated section or separate file once the ASIC top-level is frozen.

---

## Verification Plan

The contest flow expects strong verification coverage. This repository will include:

### 1. RTL Verification
Run RTL simulation:

```bash
cf verify <test_name>
```

Run all tests:

```bash
cf verify --all
```

Planned RTL checks include:

- register map access,
- configuration sequencing,
- state machine behavior,
- mux control,
- fault / status generation,
- interface protocol handling.

### 2. Gate-Level Verification
Run gate-level simulation:

```bash
cf verify <test_name> --sim gl
```

Planned GL checks include:

- post-synthesis functional equivalence,
- wrapper-level integration,
- timing-relevant behavior under gate-level netlists.

### 3. Static Timing Analysis (STA)

```bash
make extract-parasitics
make create-spef-mapping
make caravel-sta
```

Run this if timing support scripts need refresh:

```bash
make setup-timing-scripts
```

### 4. Project-Specific System Validation
Outside the pure RTL/GDS flow, system-level validation will also include:

- PCB bring-up,
- firmware-driven configuration,
- airflow response testing,
- calibration and repeatability measurements.

---

## Local Precheck

Before shuttle submission, run local precheck to verify repository and tapeout readiness.

> [!IMPORTANT]
> GPIO configuration is required before running precheck.

```bash
cf precheck
```

You can also run specific checks or skip LVS when appropriate:

```bash
cf precheck --disable-lvs
cf precheck --checks license --checks makefile
```

Precheck should be part of the normal development cycle, not only a last-minute step.

---

## Mechanical, PCB, and Firmware Deliverables

A key part of this project is that it is **not only a silicon submission**. The final repository is intended to include open assets for the complete demonstrator.

### PCB Deliverables
Planned contents:

- schematics,
- layout source files,
- BOM,
- manufacturing outputs,
- assembly notes,
- bring-up checklist.

### Firmware Deliverables
Planned contents:

- configuration interface,
- register access helpers,
- demo acquisition flow,
- diagnostic routines,
- calibration support.

### Mechanical Deliverables
Planned contents:

- flow-body CAD,
- enclosure / mounting geometry,
- assembly guidance,
- optional drawings or printable parts for prototyping.

---

## Project Roadmap

### Phase 1 — System Definition
- finalize system partition,
- define on-chip and off-chip boundaries,
- define interfaces and pin budget,
- capture top-level architecture.

### Phase 2 — ASIC Development
- implement digital configuration and control,
- implement sensor support and diagnostics,
- build verification benches,
- integrate into the Caravel flow.

### Phase 3 — PCB and Firmware
- design support PCB,
- implement firmware for control and data collection,
- prepare bench test workflows.

### Phase 4 — Mechanical Integration
- finalize enclosure / flow path,
- integrate PCB and transducer,
- document assembly.

### Phase 5 — Validation
- electrical bring-up,
- sensor-response characterization,
- calibration procedure,
- end-to-end demo preparation.

---

## Checklist for Shuttle Submission

Before final submission, confirm the following:

### Repository
- [ ] Public GitHub repository created from the correct template
- [ ] README updated for this project
- [ ] License selected and present
- [ ] Documentation written in English
- [ ] Project structure consistent with flow expectations

### ASIC / Caravel
- [ ] `cf init` completed
- [ ] GPIO configuration completed
- [ ] Custom macros hardened
- [ ] `user_project_wrapper` integrated
- [ ] Wrapper hardened successfully
- [ ] LEF/GDS references updated
- [ ] RTL tests passing
- [ ] Gate-level tests passing
- [ ] STA run and reviewed
- [ ] `cf precheck` passing

### Deliverables
- [ ] GDSII committed or attached as required
- [ ] PCB sources included
- [ ] Firmware included
- [ ] Mechanical files included
- [ ] Demo instructions included
- [ ] Images / diagrams added to docs

---

## License

This project is intended to use a permissive open-source license compatible with contest requirements.

Current placeholder:

- **Apache 2.0**

Update this section if the project adopts another accepted license.

---

## Acknowledgments

This repository is based on the **Caravel User Project / ChipFoundry template** and adapts that flow to an open airflow sensing platform using custom SKY130 silicon.

Additional acknowledgments may be added here for:

- project collaborators,
- institutions,
- fabrication support,
- PCB and mechanical contributors,
- sensor development contributors.

---
