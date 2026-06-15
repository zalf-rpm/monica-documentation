# FAQ & Support

This page provides answers to common questions about MONICA and guides you to the right resources for getting help, reporting issues, or contributing to the project.

---

## Frequently Asked Questions

### General

**What is MONICA?**
MONICA (Model of Nitrogen and Carbon dynamics in Agro-ecosystems) is a process-based crop and soil model developed at the Leibniz Centre for Agricultural Landscape Research (ZALF). It simulates crop growth, soil water, nitrogen and carbon dynamics on a daily time step.

**Is MONICA free to use?**
Yes. MONICA is open source and distributed under the **Mozilla Public License v2.0 (MPL-2.0)**. The source code, parameter files, and documentation are all freely available on GitHub.

**What crops does MONICA support?**
MONICA supports a wide range of arable crops including winter and spring wheat, maize, barley, rye, rapeseed, potato, sugar beet, soybean, and grass ley, among others. The full list of available crop parameter files can be found in the [monica-parameters repository](https://github.com/zalf-rpm/monica-parameters/tree/master/crops).

**What operating systems does MONICA run on?**
MONICA runs on **Windows** and **Linux**. Pre-built binaries are available for both platforms on the [releases page](https://github.com/zalf-rpm/monica/releases). macOS is not officially supported but may work with manual compilation.

**Where can I find the MONICA parameter files?**
All parameter files (crops, soils, fertilisers, residues) are maintained in the [monica-parameters repository](https://github.com/zalf-rpm/monica-parameters) on GitHub.

---

### Installation & Setup

**How do I install MONICA?**
Starting with version 3.6.x, MONICA is distributed as a ZIP archive — simply download and extract it, no installer required. Download the latest release from the [MONICA releases page](https://github.com/zalf-rpm/monica/releases) and extract it to a convenient location (e.g. `C:\monica\`). A quick start guide for Windows users is also available on the [Overview](../../) page of this documentation.

For full step-by-step installation instructions, see the **Installation and Setup** section of the User Guide:

- [Windows installation](../../user_guide/Installation_and_Setup/windows_installation/)
- [Linux installation](../../user_guide/Installation_and_Setup/linux_installation/)
- [MONICA server setup](../../user_guide/Installation_and_Setup/monica_server/)
- [Rundeck setup](../../user_guide/Installation_and_Setup/rundeck_setup/)
- [Dependencies & Python requirements](../../user_guide/Installation_and_Setup/dependencies_and_python/)
- [Troubleshooting common installation issues](../../user_guide/Installation_and_Setup/troubleshooting_installation/)

**How do I compile MONICA from source?**
Compilation instructions are available for both platforms:

- [How to compile MONICA (Windows)](https://github.com/zalf-rpm/monica/wiki/How-to-compile-MONICA-(Windows))
- [How to compile MONICA (Linux)](https://github.com/zalf-rpm/monica/wiki/How-to-compile-MONICA-(Linux))

**How do I run MONICA as a standalone command-line application?**
See the [Running MONICA from the Command Line](../../user_guide/how_to_run/command_line/) page in this documentation.

**How do I run MONICA on a high performance computer (HPC)?**
See the [Running on HPC](../../user_guide/how_to_run/hpc/) page in this documentation. Basic knowledge of how MONICA works is assumed in the HPC guide.

**Where can I learn about MONICA's configuration file format?**
The Configuration Files section of this documentation covers all input file formats in detail, including [sim.json](../../user_guide/configuration_files/config_sim_json/), [crop.json](../../user_guide/configuration_files/config_crop_json/), and [site.json](../../user_guide/configuration_files/config_site_json/).

---

### Model Use

**Can MONICA be used via Python?**
Yes. MONICA can be called from Python scripts, both as a local process and as a server-client setup. See the [Python API Usage](../../user_guide/how_to_run/python_api_usage/) page in this documentation for details and examples.

**Can MONICA be run in Docker?**
Yes. MONICA is available as a Docker image. See the [Running MONICA in Docker](../../user_guide/how_to_run/docker/) page in this documentation for instructions.

**What is the difference between species and cultivar parameters?**
Species parameters define physiological properties shared across all cultivars of a crop (e.g. photosynthetic pathway, organ structure). Cultivar parameters define properties specific to a particular variety (e.g. temperature sum requirements, frost tolerance). Both are required for a simulation.

**Where can I find older versions of MONICA?**
Information about the different versions of MONICA and their changes can be found on the [MONICA versions](https://github.com/zalf-rpm/monica/wiki/Monica-versions) wiki page.

---

## Getting Support

### Documentation

The official MONICA documentation is available at:
[https://zalf-rpm.github.io/monica-documentation/](https://zalf-rpm.github.io/monica-documentation/)

An older version of the documentation is also available in the [GitLab MONICA documentation repository](https://gitlab.com/zalf-rpm/monica-docs/blob/master/Readme.md).

---

### Mailing List / Google Group

For discussions, questions, and help regarding MONICA, join the **MONICA Google Group**:
[https://groups.google.com/forum/#!forum/zalf-rpm-monica](https://groups.google.com/forum/#!forum/zalf-rpm-monica)

Anybody can read the list, but you need to be an accepted member to post messages.

---

### Bug Reports & Feature Requests

If you find a bug in MONICA, have an improvement idea, or want to request a new feature, please open an issue on the GitHub issue tracker:
[https://github.com/zalf-rpm/monica/issues](https://github.com/zalf-rpm/monica/issues)

Please include as much detail as possible including your operating system, MONICA version, input files (if relevant), and a description of the unexpected behaviour.

---

### Contributing

If you would like to help develop MONICA, fix bugs, or offer improvements, please use the GitHub Pull Request mechanism:
[https://github.com/zalf-rpm/monica/pulls](https://github.com/zalf-rpm/monica/pulls)

Contributions must be compatible with the **Mozilla Public License v2.0 (MPL-2.0)** under which MONICA is distributed.

---

### Contact

MONICA is developed and maintained by the Research Platform **"Data Analysis & Simulation"** at the **Leibniz Centre for Agricultural Landscape Research (ZALF)**, Müncheberg, Germany.

For direct enquiries, you can reach the team via the Google Group above or through the GitHub issue tracker.