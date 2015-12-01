Changes in new Monica version 2.1:

Monica now has to be started from the users directory (usually c:\users\???USER_NAME???\MONICA). There it finds the databases and db-connection.ini.
Starting the Hohenfinow2 example from the commandline would now be:

cd c:\users\???USER_NAME???\MONICA
monica.exe path: Examples/Hohenfinow2

monica.exe is now in the windows path variable (%PATH%)

you can also say
monica.exe path: Examples/Hohenfinow2/json project: test

then Monica will search in the path: directory for 4 files names test.climate.csv, test.sim.json, test.crop.json and test.site.json. Else it will assume that in the path: directory there are 4 files without the "test." prepended as in the previous example. Both versions and the old hermes version are in the Hohenfinow2 directory.
To use the hermes files, you could (at least for now, but certainly not forever) say:

monica.exe mode: hermes path: Examples/Hohenfinow2

Also if you would like to enable debug mode use: debug?: true, thus a minimal debug enabled example would be:

monica.exe debug?: true path: Examples/Hohenfinow2

If you look at the shortcut to the monica executable in the Windows Start menu (the properties), you'll see that this runs a debug enabled, project enabled test example run in Examples/Hohenfinow2/json.

In order to learn a bit more about the new files, you could compare sim, crop and site json to the hermes files. Their content is identical. Sim.json sets a few simulation specific parameters, e.g. also start and end date. Site.json sets site parameters and also contains the exact soil profile data, while crop.json holds the crop rotation and crop parameters. The important thing to note is, that these files tell everything what happens with Monica. In crop.json for instance (in the example) the first section is just used as a section to create the crops being referenced in the crop-rotation below. There are kind of function calls in these sections like ["include-from-db", "crop", "rape", "winter rape"]. This means that at this point the [] will be replaced with the parameters section (json format) from the database, table "crop", species "rape" and cultivar "winter rape". This also tells that the database has changed so that overall in Monica now crops are usually referenced by species and cultivar. The old crop "id" is basically gone. It's still used internally, but from outside monica (as in the json files) you have to specifiy that species id (name) and cultivar id (name). Both can be found in the Monica database in the tables species and cultivar. Actually cultivar is enough as the cultivar specific parameters include of course the name to the species. There is also a view (virtual generated table) called crop, which looks like the old crop table.