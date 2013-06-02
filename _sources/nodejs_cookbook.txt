Introduction to NodeJS
==========================

Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

Installation of Node.js
--------------------------

Naturally, we need to install Node before we can write and execute an app. Installation is straight forward, if you use Windows or OS X; the nodejs.org website offers installers for those operating systems. For Linux, use any package manager. Open up your terminal and type ::

  sudo apt-get update
  sudo apt-get install node

Installing New Modules
-----------------------

Node.js has a package manager, called Node Package Manager (NPM). It is automatically installed with Node.js, and you use NPM to install new modules. To install a module, open your terminal/command line, navigate to the desired folder, and execute the following command ::
  
  npm install module_name

It doesn’t matter what OS you have; the above command will install the module you specify in place of module_name.

Getting started with Node
----------------------------


Our first Node.js script will print the text 'Hello World!' to the console. Create a file, called hello.js, and type the following code ::

   console.log('Hello World!');

Now let’s execute the script. Open the terminal/command line, navigate to the folder that contains hello.js, and execute the following command ::

node hello.js

You should see 'Hello World!' displayed in the console.

Creating a HTTP Server 
------------------------

An example of a web server written with Node which responds with 'Hello HTTP' ::


  <%
    // Include http module.
    var http = require("http");

    // Create the server. Function passed as parameter is called on every request made.
    // request variable holds all request parameters
    // response variable allows you to do anything with response sent to the client.
    http.createServer(function (request, response) {
      // Attach listener on end event.
      // This event is called when client sent all data and is waiting for response.
       request.on("end", function () {
        // Write headers to the response.
        // 200 is HTTP status code (this one means success)
        // Second parameter holds header fields in object
        // We are sending plain text, so Content-Type should be text/plain
        response.writeHead(200, {
           'Content-Type': 'text/plain'
        });
        // Send data and end response.
       response.end('Hello HTTP!');
    });
  // Listen on the 8080 port.
  }).listen(8080);
  %>

You can send more data to the client by using the response.write() method, but you have to call it before calling response.end(). Save this code as http.js and type this into your console ::

>node http.js

Open up your browser and navigate to http://localhost:8080. You should see the text “Hello HTTP!” in the page.

All of the examples in the documentation can be run similarly.

A few APIs provided by Node.js :
===========================

1. `Child Process <http://nodejs.org/api/child_process.html>`_ 

The set of APIs under this class help in spawning and managing a child process in an application.

2. `Cluster <http://nodejs.org/api/cluster.html>`_ 

A single instance of Node runs in a single thread. To take advantage of multi-core systems the user will sometimes want to launch a cluster of Node processes to handle the load.

The cluster module allows you to easily create a network of processes that all share server ports.

3. `HTTP <http://nodejs.org/api/http.html>`_ 

To use the HTTP server and client one must require('http').

The HTTP interfaces in Node are designed to support many features of the protocol which have been traditionally difficult to use. In particular, large, possibly chunk-encoded, messages. 

4. `Net <http://nodejs.org/api/net.html>`_ 

The net module provides you with an asynchronous network wrapper. It contains methods for creating both servers and clients (called streams). You can include this module with :: require('net');

5. `Process <http://nodejs.org/api/process.html>`_ 

The process module is used to invoke methods on the EventEmitter instance. 

6. `Stream <http://nodejs.org/api/stream.html>`_ 

A stream is an abstract interface implemented by various objects in Node. For example a request to an HTTP server is a stream, as is stdout. Streams are readable, writable, or both. All streams are instances of EventEmitter

You can load the Stream base classes by doing require('stream'). There are base classes provided for Readable streams, Writable streams, Duplex streams, and Transform streams.

7. `URL <http://nodejs.org/api/url.html>`_

This module has utilities for URL resolution and parsing. Call require('url') to use it.


8. `Util <http://nodejs.org/api/util.html>`_

These functions are in the module 'util'. Use require('util') to access them.


Creation of a Scalable Realtime SMS voting application using NodeJS 
===================================================================

In this post , we take you through the creation of a scalable realtime SMS voting application using NodeJS along with other technologies .
Prerequisites:

1. Install NOdeJS and NPM as mentioned above
2. Install Express which aims to provide small, robust tooling for HTTP servers. This can be done using the following command ::

  sudo npm install express -g express -H app-dir cd app-dir/ npm install

3. Sign-up for a Nodejitsu account where we can deploy our Node app

.. image:: _static/nodejitsu.png

4. Install Jitsu, the command-line tool for Nodejitsu

  `https://github.com/nodejitsu/jitsu`
5. Deploy the app ::

  jitsu deploy

You will be asked to enter a subdomain for your application and the version of Node.js to use. The defaults should work fine, so feel free to hit enter. Once your deployment has finished, check to see that it’s running:

  `http://your-subdomain.jit.su`

6. Set-up CouchDB 
Setting-up a local CouchDB is easy, but for the purposes of this tutorial we show you how to use Cloudant’s hosted CouchDB

.. image:: _static/cloudantsetup.png

7. To set-up a phone number to process incoming votes over SMS ,sign-up for a Twilio account `here https://www.twilio.com/try-twilio`_
Go to your dashboard and configure your number.If you click on “Dashboard”, you’ll see your Account SID and Auth Token. The Auth Token is initially hidden, but just click on the key to reveal it.

.. image:: _static/twilio.png

8. In order to use Highcharts,download and include the JS library:
<script src="http://www.twilio.com/blog/javascripts/highcharts.js"></script>

All the code can be obtained from the gthub in the following link https://github.com/ShivapriyaOH/node263.git

The following steps will walk you through the creation of the application:

1. Create a database called “events”

