# NEPI-Engine
This repository contains documentation and tools for getting started with NEPI Engine, a full-featured edge-AI and automation software platform for NVIDIA Jetson and other embedded edge-compute hardware platforms.

**[Learn more about NEPI Engine](https://nepi.com/)**

## Hardware and O/S Requirements
NEPI Engine runs on lots of embedded Linux devices, though certain hardware and O/S configurations are more well-suited than others. In general, NVIDIA Jetson platforms are well tested and supported. The scripts and documentation in this repository are tailored for modern Debian-based Linux distributions (e.g., Ubuntu 18.04+); if that is not your situation, then NEPI _may_ still be appropriate wholly or in part, albeit with additional setup legwork.

**Numurus provides commercially licensed pre-built root filesystem images and turnkey processing hardware solutions for select platforms -- speed up your development time considerably by [exploring one of these options](https://nepi.com/documentation/nepi-engine-pre-built-image-download-and-installation/). Or [contact us](mailto:nepi@numurus.com) to discuss professional support options for other platforms**

The following sections describe the architecture of the NEPI Engine and provide tools and guidance for getting NEPI running on your device.

## NEPI Engine Architecture

A NEPI-enabled device provides the complete NEPI Engine suite of tools and applications. Most of these components can be enabled and disabled through system configuration, and many can also be started and stopped at run-time as needed.

NEPI Engine setup and source code is distributed across two top-level repositories:

- [nepi_engine_ws](https://github.com/nepi-engine/nepi_engine_ws) - Superproject for all NEPI Engine source code, including hardware drivers, ROS-based SDK components, user interfaces, and edge-side NEPI Connect components. Source code is organized as a collection of git submodules below this superproject. Building and running this software depends on a properly prepared root filesystem, as covered by _nepi_rootfs_tools_.

Some other stand-alone repositories may be useful depending on your needs
- [nepi_ros_interfaces](https://github.com/nepi-engine/nepi_ros_interfaces) - Collection of NEPI Engine custom ROS messages and services. Included as part of _nepi_engine_ws_, but if you are only trying to interact with an existing NEPI Engine system via the ROS interface, this repository can be included in your own workspace, built, and sourced to provide these message and service objects to the rest of your application.

- [nepi_sample_auto_scripts](https://github.com/nepi-engine/nepi_sample_auto_scripts) - Large and growing collection of NEPI Engine automation scripts that provide useful functionality and examples for the powerful NEPI Engine Automation Manager. Typically these scripts are deployed as-is to the NEPI storage partition (i.e., user partition) and/or used as references when developing new scripts.

- [nepi_rootfs_tools](https://github.com/nepi-engine/nepi_rootfs_tools) - Collection of scripts and documentation for preparing a base NEPI Engine root filesystem. This includes setting up for the NEPI fully redundant system image software update scheme and installing all dependencies (including platform-specific dependencies where applicable). Start here if you are bringing up a system from scratch... note that you'll need to switch to the appropriate ROS 1 or ROS 2 branch immediately, as detailed in the default branch README.

## Get Involved
The best way to get involved is to contribute to NEPI-Engine source code and documentation. While Numurus accepts community contributions to the NEPI Engine open-source project, contributors must submit a signed CLA before contributing code. Contributions in the form of pull requests are gladly accepted as long as we have a signed Contributor License Agreement from you or your organization. Just download the relevant agreement and follow the instructions:
- [Individual CLA](https://numurus.com/wp-content/uploads/NEPI-Engine-Individual-Contributor-License-Agreement.pdf)
- [Organization CLA](https://numurus.com/wp-content/uploads/NEPI-Engine-Organization-Contributor-License-Agreement.pdf)

## Typical System Bring-up
The following workflow describes how NEPI Engine is first deployed to a new hardware platform. It assumes
- Your target device has a Debian-based Board Support Package (BSP) and/or Operating System (O/S) available and accessible as a binary image (_img.raw_, _.bin_), compressed archive (_.tar.gz_), etc.
- Your target device has a storage media installed that allows you to add NEPI-specific partitions (typically requires 32GB for each of the NEPI A/B partitions, and ideally a large leftover space to be dedicated to the _nepi_storage_ partition)
- Your target device can be configured for internet access. 

Workflow:
1. Install the base Board Support Package (BSP) and/or Operating System (O/S) on the platform according to manufacturers instructions. In some cases, this step is already completed by the manufacturer or reseller and you can skip. A minimal install is typically sufficient, since this BSP will serve only as the NEPI _INIT_ rootfs.

1. Boot, log in, and configure the target device for internet connectivity. 

1. Follow the instructions for [installing the INIT Rootfs](https://github.com/nepi-engine/nepi_rootfs_tools#installing-the-init-rootfs) from the nepi_rootfs_tools README.

1. Follow the instructions for [installing the Main A/B Rootfs from complete image](https://github.com/nepi-engine/nepi_rootfs_tools?tab=readme-ov-file#installing-the-main-ab-rootfs-from-complete-image) from the same nepi_rootfs_tools README. For the "complete image," use the same initial BSP ROOTFS image as in step 1 above.

1. At this point, you should have rebooted/power cycled the target device and it should have booted up into a pristine copy of the BSP rootfs mounted from your NEPI_ROOTFS_A partition. You can verify with
    ```
    $ df
    ```
    ensuring that the NEPI_ROOTFS_A device is listed as "Mounted on" /

1. Now begin converting the BSP rootfs into a full-fledged NEPI rootfs by following the instructions for [constructing the main a/b rootfs from a base image](https://github.com/nepi-engine/nepi_rootfs_tools?tab=readme-ov-file#constructing-the-main-ab-rootfs-from-a-base-image) from the nepi_rootfs_tools README. You should stop after running the setup_nepi_<xyz>_rootfs.sh script and proceed from here to build NEPI s/w from source.

1. At this point you have a rootfs that has all dependencies and build-tools installed to begin building the NEPI Engine software. The _nepi_engine_ws_ provides scripts to deploy source code and build NEPI Engine software directly on the target platform. See the README in that repository
[nepi_engine_ws](https://github.com/nepi-engine/nepi_engine_ws)

## Alternatives
This section details approaches to overcoming some target platform limitations. In most cases these alternatives have been successfully employed by Numurus in porting NEPI to specific platforms.

### Target device has no internet capability
If you cannot run setup scripts that install NEPI Engine dependencies because the target device has no internet capability, it is often possible to mount the BSP root filesystem on a Linux development host PC using the relevant _qemu_ package to provide an emulator engine and then _chroot_ to the mountpoint and install external dependencies, etc. via the host PC internet connection using the regular nepi_rootfs_tools setup scripts.

Because there is different preparation for the NEPI _init_ and _main_ rootfs, you should maintain a pristine copy of the BSP rootfs at all times and make copies that you modify into. Beware, this approach usually requires considerable disk space on the development machine to maintain the various rootfs images.

### Target device does not have user-partionable installation media
In cases where there is a strictly prescribed partition layout for the sole storage device, it is sometimes possible to reconfigure the partition tables such that the BSP deployment process creates the necessary NEPI_ROOTFS_<A,B> and DATA partitions. This is how NEPI is installed on [Jetson Orin-NX on the SSD](https://github.com/nepi-engine/nepi_rootfs_tools/blob/master/dev_host_tools/jetson_initrd_flash_support/README_nepi_initrd_flash.txt) alongside all the BSP partitions. See manufacturer documentation to determine if this is a viable path for your platform.

It is also possible to bypass the NEPI A/B filesystem support and convert the sole BSP rootfs into a NEPI main rootfs by running the appropriate _setup_nepi_rootfs.sh_ script right after deploying the BSP rootfs. Note that this lead to some error messages in NEPI logs and an inability to use the NEPI full system image update capabilities, but generally provides a well-featured NEPI Engine deployment.

### BSP Rootfs is a .tar.gz, not a binary image file
This will only impact the way you write the initial contents to the NEPI_ROOTFS_A and NEPI_ROOTFS_B partitions. In general, you'll replace the _dd_ call with a _tar -xf_ call.
