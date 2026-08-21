# FAQ & Support

This page answers common questions about MONICA and points you to resources for installing and running the model, getting help, reporting issues, and contributing to the project.

---

## Frequently Asked Questions

### General

**What is MONICA?**

MONICA (Model for Nitrogen and Carbon dynamics in Agro-ecosystems) is a dynamic, process-based crop and soil model developed at the Leibniz Centre for Agricultural Landscape Research (ZALF). It simulates crop growth, soil water, nitrogen and carbon dynamics on a daily time step.

For a general introduction to the model and its scientific foundations, see the [MONICA Overview](../index.md).

**Is MONICA free to use?**

Yes. MONICA is open-source software distributed under the **Mozilla Public License v2.0 (MPL-2.0)**. 

The project resources are publicly available on GitHub:

- [MONICA source code](https://github.com/zalf-rpm/monica)
- [MONICA parameter sets](https://github.com/zalf-rpm/monica-parameters)
- [MONICA documentation](https://github.com/zalf-rpm/monica-documentation)

Review the license terms before redistributing MONICA or publishing modified source files.

**Which crops can MONICA simulate?**

The MONICA parameter repository contains parameter sets for numerous annual and perennial crops, including winter and spring wheat, maize, barley, rye, rapeseed, potato, sugar beet, soybean, tomato, and several grass and grass-legume mixtures. 

See the [crops directory in the monica-parameters repository](https://github.com/zalf-rpm/monica-parameters/tree/master/crops) for the available species and cultivar parameter files.

The presence of a parameter set does not guarantee that it has been calibrated or validated for every location, climate, soil, or application. Users should evaluate model performance against suitable observations before applying a parameter set in a new context.

**Which operating systems does MONICA support?**

MONICA runs on **Windows** and **Linux**. 

A pre-built Windows ZIP package is available from the [MONICA Releases page](https://github.com/zalf-rpm/monica/releases). Linux users currently compile MONICA from source. macOS is not officially supported or documented.

**Where can I find the MONICA parameter files?**

The official parameter sets are maintained in the [monica-parameters repository](https://github.com/zalf-rpm/monica-parameters).

The repository contains:

- crop species and cultivar parameters
- crop residue parameters
- soil parameters
- mineral fertiliser parameters
- organic fertiliser parameters
- general model parameters
- example project configurations

---

### Installation & Build

**How do I install MONICA on Windows?**

Starting with MONICA 3.6.12, the Windows version is distributed as a ZIP archive rather than through a traditional installer.

Download the latest `monica_win64_<version>.zip` package from the [MONICA Releases page](https://github.com/zalf-rpm/monica/releases), then extract it to a convenient location, for example: `C:\monica\`. No installation program is required. A Windows quick start guide is also available on the [Overview](../index.md) page.

For detailed instructions, see:

- [Windows installation](../user_guide/installation/windows_installation.md)
- [Windows compilation](../user_guide/installation/windows_compilation.md)
- [Linux compilation](../user_guide/installation/linux_installation.md)
- [Python support and dependencies](../user_guide/installation/dependencies_and_python.md)
- [Troubleshooting installation and setup](../user_guide/troubleshooting/troubleshooting_installation.md)

**How do I install MONICA on Linux?**

A pre-built Linux package is not currently provided. Linux users must compile MONICA from source. See [Linux compilation](../user_guide/installation/linux_installation.md) for prerequisites and build instructions.

**How do I compile MONICA from source?**

Use the maintained compilation guides in this documentation:

- [Compile MONICA on Windows](../user_guide/installation/windows_compilation.md)
- [Compile MONICA on Linux](../user_guide/installation/linux_installation.md)

The MONICA source repository and the `monica-parameters` repository are required. Additional build dependencies are described in the platform-specific guides.

---

### Running MONICA

**How do I run MONICA as a standalone command-line application?**

Use the `monica-run` executable with a simulation configuration file: `monica-run path/to/sim.json`

See [Running MONICA from the Command Line](../user_guide/how_to_run/command_line.md) for command-line options and examples..

**Can MONICA be run with Python?**

Yes. Python scripts can prepare simulation jobs and communicate with MONICA workers using the documented producer-consumer pipeline.

For standalone workflows, Python can also invoke the `monica-run` executable as an external process.

See [Running MONICA with the Python producer-consumer pipeline](../user_guide/how_to_run/python_producer_consumer.md) for requirements and examples.

**Can MONICA run in Docker?**

Yes. The project publishes the `zalfrpm/monica-cluster` Docker image for running MONICA workers and associated proxy services.

See [Running MONICA in Docker](../user_guide/how_to_run/docker.md) for available ports, environment variables, and example commands.

**How do I run MONICA on an HPC cluster?**

See the [Running on HPC clusters](../user_guide/advanced/hpc.md). The HPC guide assumes that you already understand MONICA configuration files and the basic producer-worker-consumer workflow.

Related documentation includes:

- [MONICA server](../user_guide/advanced/monica_server.md)
- [Rundeck](../user_guide/how_to_run/rundeck.md)
- [Singularity](../user_guide/advanced/singularity.md)

---

### Configuration and Parameters

**Where is MONICA's configuration file format documented?**

The Configuration section describes MONICA's main input files:

- [Configuration overview](../user_guide/configuration_files/config_overview.md)
- [sim.json](../user_guide/configuration_files/config_sim_json.md)
- [crop.json](../user_guide/configuration_files/config_crop_json.md)
- [site.json](../user_guide/configuration_files/config_site_json.md)

A typical simulation also requires climate data and references to the necessary model parameter sets.

**What is the difference between species and cultivar parameters?**

Species parameters define characteristics shared by the cultivars of a crop species. Examples include:

- photosynthetic pathway
- crop organ structure
- initial organ biomass
- root growth characteristics
- nitrogen concentration parameters

Cultivar parameters describe characteristics that may differ among cultivars or crop types within a species. Examples include:

- thermal time requirements
- vernalisation and day-length requirements
- maximum assimilation rate
- biomass allocation
- heat and frost tolerance
- yield components

Cultivar parameters complement rather than replace the corresponding species parameters. A crop simulation normally requires both.

See:

- [Species parameters](../parameters/crop_parameters/species_parameters.md)
- [Cultivar parameters](../parameters/crop_parameters/cultivar_parameters.md)

**Where can I find previous MONICA releases?**

Published versions and their downloadable assets are available on the [MONICA Releases page](https://github.com/zalf-rpm/monica/releases).

The complete source code history is available through the repository's [tags](https://github.com/zalf-rpm/monica/tags) and commit history. When reporting results, record the exact MONICA version and parameter set revision used in the simulation.

---

## Getting Support

### Documentation

The official MONICA documentation is available at:
[https://zalf-rpm.github.io/monica-documentation/](https://zalf-rpm.github.io/monica-documentation/)

The source files for this documentation are maintained in the [monica-documentation repository](https://github.com/zalf-rpm/monica-documentation).

A legacy version of the documentation is preserved in the [GitLab MONICA documentation repository](https://gitlab.com/zalf-rpm/monica-docs/blob/master/Readme.md). Because this material may describe older MONICA versions, prefer the current documentation unless you are working with a legacy release.

---

### Mailing List & Google Group

The MONICA Google Group is available for discussions and questions:
[https://groups.google.com/g/zalf-rpm-monica](https://groups.google.com/g/zalf-rpm-monica)

Messages can be read publicly. Membership approval is required before posting.

When asking for help, include the MONICA version, operating system, execution method, relevant error messages, and a minimal example if possible.

---

### Bug Reports & Feature Requests

Use the [MONICA GitHub issue tracker](https://github.com/zalf-rpm/monica/issues) to report reproducible bugs, suggest improvements, or request features.

Before opening an issue, search the existing issues to see whether the problem has already been reported.

A useful bug report should include:

- MONICA version or Git commit
- parameter set version or Git commit
- operating system
- execution method, such as command line, Docker, or server
- minimal input files needed to reproduce the problem
- exact command or script used
- relevant logs and error messages
- expected and observed behavior

Do not include confidential or sensitive data in a public issue.

---

### Contributing

Contributions to MONICA are welcome. Use the GitHub pull-request workflow: [https://github.com/zalf-rpm/monica/pulls](https://github.com/zalf-rpm/monica/pulls)

Before beginning a substantial change, consider opening an issue to discuss the proposed implementation with the maintainers.

Contributions must be compatible with the **Mozilla Public License v2.0 (MPL-2.0)** under which MONICA is distributed.

Include appropriate tests and documentation when changing model behavior, configuration formats, or public interfaces.

---

### Contact

MONICA is developed and maintained by the Research Platform [**Simulation and Data Science**](https://www.zalf.de/en/struktur/pb4/Pages/default.aspx) and [**Computation and Data Service Platform**](https://www.zalf.de/en/struktur/cdp/Pages/default.aspx) at the [**Leibniz Centre for Agricultural Landscape Research (ZALF)**](https://www.zalf.de/en/Pages/ZALF.aspx) in Müncheberg, Germany.

Use the following project channels:

- General questions and discussions: [MONICA Google Group](https://groups.google.com/g/zalf-rpm-monica)
- Bugs and feature requests: [GitHub issue tracker](https://github.com/zalf-rpm/monica/issues)
- Source code contributions: [GitHub pull requests](https://github.com/zalf-rpm/monica/pulls)

Authors:

- [Claas Nendel](https://www.zalf.de/en/ueber_uns/mitarbeiter/pages/Nendel_C.aspx)
- [Xenia Specka](https://www.zalf.de/en/ueber_uns/mitarbeiter/pages/Specka_X.aspx)
- [Michael Berg-Mohnicke](https://www.zalf.de/en/ueber_uns/mitarbeiter/pages/Berg_M.aspx)