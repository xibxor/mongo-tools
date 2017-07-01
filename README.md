**mongoimport** forked changes
===================================

* can now import csvs that use semicolon (;) as a delimeter
  
* can even import csvs that use commas (,) or a mix of both commas AND semicolons as delimeters

* if types are specified, mongoimport can now import an int32 type represented as a positive overflowing integer. 
  

For example, consider this scenario:
   
A field f is specified as f.int32() and the csv contains f equals 4294967295 (0xFFFFFFFF).
     
Instead of erroring out saying 4294967295 is greater than INT_MAX, the value will be imported as -1. Essentially, we take whatever the bit value of the field is, and we cast the memory itself to an int32. This allows importing of a variety of representations that whatever dumped the csv could have had in mind. And because two's complement is imdepotent, no values can be misinterpreted even if the csv dump is signed. So in other words, -1 and 4294967295 will have the same imported value even when in the same file.
    
    
Hope these fixes/changes help someone else out..mongoimport's inflexibility was very frustrating! Feel free to create an issue if something isn't working as intended or if you need help implementing a new change. Cheers!




MongoDB Tools
===================================

 - **bsondump** - _display BSON files in a human-readable format_
 - **mongoimport** - _Convert data from JSON, TSV or CSV and insert them into a collection_
 - **mongoexport** - _Write an existing collection to CSV or JSON format_
 - **mongodump/mongorestore** - _Dump MongoDB backups to disk in .BSON format, or restore them to a live database_
 - **mongostat** - _Monitor live MongoDB servers, replica sets, or sharded clusters_
 - **mongofiles** - _Read, write, delete, or update files in [GridFS](http://docs.mongodb.org/manual/core/gridfs/)_
 - **mongooplog** - _Replay oplog entries between MongoDB servers_
 - **mongotop** - _Monitor read/write activity on a mongo server_

Report any bugs, improvements, or new feature requests at https://jira.mongodb.org/browse/TOOLS

Setup
---------------
Clone the repo and run `. ./set_gopath.sh` (`set_gopath.bat` on Windows) to setup your GOPATH:

```
git clone https://github.com/mongodb/mongo-tools
cd mongo-tools
. ./set_gopath.sh
```

Building Tools
---------------
To build the tools, you need to have Go version 1.3 and up.

An additional flag, `-tags`, can be passed to the `go build` command in order to build the tools with support for SSL and/or SASL. For example:

```
mkdir bin
go build -o bin/mongoimport mongoimport/main/mongoimport.go # build mongoimport
go build -o bin/mongoimport -tags ssl mongoimport/main/mongoimport.go # build mongoimport with SSL support enabled
go build -o bin/mongoimport -tags "ssl sasl" mongoimport/main/mongoimport.go # build mongoimport with SSL and SASL support enabled
```

Contributing
---------------
See our [Contributor's Guide](CONTRIBUTING.md).

Documentation
---------------
See the MongoDB packages [documentation](http://docs.mongodb.org/master/reference/program/).

