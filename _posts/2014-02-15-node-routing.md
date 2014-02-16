---
layout: post
title: Simple Node.js URL Router
description: A simple Node.js URL router I made as an exercise.
---
A Simple Node.js URL Router
===========================
***
There is something about winter that always makes it difficult for me to sleep
through the night. I don't know if it's a result of me being inside more often,
or just some weird seasonal problem that I have. At any rate, I've been using my
new found waking hours to play around with Node.js. I was going through the
[nodeschool](http://nodeschool.io/) tutorials when I thought it might be a cool
exercise to write a simple URL router. Below is the code for the router.

{% highlight javascript %}

var http = require('http');
var url = require('url');

var routes = {};

module.exports = {
  'add_route' : add_route,
  'start_server': start_server,
}

function add_route (route) {
  for (var prop in route) {
    routes[prop] = route[prop];
  }
}

function start_server (port) {
  var server = http.createServer(function(request, response) {
    var url_object = url.parse(request.url, true);
    for (var key in routes) {
      var re = new RegExp(key);
      if(re.test(url_object.pathname)) {
        routes[key](url_object, response);
        break;
      }
    }
    response.end();
  });

  server.listen(port);
}

{% endhighlight %}

This router allows you to specify routes using regular expressions kind of like
the django URL router does. Below is some example code that uses it.

{% highlight javascript %}

var routing = require('./routing.js');

function hello_world (url_object, response) {
  response.write('hello world!');
}

routing.add_route({ '\/hello': hello_world });
routing.start_server(9210);

{% endhighlight %}

I have been trying to figure out if there's an easy way to avoid the O(n) scan
through the routes. I'll think about it more and probably revisit this code.
