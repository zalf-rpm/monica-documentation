# Running multiple MONICAs on High Performance Computers (Clusters) or in parallel on potentially multiple machines

After installation of the Windows version of MONICA the command monica-zmq-server can be used to start MONICA in "server" mode. This means a MONICA process will be started which waits for ZeroMQ messages which contain a full description of a single simulation to execute. This MONICA server process can be configured via commandline parameters:

```dos
C:\> monica-zmq-server -h

monica-zmq-server[options]

options:

 -h | --help ... this help output
 -v | --version ... outputs monica-zmq-server version and ZeroMQ version being used

 -d | --debug ... show debug outputs
 -s | --serve-address [ADDRESS] (default: tcp://*:6666)] ... serve MONICA on given address
 -p | --proxy-address [(PROXY-)ADDRESS1[,ADDRESS2,...]] (default: tcp://localhost:6666)] ... receive work via proxy from given address(es)
 -bi | --bind-input ... bind the input port
 -ci | --connect-input (default) ... connect the input port
 -i | --input-address [bind|connect]|[ADDRESS1[,ADDRESS2,...]] (default: tcp://localhost:6666)] ... receive work from given address(es)
 -bo | --bind-output ... bind the output port
 -co | --connect-output (default) ... connect the output port
 -o | --output-address [ADDRESS1[,ADDRESS2,...]] (default: tcp://localhost:7777)] ... send results to this address(es)
 -or | --router-output-address [ADDRESS1[,ADDRESS2,...]] (default: tcp://localhost:7777)] ... send results to this address(es) but use a router socket
 -c | --control-address [ADDRESS] (default: tcp://localhost:8888)] ... connect MONICA server to this address for control messages
```

## Understanding MONICA server

### ZeroMQ

[**ZeroMQ**](http://zeromq.org) is a small messaging library which is available for many different platforms and programming languages. It allows to send easily messages which can contain arbitrary information between connected parties (nodes/programs/etc). 

In the case of MONICA that means that a **ZeroMQ message** is actually a **JSON** string, which contains all the information MONICA needs to run a full simulation. This includes all (climate-) data, soil- and crop-parameters as well as the configuration of the particular simulation. One possible exception are at the moment climate data which optionally can be read by the MONICA server process itself if the path to a climate **.csv** file is given. This has been done for performance reasons.

**monica-zmq-server** can thus be treated as a stateless process, which receives via *ZeroMQ* messages/jobs, runs a single simulation and returns a result message. This result message is again a **JSON** string which contains the results requested by the configuration in the **sim/output/events** section of the received message.


### Running single server in **Request-Reply** mode

The simplest way to use MONICA in server mode is to execute 

```dos
C:\> monica-zmq-server -s
```

which will start *monica-zmq-server* and will listen on all network interface on port **6666** by default. This is similar to a normal webserver. The user could send a ZeroMQ message to the started server, wait for the simulation to execute and receive the simulation result on the same port.


### Running a single server in **Pipeline** mode


![MONICA producer-consumer workflow](monica-prod-cons-flow.png)



### Running and using multiple servers via **ZeroMQ** proxy processed 



### Running multiple servers via **Docker** containers

An easy way to get a setup of two proxies and a configurable number of MONICA server processes is to use the provided **Docker** images from **Docker-Hub**. More information of how to run a MONICA Docker image can be found [here](wiki/Run-MONICA-in-Docker.asciidoc).



