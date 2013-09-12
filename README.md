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
    npm install restify
    cd bin
    ./generate myserver data/schema.json data/server.template > server
    ./server
    myserver listening on 8001

Voila!

How to design a REST schema
===========================
look (https://github.com/coderofsalvation/restify-generator/blob/master/data/schema.json)[here]
