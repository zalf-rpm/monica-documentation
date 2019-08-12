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

**ZeroMQ** makes it easy to connect processes/programs/scripts using a message passing system. Usually if two processes have to communicate there will be a **server-like** part and a **client-like** part. The server part being usually the more stable one (like a normal webserver), whereas clients come and go, connect and disconnect. In order to communicate both parties have to agree on a supported transport protocol (in our cases here **tcp**), an **IP address** or **DNS name** and a **port**. Thus when a client **connects** to an url like **tcp://my-domain.de:6666** it will tell the **ZeroMQ** library used by MONICA to use the **TCP** protocol to transport the messages and connect to a server listening on port **6666** at domain **my-domain.de**. The server would do something called **bind** to network interfaces at a given port and with a transport protocol. So **tcp://*:6666** would be the counterpart to the client address, which tells **ZeroMQ** to use **TCP** as transport protocol, bind to all available network interfaces (*) and port **6666**.

Because we're using ZeroMQ to transport our messages you don't have to care if the client or the server is there first. If one of the connecting parties starts up it will try to connect to the other party. Also ZeroMQ allows n:m connections, if it makes sense. This means multiple clients can at the same time be connected to multiple servers. This is especially relevant if the processes are connected in **pipeline** mode, where one process will receive data (input) on one port and send results (output) on to other connected processes on other port(s). Have also a look at the following picture.

![MONICA producer-consumer workflow](monica-prod-cons-flow.png)

This picture shows the general workflow employed currently with MONICA for running on HPCs/cluster computers. There will be a producer process (usually a **Python script**) which creates work (simulations) which are being send to an intermediate process (called a ZeroMQ **proxy**), which distributes the received messages (simulation jobs) amongst connected (to the **proxy**) **monica-zmq-server** processses. Each **monica-zmq-server** process waits for simulation jobs (messages), runs the simulation and sends the simulation result on to another ZeroMQ **proxy** which collects all results and forwards them to a connected consuming/aggregating process (usually a **Python script**). Producer and consumer as well as the MONICA processes (**client-like**) connect to the ZeroMQ proxy processes, which play the stable **server-like** role in this setup.

This simple setup allows to start an arbitrary/necessary amount of MONICA processes to handle the workload created by the producer process. In general there could be multiple producers connected to the **input side proxy** as well as multiple consumers to the **output side proxy**. But while conceptually it's easy to add more producers, it might be more difficult to have more consumers (to handle aggregation) as usually aggregation can't be separated easily. Also in normal setups often the producer process can become a bottleneck as a single process has to create all the simulation setups for N MONICAs. 

For more information and details, please look up the ZeroMQ documentation.


### Running single server in **Request-Reply** mode

The simplest way to use MONICA in server mode is to execute 

```dos
C:\> monica-zmq-server -s
```

which will start *monica-zmq-server* and will listen on all network interface on port **6666** by default. This is similar to a normal webserver. The user could send a ZeroMQ message to the started server, wait for the simulation to execute and receive the simulation result on the same port.

Via the commandline parameters **-s** you can choose a different port for the server to listen on. So 

```dos
C:\> monica-zmq-server -s tcp://*:5555
```

will start MONICA as server in **Request-Reply** mode listening on port **5555**.


### Running a single server in **Pipeline** mode

A single server can also be started in **pipeline** mode, which means that the server will accept requests on one port but send the request to clients connected to another (the output) port. To do that you start MONICA with

```dos
C:\> monica-zmq-server -bi -i tcp://*:6666 -bo -o tcp://*:7777
```

This will start a MONICA server process listening on port **6666** for job messages and sending results on port **7777** to connected clients. This will be done by playing the "ZeroMQ server role" (**binding** to the port), so clients have to **connect** to the given port (**bi = bind input** port and **bo = bind output** port).

On the other hand, starting *monica-zmq-server* with

```dos
C:\> monica-zmq-server -ci -i tcp://*:5555 -co -o tcp://*:5556
```

would "listen" for jobs on port **5555** and send results on port **5556**. But this time the MONICA server would have to **connect** to the processes/scripts/clients who will send jobs or receive the simulation results. Under normal single server use cases the first version using **bind = -bi / -bo** should be used.


### Running and using multiple servers via **ZeroMQ** proxy processes in **pipeline** mode

To utilize many **monica-zmq-server** processes at the same time, it makes sense to distribute work amongst many MONICAs via another process called a ZeroMQ **proxy**. In theory one could start a **producer** process (e.g. a **Python script**) as a **server-like** process (e.g. binding to **tcp://*:6666**) and let many **monica-zmq-server**s connect (e.g. via connecting to **tcp://localhost:6666**) to the **producer** process. The same could happen for the output side, where a **consumer** process binds to **tcp://*:7777** and all MONICAS connect to **tcp://localhost:7777**. Then ZeroMQ would take care to distribute the work from the single producer to many MONICAs and send all results on to a single consumer.

Practically letting play the **producer**/**consumer** play the stable **server-like** is not the best solution, as these are actually the mostly changing part in such a setup and it will always take a few seconds until all MONICAs automatically have been connected to a newly started producer and consumer. For that reason it makes sense to introduce into the setup two stable processes (**proxies**) as can be seen on the picture in the ZeroMQ section, which are for instance running steadily on a high performance cluster, playing the **server-like** part and **bind** to the network interfaces and let an arbitrary amount of MONICAs connect to them. This network/cloud of MONICAs is stable and waiting for simulation jobs at the proxy gatekeepers input ports. Then both, **producer** and **consumer** can **connect** to the their respective proxy and start sending/receiving messages.



### Running multiple servers via **Docker** containers

An easy way to get a setup of two proxies and a configurable number of MONICA server processes is to use the provided **Docker** images from **Docker-Hub**. More information of how to run a MONICA Docker image can be found [here](wiki/Run-MONICA-in-Docker-and-Singularity.asciidoc).



