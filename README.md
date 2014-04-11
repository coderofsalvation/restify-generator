restify-generator
=================

generates a restify RESTserver from a iodocs json schema

Why
===
To easily generate a startingpoint/skeleton RESTserver from a design which is robust: think first, then code.
Also, the same design can be used with iodocs in order to generate a REST documentation/dasboard.

Installation
============

    git clone https://github.com/coderofsalvation/restify-generator.git
    cd restify-generator
    npm install restify
    mkdir api
    cd bin
    ./generate serverapi ../data/schema.json ../data/server.template ../serverapi.js api
    node serverapi.js
    myserver listening on 8001

Voila!

How to design a REST schema
===========================
look [here](https://github.com/coderofsalvation/restify-generator/blob/master/data/schema.json)

Example script
==============
I used this bashscript to quickly generate/update my API with several versions:


    #!/bin/bash
    mypath="$(dirname $(readlink -f "$0"))"
    cd $mypath;

    serverfile="$mypath/server"

    # clone restify-generator if not exists and install restify if not installed
    [[ ! -d "$mypath/restify-generator" ]] && git clone https://github.com/coderofsalvation/restify-generator.git 
    [[ ! -d "node_modules"              ]] && npm install restify

    generateVersion(){
      version="$1"
      cd "$mypath/restify-generator/bin"
      [[ "$1" == "clean" ]] && rm -r "$mypath/RESTresources/v$version"
      [[ ! -d "$mypath/RESTresources/v$version" ]] && mkdir -p "$mypath/RESTresources/v$version"
      ./generate myproject "$mypath/data/v$version/schema.json" \
                               "$mypath/data/server.template" \
                               "$serverfile" \
                               "$(readlink -f "../../")"
      chmod 755 "$serverfile"
    }

    generateVersion 1

This produced:

    $ ls 
    RESTresources/v1/writeIRC.js
    server

This was the contents of the 'server' file:    

    #!/usr/bin/node
    /**
     * Restify RESTserver
     * 
     * usage: chmod 755 server; ./server
     * how to test: curl -is http://localhost:8001/hello/mark -H 'accept: text/plain'
     */
    
    var restify = require('restify');
    
    function notImplemented(req, res, next) {
      res.send('not implemented ' + req.params.name);
    }
    
    var server = restify.createServer({
      name: 'myproject',
    });
    
    server.listen(8001, function() {
      console.log('%s listening at %s', server.name, server.url);
    });
    
    server.post('/v1/writeIRC', function(req, res, next ){
      require('./RESTresources/v1/writeIRC.js').handle( req, res, next, myproject );
    });

and this the contents of the 'writeIRC' RESTresources I defined in data/schema.json.
(NOTE: it doesnt get overwritten if the file already exist) :

    /***
     * POST /v1/writeIRC
     *
     * posts a message on a irc (channel)
     */

    function handle(req, res, next, myproject ){
      var response  = myproject.response;
      var code = response.code = 0;
      response.message = myproject.codes[code]; response.data = req.query;  res.send( response );
    }

    module.exports.handle = handle;
