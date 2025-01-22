# NEPI-Engine
This repository contains documentation and tools for getting started with NEPI Engine, a full-featured edge-AI and automation software platform for NVIDIA Jetson and other embedded edge-compute hardware platforms.

**[Learn more about NEPI Engine](https://nepi.com/)**

## Hardware and O/S Requirements
NEPI Engine runs on lots of embedded Linux devices, though certain hardware and O/S configurations are more well-suited than others. In general, NVIDIA Jetson platforms are well tested and supported. The scripts and documentation in this repository are tailored for modern Debian-based Linux distributions (e.g., Ubuntu 18.04+); if that is not your situation, then NEPI _may_ still be appropriate wholly or in part, albeit with additional setup legwork.

**Numurus provides commercially licensed pre-built root filesystem images and turnkey processing hardware solutions for select platforms -- speed up your development time considerably by [exploring one of these options](https://nepi.com/documentation/nepi-engine-pre-built-image-download-and-installation/). Or [contact us](mailto:nepi@numurus.com) to discuss professional support options for other platforms**

The following sections describe the architecture of the NEPI Engine and provide tools and guidance for getting NEPI running on your device.

## NEPI Engine Architecture

A NEPI-enabled device provides the complete NEPI Engine suite of tools and applications. Most of these components can be enabled and disabled through system configuration, and many can also be started and stopped at run-time as needed.

The entire NEPI software source code, including the nepi engine system, nepi applications, ai frameworks, drivers, and build scripts, is available in the top-level "nepi_engine_ws" repository:

- [nepi_engine_ws](https://github.com/nepi-engine/nepi_engine_ws) - Superproject for all NEPI Engine source code, including hardware drivers, ROS-based SDK components, user interfaces, and edge-side NEPI Connect components. Source code is organized as a collection of git submodules below this superproject. Building and running this software depends on a properly prepared root filesystem, as covered by _nepi_rootfs_tools_.

The nepi_engine_ws includes the following nepi component repos
- [nepi_engine](https://github.com/nepi-engine/nepi_engine) - The complete NEPI engine operating environment including NEPI SDK, NEPI Managers, NEPI ROS Environment, NEPI ROS Interfaces, and NEPI Resident User Interface (RUI)

- [nepi_drivers](https://github.com/nepi-engine/nepi_drivers) - Collection of NEPI driver interfaces for sensors and control devices. These driver interfaces abstract the hardware interface into NEPI standard interfaces allowing downstream applications to interact with the hardware without needing to know any specific details about the specific hardware interfaces.

- [nepi_apps](https://github.com/nepi-engine/nepi_apps) - Collection of NEPI applications that expand the capabilities and features of the base NEPI software environment.

- [nepi_ai_frameworks](https://github.com/nepi-engine/nepi_ai_frameworks) - Collection of NEPI ai framework interfaces for loading and running ai detection models.

- [nepi_3rd_party](https://github.com/nepi-engine/nepi_3rd_party) - Collection of 3rd party provided repositories used by the NEPI Engine system.  
- 

Some other stand-alone repositories may be useful depending on your needs
- [nepi_ros_interfaces](https://github.com/nepi-engine/nepi_ros_interfaces) - Collection of NEPI Engine custom ROS messages and services. Included as part of _nepi_engine_ws_, but if you are only trying to interact with an existing NEPI Engine system via the ROS interface, this repository can be included in your own workspace, built, and sourced to provide these message and service objects to the rest of your application.

- [nepi_sample_auto_scripts](https://github.com/nepi-engine/nepi_sample_auto_scripts) - Collection of NEPI Engine automation scripts that provide useful functionality and examples for the powerful NEPI Engine Automation Manager. Typically these scripts are deployed as-is to the NEPI storage partition (i.e., user partition) and/or used as references when developing new scripts.

## Get Involved
The best way to get involved is to contribute to NEPI-Engine source code and documentation. While Numurus accepts community contributions to the NEPI Engine open-source project, contributors must submit a signed CLA before contributing code. Contributions in the form of pull requests are gladly accepted as long as we have a signed Contributor License Agreement from you or your organization. Just download the relevant agreement and follow the instructions:
- [Individual CLA](https://numurus.com/wp-content/uploads/NEPI-Engine-Individual-Contributor-License-Agreement.pdf)
- [Organization CLA](https://numurus.com/wp-content/uploads/NEPI-Engine-Organization-Contributor-License-Agreement.pdf)


