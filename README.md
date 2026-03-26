<div align="center">

<img src="https://umsousercontent.com/lib_lnlnuhLgkYnZdkSC/hj0vk05j0kemus1i.png" alt="ChipFoundry Logo" height="140" />

# Open-Source Smart Airflow Sensor Platform with Custom SKY130 Silicon

**A Caravel-based mixed-system reference design for hot-wire airflow sensing**

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Process](https://img.shields.io/badge/Process-SKY130-0A7EA4.svg)]()
[![Platform](https://img.shields.io/badge/Platform-Caravel-4C6EF5.svg)]()
[![Status](https://img.shields.io/badge/Status-Concept%20%2F%20Development-orange.svg)]()
[![ChipFoundry Marketplace](https://img.shields.io/badge/ChipFoundry-Marketplace-6E40C9.svg)](https://platform.chipfoundry.io/marketplace)

</div>

---

## Table of Contents
- [Overview](#overview)
- [Why This Project Matters](#why-this-project-matters)
- [Current Prototype Baseline](#current-prototype-baseline)
- [System Images](#system-images)
- [Project Goals](#project-goals)
- [System Architecture](#system-architecture)
- [On-Chip vs Off-Chip Partition](#on-chip-vs-off-chip-partition)
- [ASIC Block Table](#asic-block-table)
- [Why This Scope Is Realistic](#why-this-scope-is-realistic)
- [Target Applications](#target-applications)
- [Repository Structure](#repository-structure)
- [Documentation & Resources](#documentation--resources)
- [Prerequisites](#prerequisites)
- [Starting Your Project](#starting-your-project)
- [Development Flow](#development-flow)
- [GPIO Configuration](#gpio-configuration)
- [Verification Plan](#verification-plan)
- [Mechanical, PCB, and Firmware Deliverables](#mechanical-pcb-and-firmware-deliverables)
- [Project Roadmap](#project-roadmap)
- [Checklist for Shuttle Submission](#checklist-for-shuttle-submission)
- [Project Status](#project-status)
- [License](#license)
- [Acknowledgments](#acknowledgments)

---

## Overview

This repository contains an **open-source airflow sensing reference design** built around:

- a **hot-wire thermal flow transducer**,
- a **custom SKY130 chip** integrated as a **Caravel user project**,
- a **support PCB**,
- **firmware** for configuration and data collection,
- and a **mechanical flow body / enclosure**.

The project demonstrates how **custom silicon** can improve a real airflow sensing platform by reducing board complexity, improving configurability, enabling diagnostics, and creating a more reproducible path from prototype to deployable system.

This project is intended as an **open reference design demonstrator**, not as a finished certified medical device.

---

## Why This Project Matters

Many airflow sensing systems work well in the lab but remain difficult to reproduce because they depend on:

- discrete analog front ends,
- manual trimming,
- PCB parasitics,
- ad-hoc packaging,
- and undocumented calibration workflows.

This project addresses that gap by co-designing:

1. the **thermal transducer**,
2. the **custom silicon interface**,
3. the **PCB and firmware**,
4. and the **mechanical flow body**.

The result is meant to be a **complete open platform**, not only a chip or only a sensor.

---

## Current Prototype Baseline

The current prototype already demonstrates the main system concept:

- hot-wire airflow sensing,
- microfabricated heated transducer,
- discrete analog electronics,
- packaged flow body,
- and a complete bench-testable assembly.

Current baseline characteristics:

- **Supply:** ±5 V  
- **Analog output:** 0–5 V  
- **Flow range:** 0–160 L/min  
- **Mechanical interface concept:** 22 mm  

These values describe the existing prototype baseline and may evolve during the contest-driven redesign.

---

## System Images

### 1. Current Prototype Assembly

> Place your current product image at: `docs/img/prototype_sensor.jpg`

![Prototype Assembly](docs/img/prototype_sensor.jpg)

### 2. System Architecture Diagram

> Place your architecture diagram at: `docs/img/system_architecture.png`

![System Architecture](docs/img/system_architecture.png)

### 3. ASIC Partition / On-Chip vs Off-Chip Diagram

> Place your partition diagram at: `docs/img/asic_partition.png`

![ASIC Partition](docs/img/asic_partition.png)

### 4. Optional PCB Image

> Optional: place a PCB image at: `docs/img/pcb_top.jpg`

![Support PCB](docs/img/pcb_top.jpg)

---

## Project Goals

The first contest version aims to deliver:

- an open airflow sensing demonstrator,
- a custom SKY130 user project integrated in Caravel,
- a support PCB for off-chip precision and system functions,
- firmware for control, status, and data capture,
- and an open mechanical assembly for airflow testing.

The key objective is to prove that **partial silicon integration** already provides value by improving:

- reproducibility,
- modularity,
- digital configurability,
- diagnostics,
- and long-term platform scalability.

---

## System Architecture

The platform is composed of four layers.

### 1. Thermal Flow Transducer
A hot-wire airflow transducer based on a heated microelement and auxiliary thermal sensing structures.

### 2. Custom Silicon
A SKY130 chip integrated in the Caravel user area. The chip provides control, diagnostics, channel selection, digital configuration, and system observability.

### 3. Support Electronics
A PCB containing the blocks intentionally kept off-chip in revision 1, such as precision references, high-resolution conversion, power conditioning, host interface, and protection.

### 4. Mechanical Platform
A reproducible flow body and enclosure that supports assembly, bench testing, and future application-specific integration.

---

## On-Chip vs Off-Chip Partition

The most important design decision in this project is the partition between what is integrated in silicon and what remains external in revision 1.

### On-Chip Focus
The ASIC will focus on blocks that provide **high integration value** with **manageable implementation risk**:

- excitation control,
- signal routing,
- digital configurability,
- threshold diagnostics,
- fault reporting,
- and test visibility.

### Off-Chip Focus
The PCB will retain the blocks that are either:

- high precision,
- high risk for a first shuttle,
- strongly application-dependent,
- or easier to validate externally in revision 1.

This partition keeps the project ambitious, but still realistic.

---

## ASIC Block Table

| Block | Location | Main Function | Why It Matters | Risk Level | Revision 1 Plan |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Hot-wire excitation control | On-chip | Programmable control of the heater drive or heater-control path | Enables silicon-level control and repeatable operating modes | Medium | Include |
| Analog channel multiplexer | On-chip | Selects sensor / auxiliary thermal / test channels | Reduces external routing complexity and improves observability | Low | Include |
| Diagnostic comparators | On-chip | Threshold detection for status, fault, or direction-related conditions | Adds practical system intelligence with modest complexity | Low | Include |
| Bias and trim control | On-chip | Internal programmable settings for operating modes and tuning | Improves repeatability and calibration flexibility | Medium | Include |
| Configuration registers | On-chip | Stores system configuration and control bits | Core digital infrastructure for the platform | Low | Include |
| SPI or I²C slave | On-chip | Communication with external host | Makes the ASIC easy to configure and demonstrate | Low | Include |
| Startup / control FSM | On-chip | Initializes and sequences the internal blocks | Simplifies bring-up and deterministic operation | Low | Include |
| Fault / status flags | On-chip | Reports internal and sensor-related conditions | Useful for demo, debug, and future system robustness | Low | Include |
| Test / observability hooks | On-chip | Exposes internal states and nodes for verification | Critical for bring-up and mixed-system debug | Low | Include |
| Precision reference | Off-chip | Stable reference for accurate measurement chain | Precision block better kept external in first shuttle | Medium | External |
| High-resolution ADC | Off-chip | Converts conditioned analog data for final measurement | Higher complexity and verification effort | High | External |
| Final analog gain stage | Off-chip | Precision conditioning of the sensed signal | Better controlled on PCB for first revision | Medium | External |
| Power regulation / filtering | Off-chip | Supplies clean rails to system blocks | Board-level requirement, easier to iterate externally | Low | External |
| Host MCU / data logger | Off-chip | System supervision, data capture, UI/demo | Better handled outside ASIC in revision 1 | Low | External |
| ESD / system-level protection | Off-chip | Protects the complete platform and connectors | System-dependent and board-specific | Low | External |

---

## Why This Scope Is Realistic

This project is intentionally scoped so that the custom silicon does **not** attempt to replace the entire precision analog front end in the first tapeout.

That makes the proposal stronger for a contest-oriented flow because it:

- focuses the ASIC on functions that are highly demonstrable,
- keeps digital verification central,
- limits precision-analog risk,
- preserves flexibility on the support PCB,
- and creates a cleaner path to a tapeout-ready deliverable.

In other words, revision 1 is designed to be a **credible platform demonstrator**, not an overextended one-shot full-custom instrument ASIC.

---

## Target Applications

This project is positioned as an **open airflow sensing platform** for:

- industrial airflow monitoring,
- embedded gas-flow instrumentation,
- laboratory setups,
- educational platforms,
- edge sensing and telemetry nodes,
- research systems,
- respiratory-flow experimentation in non-certified environments.

---

## Repository Structure

A successful Caravel project requires the standard directory layout for the automated tools to function. This repository also adds folders for PCB, firmware, and mechanical development.

| Directory | Description |
| :--- | :--- |
| `openlane/` | Configuration files for hardening macros and the wrapper |
| `verilog/rtl/` | Source RTL for the project |
| `verilog/gl/` | Gate-level netlists generated after hardening |
| `verilog/dv/` | Design verification files, including cocotb and Verilog testbenches |
| `verilog/includes/` | Include lists for simulation and build scripts |
| `gds/` | Final GDSII files for fabrication |
| `lef/` | LEF files for hardened macros |
| `mag/` | Magic layout artifacts, if generated |
| `docs/` | Project documentation, diagrams, and notes |
| `docs/img/` | Images used in this README |
| `firmware/` | Firmware source for configuration and demo operation |
| `pcb/` | PCB source files and manufacturing outputs |
| `mechanical/` | Mechanical CAD files and enclosure / flow-body design files |
| `models/` | Behavioral or sensor models for simulation |
| `scripts/` | Utility scripts for automation and testing |

---

## Documentation & Resources

Official Caravel / ChipFoundry references:

- **[Caravel Datasheet](https://github.com/chipfoundry/caravel/blob/main/docs/caravel_datasheet_2.pdf)**  
- **[Caravel Technical Reference Manual (TRM)](https://github.com/chipfoundry/caravel/blob/main/docs/caravel_datasheet_2_register_TRM_r2.pdf)**  
- **[ChipFoundry Marketplace](https://platform.chipfoundry.io/marketplace)**  

Project-specific documentation planned for this repository:

- system architecture notes,
- ASIC block descriptions,
- GPIO allocation notes,
- PCB schematics and layout notes,
- mechanical design notes,
- bring-up instructions,
- airflow characterization and calibration procedures.

---

## Prerequisites

Ensure your environment includes:

1. **Docker**  
2. **Python 3.8+** with `pip`  
3. **Git**  
4. Enough disk space for:
   - SKY130 PDK,
   - OpenLane,
   - Caravel-lite,
   - simulation and hardening outputs.

---

## Starting Your Project

### 1. Repository Setup

```bash
git clone <your-github-repo-URL>
pip install chipfoundry-cli
cd <project_name>
