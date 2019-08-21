# MONICA - The Model for Nitrogen and Carbon in Agro-ecosystems

MONICA is a dynamic, process-based simulation model which describes transport and bio-chemical turn-over of carbon, nitrogen and water in agro-ecosystems. On daily time steps the most important processes in soil and plant are modelled mechanistically. They are linked in such way that feed-back relations of the single processes are reproduced as close to nature as possible. MONICA works one dimensional and represents a space of 1 m² surface area and 2 m depth. 

The acronym MONICA is derived from „MOdel of Nitrogen and Carbon dynamics in Agro-ecosystems”.

This Wiki contains information about the software development of MONICA as well as its use. Additional information of the model (processes, installation of version 1.2) can be found on [http://monica.agrosystem-models.com/](http://monica.agrosystem-models.com/).

## Getting help / helping

### Dokumentation

The documentation of the MONICA processes can be found in our GitLab repository. [MONICA documentation](https://gitlab.com/zalf-rpm/monica-docs/blob/master/Readme.md)

### Mailing list

A mailing list for discussions and help regarding MONICA can be found in the [MONICA Google group](https://groups.google.com/forum/#!forum/zalf-rpm-monica). Everybody can read the list, but only accepted members of the group can post messages.

### GitHub issues

If you find bugs in MONICA, has improvement ideas or requests for enhancements, please use the [GitHub issue tracker](https://github.com/zalf-lsa/monica/issues).

### helping / Pull requests

If you'd like to help developing MONICA, fix bugs or offer improvements, please use the [GitHub Pull request](https://github.com/zalf-lsa/monica/pulls) mechanism.


## History

Some information about the different versions of MONICA can be found here [MONICA versions](wiki/Monica-versions).

## Compile MONICA

How to compile MONICA?[(Windows)](https://github.com/zalf-rpm/monica/wiki/How-to-compile-MONICA-(Windows)) / [(Linux)](https://github.com/zalf-rpm/monica/wiki/How-to-compile-MONICA-(Linux))

An older german version of a compile how to can be found here [How to compile MONICA? (old)](wiki/How-to-compile-MONICA). How to compile the current version also be found in the [README](https://github.com/zalf-lsa/monica/blob/master/README.md) in the repository.

## Use MONICA

MONICA can be used in two broad ways, as a standalone commandline application and with scripts which will send jobs to a cloud of MONICA worker processes.

How to use MONICA as a standalone commandline application is described [here](wiki/How-to-run-standalone-CMD-MONICA). 

To find a description of how to use MONICA on high performance computers or with multiple machines look [here](wiki/How-to-run-HPC-MONICA). Basic information of how MONICA works is assumed in the HPC use case description.