.. image:: _static/cloudantdbcreate.png

2. Create a new document

.. image:: _static/cloudantdbdoc.png

3. Populate the document with some test data

 Load a dummy event document into our database so we can get on with building our app.

.. image:: _static/sample_data.png

4. Edit the event on Cloudant to reflect your Twilio phone number

.. image:: _static/phonesetup.png

5. Create a view to query events.

A view is a map function that processes all of the documents in your database and created a hash of key/value pairs. These functions are stored inside of special documents called Design Documents.
Create a design document and paste in the content below.

.. image:: _static/designdoc.png

6.  Look into config.js from the github.We set the app to point at your CouchDB instance on Cloundant, you will need to provide the following four values ::
    config.couchdb.url: ‘https://your-username.cloudant.com’
    config.couchdb.port: 443
    config.couchdb.username: ‘your username’
    config.couchdb.password: ‘your password’
  
7. In package.json we add all the dependencies, which looks like this ::

   { 
     "name": "node263-votr-part1",
     "version": "0.0.1-51",
     "private": true,
     "scripts": {
       "start": "app.js"
     },
     "dependencies": {
       "express": "3.2.4",
       "hjs": "*",
       "cradle": "0.6.3",
       "twiliosig": "0.0.1",
       "socket.io": "0.9.11",
       "underscore": "1.4.x",
       "nano": "3.3.x",
       "twilio": "1.1.x"
     },
     "subdomain": "node263-votr-part1",
     "engines": {
       "node": "0.8.x"
     }
   }


8. Create events.js where we define the three methods:
findBy – looks up an event based on a key
hasVoted – checks to see if a person has already voted for a specific event
saveVote – saves a person’s vote

https://github.com/ShivapriyaOH/node263/blob/master/events.js


9. In routes/index.js has a function called 'voteSMS' to handle the incoming requests from Twilio is defined. Each time Twilio receives an SMS, this function will be called.

https://github.com/ShivapriyaOH/node263/blob/master/routes/index.js

Add a route to our Express application to accept POST requests for the path “/vote/sms” and wire it to the voteSMS function we just created.

10. Node.js app and edit config.js

If you click on “Dashboard”, you’ll see your Account SID and Auth Token. The Auth Token is initially hidden, but just click on the key to reveal it.


11. Using Highcharts to display graph :

We have a variable 'voting' that contains the current state of this event. All we have to do is iterate through this data and build a couple of arrays for Highcharts to use to define the series ::

  var chartdata = [], 
  labels = [];
  voting.forEach(function(vo, i) {
    // the number of votes
    chartdata.push(vo.votes);
    // the label for this data point
    labels.push(vo.name+' - '+(i+1));
  });

Once we have the chartdata and labels arrays initialized, we simply pass these arrays to Highcharts during the initialization ::

  xAxis: {
    categories: labels
  }
  series: [{name: 'Votes', data: chartdata}]

12. To do add some real-time display and animate the graph as live votes come in over SMS we use socket.io. Adding socket.io to our Node.js app is done in the following manner :
 a.Edit package.json and add “socket.io”: “0.9.11″ to our dependencies
 b.npm install

The following code snippet shows the use of socketio ::

  socketio = require('socket.io')
  // Attach socket.io to our web server
  io = socketio.listen(server);
  io.configure('development', function(){
  io.set('log level', 1);
  });
  io.sockets.on('connection', function(socket) {
    socket.on('event', function(event) {
     socket.join(event);
    });
  });

The code can be obtained from https://github.com/ShivapriyaOH/node263/blob/master/app.js 

13. Include the JS library on the client :
<script src="http://www.twilio.com/blog/socket.io/socket.io.js"></script>

We need to open up a socket and once the connection is established, send the event message to the server ::

  var socket = io.connect();
  socket.on('connect', function() {
    console.log("Connected, lets sign-up for updates about votes for this event");
    socket.emit('event', '{{ shortname }}');
  });

14. We need to use socket.io to tell the browser when new votes come in ::

  socket.on('vote',function(data) {
    vote = parseInt(data);
    index = vote - 1;
    votes = chart.series[0].data[index].y;
    chart.series[0].data[index].update(votes+1);
  });

This code accepts the incoming vote, parses the string to create an int, calculates an array index and then does a simple increment of the vote count.  


15. In order to obtain a scalable application , we are going to use separate documents for the children in CouchDB as "events" and "votes".

16. The map functions have to deal with both event and vote documents to create key/value pairs. Here’s what the map function looks like in the event/all view ::

  function(doc) {
    if(doc.type=='event') {
      emit([doc._id],doc);
    }
    if(doc.type=='vote') {
      emit([doc.event_id,doc.vote, doc._id],doc);
    }
  }

A reduce function processes all of the results from map and returns the final output.
We can use reduce to collapse the table above and get the total number of votes for each option by using the built-in _count function ::

  "all": {
   "map": "...",
   "reduce": "_count"
  }

17. Getting the Vote Count per Voteoption
When someone hits the event page (http://domain/events/Java) we need to fetch information about the event and the number of votes for each voting option. Using map/reduce as described above, this is a simple and fast query to CouchDB ::

  voteCounts = exports.voteCounts = function(event, callback) {
    db.view('event', 'all', {startkey: [event._id], endkey: [event._id, {}, {}], group_level: 2}, function(err, body) {
      if (err) {
        callback(err);
      }
      else {
        // populate count for voteoptions
        event.voteoptions.forEach(function(vo, i){ 
          var found = _und.find(body.rows, function(x) {return x.key[1] == vo.id});
          vo['votes'] = (found? found.value : 0);
        });
        callback();
      }
    });
  }



