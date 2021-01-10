---
layout: post
# categories: Flask, web applications
data: 2021-04-01
---


This is an article I am writing after deploying my first Flask app on a virtual server.
Tools needed for the deployment on the server : Flask application, postgres(if want to 
deploy both the web and database server on one machine), Gunicorn, supervisor and NGINX.
Now I will talk about why do we need these tools and how to use them.

### <b>Why do we need these tools ?</b>

<i>I will skip why we need Flask and postgres. </i>

<i>A word on terminology: I will use word app which means Flask application (website made with flask). Sometimes I might use the word machine which can be interchanged with virtual server</i>

First let's see how we run a flask app on local machine. To run a flask app we set some configuration settings for example where to start, environment, secret key, server name [etc.][flaskconf], after this we go to terminal and do `flask run` voila we are given a local address where we can see our app running.

 Now there are so many doubts that can arise at this point so I will use question-answer format to understand how flask runs the application.

<b>What does `flask run` do ?</b>

This command creates a web server which listens to requests on your localhost on some port. This created server is a development server and is not secure, stable or efficient. What this means is 

<b>What is a web server, what does it do and how?</b>

It can be confusing what is a web server, a server can refer to both software and hardware. On hardware we define server as a computer which stores some web server software and website this hardware is connected to internet and can share data with other devices.  

On software side we think it as a software which accepts HTTP request and responds with a HTTP request. We can also serve files off of disk through the web server. 

<b>When we click something on the website how is the request created and how do we know where to send this request ? </b>

The HTTP request consists of HTTP method like `GET`, `POST`, the path of resource to fetch, optional headers that convey additional information for servers, a body for some methods like `POST`. Now how we got to this HTTP request depends, maybe you clicked some link. After this request is cliked to send then we first need to figure out where to send this request, the clients computer first checks their `hosts` file to see if the address is in there for e.g `localhost`. Then this is passed to DNS server which itself has multiple layers but if the website exits it will return the IP address of that website. Once we figure out the address then we establish a connection to that machine via TCP. 

<b> How does web server know who requested what ?

Once a TCP connection is made to some server then the client(sender) will send the HTTP request to that server. The server can create a new thread for this new command, which will then return whatever the client requested.

What if the user is logged in or some data needs to be stored for further communication ? We can use sessions (on the server side) and cookies( on the client side) to store data.

<b>How is the webpage generated how does flask know which file to show as homepage ?</b>
First we need to know that we can include the frontend (HTML, CSS, JS) in the flask app itself or can use other frontend frameworks like angular, react etc.
Why would one like to create a frontend in the Flask app itself ? If one is serving mostly static and some on the fly generated content then using the frontend in Flask app makes sense.

So in case of frontend inside the Flask app we create a `/static` folder, in the `/static` folder we then have a `index.html`, other html pages, CSS, Js. Flask uses `route` decorator to bind a url to a URL, so when a user sends a request flask checks for which route this request was made and based on that a function handles what happens wjen user asks for that route. Here we can ask Flask to return a html template from the static folder.


<b>Why do we need Gunicorn</b> 

For development we use `flask run` which is not secure, stable and efficient. So we need something which solves this problem, enter WSGI[wsgi] servers these are production grade <span>application servers</span> for python apps. A few of those are gunicorn, uWSGI, Gevent etc. we can use one of these servers to run on our virtual server and return requests to users. We can use multiple workers which work in parallel to serve requests this feature is not available in the development server.  But there is one problem, they don't serve static files efficiently, we also can't compress our static files while returning requests. 


<b>Why do we need NGINX ?</b>

To serve static files efficiently and deciding where to send the incoming requests to that machine, maybe we have an email server also on the same machine. We can also compress the outgoing files like css, js using gzip, NGINX can also be used to implement caching of the dynamic content. If one has multiple virtual servers running a flask app then we can also use its load balancing feature as well. Other feature is reverse proxy (see [this][stackReverseProxy] and [this][apacheReverseProxy] to understand what is proxy and reverse proxy) this can be used if one does not want to expose the main server directly.

<b> So who actually handles the HTTP requests and how does NGINX and Gunicorn fit together ? </b>

Both NGINX and Gunicorn handle the request, NGINX receives a request then it decides if this is a request for some static content or dynamic one. If it is a static one then the request is handled by NGINX itself else the request is forwarded to Gunicorn, which will take the request give it some worker, process it and return back the response to NGINX which then forwards the response back to the original client.

<b> Why do we need supervisor ?</b>

Supervisor tool will monitor the Flask server process and automatically restarts if it ever crashes or if the server is rebooted.

### <b> How to use them ? </b>

Refer to codebase.
 
[flaskconf]: https://flask.palletsprojects.com/en/1.1.x/config/
[wsgi]: https://en.wikipedia.org/wiki/Web_Server_Gateway_Interface
[stackReverseProxy]: https://stackoverflow.com/questions/224664/whats-the-difference-between-a-proxy-server-and-a-reverse-proxy-server#:~:text=A%20pair%20of%20simple%20definitions,proxy%2Dserver%2F11903901%2311903901
[apacheReverseProxy]:http://httpd.apache.org/docs/2.4/mod/mod_proxy.html#forwardreverse 