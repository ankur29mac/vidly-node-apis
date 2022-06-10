# Building backend of web apps using  "HTTP" core module can become easily complex because we have to hardcode a lot of if statements to respond to incoming requests. This cannot be maintainable.

```
    const http = require('http');

    const server = http.createServer((req, res) => {
        if(req.url = "/") {
            res.write('');
            res.end();
        }
        if(req.url = "/api/courses") {
            res.write(JSON.stringify([1,2,3]));
            res.end();
        }
    });

    server.listen(3000);

```

# Express.js 
Fast, lightweight, properly documented backend webframework to create web server and APIs.  


# REStful services/APIs - 
All applications follow the client-server architecture, so most if not all applications we use these days follow this architecture.
The app itself is the client or the front end part. Under the hood, it needs to talk to the server or the backend to get or save the data. This communication happens using the HTTP protocol. The same protocol that powers the web. So, on the backend we expose a bunch of services that are accessible only via the HTTP protocol.

The client can then directly call these service by sending HTTP requests. Now this is where REST comes into the picture.
REST is short for "Representational State Transfer". It was introduced by a PhD student, as part of his thesis.
REST is basically a convention for building these HTTP services. So we use simple HTTP protocol principles to provide support to CRUD operations on data. 

We refer to these operations all together as CRUD operations.  

Let's explore this convention using a real world example. Let's say we have a company called Vidly for renting out movies. We have a client app where we manage the lists of our customers. On the server, we should expose a service and an endpoint like this - *vidly.com/api/customers* .  

So the client can send HTTP requests to this endpoint to talk to our service. Now a few things about this endpoint you need to know.  

First of all, the address can start with HTTP/HTTPs. That depends on the application and it's requirements.  
If you want the data to be exchanged on a secure channel, you would use HTTPs.  

After that we have the domain of the application next we have/api. This is not compulsory, but you see a lot of companies follow this convention to expose their RESTful services. They include the word API somewhere in the address. It can be after the domain or it can be a subdomain like api.vidly.com.
There is no hard and fast rule.  

After that we have "/customers" which refers to the collection of customers in our application. In the REST world we refer to this part as a "resource". We can expose our resources such as customers movies, rentals, and various endpoints.  

So this is our endpoint to work with the customers. All the operations around customers, such as customers, updating a customer, would be done by sending an HTTP request to this endpoint.    

The type of the HTTP request determines the kind of operation. So every HTTP request has what we call a verb or a method that determines its type of intention here are the standard HTTP methods. We have Get or getting data, post, or creating data. Put for updating data, and delete, for deleting data.

```
    const express = require('express');
    const app = express();

    app.get();    // http verbs
    app.post();   // http verbs
    app.put();    // http verbs
    app.delete(); // http verbs

```


```
    const express = require('express');
    const app = express();

    app.get('/' or '/api/array', (request, response) => { // callback called route handler, triggers when a http request hits this route/path
        response.send('Hello World' or [1,2,3]);
    });

    

    app.get('/api/courses/:id', (request, response) => { // callback called route handler, triggers when a http request hits this route/path
        response.send(request.params.id);
    });

    app.listen(5000, () => console.log('Listening on port 5000'));

```

# Environment Variable
 Now one thing we need to improve in this code is this hard coded value for the ports. So we have used 3000 as an arbitrary number, why this may work on your development machine, it's unlikely it's going to work in the production environment. Because when you deploy this this application to a hosting environment the port is dynamically assigned by the hosting environment.    

So, typically in hosting environments for node applications, we have this environment variable called PORTS. An environment variable is basically a variable that is part of the environment in which a process runs. Its value is set outside this application.  

```
    const port = process.env.PORT || 5000
    app.listen(port, () => console.log('Listening on port 5000'));

```


```
    mac$ export PORT=5500
    win$ set PORT=5500

```







