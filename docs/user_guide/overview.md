# User Guide

This guide explains how to install, configure, and run MONICA, from a local single simulation to distributed execution on a computing cluster.

## Choose where to start

### Install MONICA

If MONICA is not yet available on your computer, begin with the installation instructions for your operating system:

- [Install MONICA on Windows](installation/windows_installation.md) using the distributed Windows package.
- [Compile MONICA on Windows](installation/windows_compilation.md) if you need to build it from source.
- [Compile MONICA on Linux](installation/linux_installation.md) if you use a Linux system.
- See [Python support and dependencies](installation/dependencies_and_python.md) when using the optional Python tools and examples.

### Choose a MONICA version

See [MONICA versions](versions.md) for the public release history, Git tags, release dates, main changes, and compatibility guidance. For reproducible simulations, record the complete Git tag together with the configuration, parameter data, and climate inputs used.

### Run MONICA

Choose the method that matches your environment:

- [Command line](how_to_run/command_line.md) for a local simulation with the MONICA executable.
- [Docker](how_to_run/docker.md) for a container-based deployment.
- [Python producer-consumer pipeline](how_to_run/python_producer_consumer.md) for scripted or distributed workflows.
- [Rundeck](how_to_run/rundeck.md) for MONICA users with access to the ZALF Rundeck service.

### Configure a simulation

A MONICA simulation is controlled by configuration files describing the simulation, site, crop, and management operations. Start with the [configuration overview](configuration_files/config_overview.md), then consult the reference for each JSON file:

- [`sim.json`](configuration_files/config_sim_json.md) defines general simulation settings, climate input, and outputs.
- [`site.json`](configuration_files/config_site_json.md) describes the site, soil profile, and environmental conditions.
- [`crop.json`](configuration_files/config_crop_json.md) describes crops and cultivation worksteps.

### Run MONICA at scale

The advanced guides cover server-based and distributed execution:

- [MONICA server](advanced/monica_server.md) explains server modes and ZeroMQ communication.
- [HPC clusters](advanced/hpc.md) explains running multiple MONICA instances.
- [Singularity](advanced/singularity.md) explains containerized execution on HPC systems.

## Typical workflow

1. Install MONICA or compile it for your operating system.
2. Run one of the supplied example simulations and verify that an output file is created.
3. Read the configuration overview before adapting the input files.
4. Modify the site, crop, management, climate, and output settings for your simulation.
5. Use Docker, the MONICA server, or an HPC workflow when scaling beyond local runs.

## Troubleshooting

If installation or execution fails, consult [Installation and setup issues](troubleshooting/troubleshooting_installation.md) for common causes and solutions.
