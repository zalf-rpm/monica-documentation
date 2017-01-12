# MONICA - The Model for Nitrogen and Carbon in Agro-ecosystems

MONICA is a dynamic, process-based simulation model which describes transport and bio-chemical turn-over of carbon, nitrogen and water in agro-ecosystems. On daily time steps the most important processes in soil and plant are modelled mechanistically. They are linked in such way that feed-back relations of the single processes are reproduced as close to nature as possible. MONICA works one dimensional and represents a space of 1 m² surface area and 2 m depth. 

The acronym MONICA is derived from „MOdel of Nitrogen and Carbon dynamics in Agro-ecosystems”.

This Wiki contains information about the software development of MONICA as well as its use. Additional information of the model (processes, installation of version 1.2) can be found on [http://monica.agrosystem-models.com/](http://monica.agrosystem-models.com/).

## History

Some information about the different versions of MONICA can be found here [MONICA versions](wiki/Monica-versions).

## Compile MONICA
An older german version of a compile how to can be found here [How to compile MONICA?](wiki/How-to-compile-MONICA). How to compile the current version can be found in the [README](https://github.com/zalf-lsa/monica/blob/master/README.md) in the repository.

## Use MONICA

### How to start MONICA

MONICA consists right now of a number of tools/parts. Most of them can be called at the commandline like

    monica command1 [command2] parameter1 parameter2 ....

eg. to run MONICA locally with the standard Hohenfinow2 example under Windows using the standard installer, 
go to the c:\users\USER_PROFILE\MONICA directory and execute in a Commandshell

    monica run Examples/Hohenfinow2/sim.json > out.csv

Alternatively one can type always the full name of the tool, which are named monica-run, monica-zmq-run, monica-zmq-server etc, 
monica is just a proxy program calling the actual tools. There are the following tools/versions of MONICA available.

* libmonica ... the libmonica.dll/.so with the MONICA core functionality
* monica ... the proxy tool, to call the other tools
* monica-python ... the monica_python.dll/.so to be used as Python library, acting as simple bridge to call MONICA from Python
* monica-run ... the standalone MONICA commandline model (using libmonica)
* monica-zmq-control ... a tool to run on the server and accepting ZeroMQ messages to spawn MONICA servers/ZMQ Proxies etc
* monica-zmq-control-send ... a client side tool to send ZMQ messages to the server 
* monica-zmq-proxy ... a tool to run serverside and forward/distribute jobs (ZMQ job messages) to MONICA workers (servers)
* monica-zmq-run ... the ZMQ client to monica-zmq-server/the equivalent to monica-run, but sends work to a monica-zmq-server
* monica-zmq-server ... a MONICA server accepting ZeroMQ messages and process them, usually to be used in connection with ZMQ proxy/proxies and a cloud of worker monica-zmq-servers

Every one of the tools can be called with a commandline parameter **-h** which will output the available parameters and how the command works. For example ```monica -h``` will result in:

```dos
C:\> monica -h

monica commands/options

commands/options:

 -h | --help ... this help output
 -v | --version ... outputs monica version

   run PARAMETERS ... start monica-run with PARAMETERS
 | zmq   run PARAMETERS ... run MONICA ZeroMQ client 'monica-zmq-run' with PARAMETERS
       | server PARAMETERS ... run MONICA ZeroMQ server 'monica-zmq-server' with PARAMETERS
       | proxy PARAMETERS ... run MONICA ZeroMQ proxy 'monica-zmq-proxy' with PARAMETERS
       | control PARAMETERS ... run MONICA ZeroMQ control node 'monica-zmq-control' with PARAMETERS
       | control send PARAMETERS ... send command to MONICA ZeroMQ control node via calling 'monica-zmq-control-send' with PARMETERS
```

Which should be read as the tool **monica** can be called with the options **-h** (for this help) or **-v** (for the version number) or a list of **commands**. 

The command **run** is for running MONICA locally (on the own computer) and `monica run` is equivalent to `monica-run`. Showing the help for **monica-run** would display:

```dos
C:\>monica run -h

monica-run [options] path-to-sim-json

options:

 -h   | --help ... this help output
 -v   | --version ... outputs monica-run version

 -d   | --debug ... show debug outputs
 -w   | --write-output-files ... write MONICA output files
 -op  | --path-to-output DIRECTORY (default: .) ... path to output directory
 -o   | --path-to-output-file FILE ... path to output file
 -c   | --path-to-crop FILE (default: ./crop.json) ... path to crop.json file
 -s   | --path-to-site FILE (default: ./site.json) ... path to site.json file
 -w   | --path-to-climate FILE (default: ./climate.csv) ... path to climate.csv
```

So for example outputing the results of the Hohenfinow2 example to the file **hohenfinow-out.csv** and show debug messages could be done with:

```
monica run -d Examples\Hohenfinow2\sim.json
```

Changes to parameters on the commandline will always supercede the same information in the configuration files.


### How to configure MONICA

MONICA is now available in a version 2. There are a lot of changes especially regarding the configuration of MONICA, how to start it and how to get the results. Most of the changes are described in the this [description of the MONICA json config files](wiki/description-json-config-files). 

