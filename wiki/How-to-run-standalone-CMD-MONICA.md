# How to start MONICA

MONICA consists right now of a number of different programs. Most of them can be called at the commandline like

    monica-xxx parameter1 parameter2 ....

eg. to run MONICA locally with the standard Hohenfinow2 example under Windows using the standard installer, 
go to the c:\users\USER_PROFILE\MONICA directory and execute in a Commandshell

    monica-run Examples/Hohenfinow2/sim.json > out.csv

There are the following programs of MONICA available.

* **monica-run** ... the standalone MONICA commandline model (using libmonica)
* **monica-zmq-proxy** ... a tool to run serverside and forward/distribute jobs (ZMQ job messages) to MONICA workers (servers). Optionally it can also just start a number of MONICAs in an own thread and serve them
* **monica-zmq-server** ... a MONICA server accepting ZeroMQ messages and process them, usually to be used in connection with ZMQ proxy/proxies and a cloud of worker monica-zmq-servers
* **monica-capnp-proxy** ... a MONICA server which actually acts as a proxy to a single server but can start many MONICAs in threads and distribute work send to the proxy amongst the MONICAs
* **monica-capnp-server** ... a MONICA server where communication with MONICA happens through a Cap'n Proto interface

Every one of the tools can be called with a commandline parameter **-h** which will output the available parameters and how the command works. Showing the help for **monica-run** would display:

```dos
C:\>monica-run -h

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
monica-run -d Examples\Hohenfinow2\sim.json
```

Changes to parameters on the commandline will always supercede the same information in the configuration files.


# How to configure MONICA

MONICA is now available in a version 2. There are a lot of changes especially regarding the configuration of MONICA, how to start it and how to get the results. Most of the changes are described in the this [description of the MONICA json config files](monica_config_index). 


# How to convert the old HERMES style configuration to the new JSON format

After installation of MONICA version **>= 2.0.1** there will be a folder called **monica-ini-to-json**. In that folder you'll find a Python2 script which will read a **monica.ini** file and the associated crop, fertilizer, irrigation and weather files and create the according **sim.json, site.json, crop.json, climate.csv** files.

You need to have Python 2 (2.7) installed and can invoke the conversion script like:

```dos
C:\Users\USER-NAME\MONICA\monica-ini-to-json>python python monica-ini-to-json.py monica.ini=PATH-TO-monica.ini 
```

By default it will try to use a **template sim.json** file called **conversion-template-sim.json** from the **monica-ini-to-json** folder. Adding the parameter **-h** to the script will output the usage options:

```dos
C:\Users\USER-NAME\MONICA\monica-ini-to-json>python python monica-ini-to-json.py -h

usage: [python] monica-ini-to-json.py [monica.ini=path-to-monica.ini] [sim.json=path-to-template-sim.json] [out-suffix=suffix-to-append-to-sim-site-crop-climate.files]
defaults: {'out-suffix': '-from-monica-ini', 'monica.ini': './monica.ini', 'sim.json': './conversion-template-sim.json'}
```

The script can be called from anywhere, but then the path to the **conversion-template-sim.json** has to be given. As usual the **sim.json** contains the paths to **crop.json** and **site.json**. To prevent overwriting other **sim, site, crop.json** files already in the folder, the output **sim, crop, site.json** will have a suffix added. This can be ignored by setting the option **out-suffix=""**.

The conversion script will assemble all the climate data into a single **csv** file. If all the **global radiation** values are zero (rounded), then the **sunhours** in the HERMES style weather files will be included, else just **globrad** will be created. Also only the weather data from **startdate** to **enddate** like defined in **monica.ini** will be included into **climate.csv**. Additionally the script can be adjusted by two global parameters:

```json
YEAR_BORDERS = [[0, 30, 2000], [31, 99, 1900]]
YEAR_SUFFIX_DIGITS = 3
``` 

YEAR_SUFFIX_DIGITS tells that the ending of the weather files will consist of the last 3 digits of a year. YEAR_BORDERS defines when dates read from the configuration files (just 6 digits, thus only 2 year digits), will switch the century. The YEAR_BORDERS parameter should be read like from year ending **00** to (incl) year **30** belong to the years after **2000**, while from year **31** to year **99** the years belong to the last century. Thus a date **020506** will become an iso-date **2006-05-02** while a date **020598** will become an iso-date **1998-05-02**. 


## things to take care of

The script can't do any magic. Right now the script will create **OrganicFertilization** worksteps if the **incorporate** field in the HERMES style fertilizer file is set to **1**, else a **MineralFertilization** workstep. In cases of references to **crop** and **fertilizer** names the **["ref", "xxx", "name"]** mechanism will be used. So the user of the script will have to take care, that either the templates contain the correct referenceable crops and fertilizers or she/he has to update the created files afterwards. If all the references to **crop** or **fertilizers** are correct, the created new **JSON** files should be immediately usable by MONICA.


