# Building backend of web apps using "HTTP" core module can become easily complex because we have to hardcode a lot of if statements to respond to incoming requests. This cannot be maintainable.

```javascript
const http = require("http");

const server = http.createServer((req, res) => {
  if ((req.url = "/")) {
    res.write("");
    res.end();
  }
  if ((req.url = "/api/courses")) {
    res.write(JSON.stringify([1, 2, 3]));
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

Let's explore this convention using a real world example. Let's say we have a company called Vidly for renting out movies. We have a client app where we manage the lists of our customers. On the server, we should expose a service and an endpoint like this - _vidly.com/api/customers_ .

So the client can send HTTP requests to this endpoint to talk to our service. Now a few things about this endpoint you need to know.

First of all, the address can start with HTTP/HTTPs. That depends on the application and it's requirements.  
If you want the data to be exchanged on a secure channel, you would use HTTPs.

After that we have the domain of the application next we have/api. This is not compulsory, but you see a lot of companies follow this convention to expose their RESTful services. They include the word API somewhere in the address. It can be after the domain or it can be a subdomain like api.vidly.com.
There is no hard and fast rule.

After that we have "/customers" which refers to the collection of customers in our application. In the REST world we refer to this part as a "resource". We can expose our resources such as customers movies, rentals, and various endpoints.

So this is our endpoint to work with the customers. All the operations around customers, such as customers, updating a customer, would be done by sending an HTTP request to this endpoint.

The type of the HTTP request determines the kind of operation. So every HTTP request has what we call a verb or a method that determines its type of intention here are the standard HTTP methods. We have Get or getting data, post, or creating data. Put for updating data, and delete, for deleting data.

```javascript
const express = require("express");
const app = express();

app.get(); // http verbs
app.post(); // http verbs
app.put(); // http verbs
app.delete(); // http verbs
```

```javascript
    const express = require('express');
    const app = express();

    // Applying Middleware
    app.use(express.json()); // Enable parsing JSON object in body . express.json() returns a middleware which we use in the reprocessing pipeline.

    app.get('/' or '/api/array', (request, response) => { // callback called route handler, triggers when a http request hits this route/path
        response.send('Hello World' or [1,2,3]);
    });



    app.get('/api/courses/:year/:month?sortBy=name', (request, response) => { // callback called route handler, triggers when a http request hits this route/path
        response.send(request.params.year, request.params.month, request.params.query); // query params are always optional, returns a key:value pair


        response.status(404).send("Content Not Found!");
    });


     app.post('/api/courses/', (request, response) => {

        const course = {
            id: Math.random(),
            name: request.body.name // In order for this line to work, we need to enable parsing up json objects in the body of the request. Because by default this feature is not enabled in express
        }

        response.status(200).send(course); // common convention to send back the data saved in db with newly created ID.
    });


    app.listen(5000, () => console.log('Listening on port 5000'));

```

# Environment Variable

Now one thing we need to improve in this code is this hard coded value for the ports. So we have used 3000 as an arbitrary number, why this may work on your development machine, it's unlikely it's going to work in the production environment. Because when you deploy this this application to a hosting environment the port is dynamically assigned by the hosting environment.

So, typically in hosting environments for node applications, we have this environment variable called PORTS. An environment variable is basically a variable that is part of the environment in which a process runs. Its value is set outside this application.

```javascript
const port = process.env.PORT || 5000;
app.listen(port, () => console.log("Listening on port 5000"));
```

```javascript

    mac $ export PORT=5500
    win $ set PORT=5500

```


# Input Validation

From security standpoint, never trust what the client sends you. Always validate the input.
Now with joi first we need to define a schema. A schema defines the shape of our objects. What properties do we have in an object? What is the type of each property?       

```javascript

    const Joi = require('joi');

    const validate = (arg) => {
        const schema = {
            name: Joi.string().min(3).required()
        }

        return Joi.validate(arg, schema)
    }

    app.post('/api/courses/', (request, response) => {

       const { error } = validate(request.body);

        if (error) return response.status(400).send(error.details[0].message);
            
        const course = {
            id: Math.random(),
            name: request.body.name 
        }

        response.status(200).send(course); 
    });


```


# Middleware

The Middleware function is basically a function that takes a request object, and either returns a response to the client, or passes control to another Middleware function.     

So in express, every "route handler" function we have is technically a Middleware function, because it takes a request object, and in this case it returns a response to the client, so it terminates the request response cycle. So this is one example of a Middleware function.      

Another exmaple - so, when we call express.json() method inside app.use(), this method returns a function, a Middleware function, the job of this Middleware function is to read the request, parse the body of the request and if there is a JSON object it will populate the request.body property. 


This is what happens at runtime, so when you see a request on the server, that request goes through this pipeline, we call this pipeline "the request processing pipeline".     
In this pipeline we have one or more Middleware functions. Each Middleware function either terminates the request response cycle by either returning the object, or it will pass control to another Middleware function.        

So, in our current implementation, our request processing Pipeline has two Middleware functions. The first one is the Middleware function that parses the request body into a JSON object.
Now, in this case it doesn't terminate the request response cycle, so then it passes control to the second Middleware function which is in this case our "route handler".       
In our "route handler" we have the request object, with the body property populated, so here we can perform some operation and then terminate the request response cycle by returning a response to the client.     

So, express includes a few built in Middleware functions, but we can also create custom Middleware functions that we can put at the front of our request processing pipeline        

So every request that we get on the server, will go through our Middleware function, but this custom Middleware function, we can perform cross cutting concerns.        

For example, we can do logging, authentication, authorization, and so on. So, an express application is essentially nothing but a bunch of Middleware functions.

```javascript

    // Use this method to install a piece of middleware in request processing pipeline.
    app.use(function(request, response, next) {
        console.log(logging...);
        next(); // passes control to next middleware function. If you don't do this, our request will end up hanging  because we are not terminating the request response cycle .
    });

    app.use(urlencoded()); //  This Middleware function parses incoming requests with url encoded payloads. That is a request with a body like this - key=value&key=value

    //  Basically, if you have an html form with input fields and post that form to the server, the body of your request will look like this. Not commonly used now.
    //  So that's where you have url encoded payload in the body of your request.

```

Middleware functions are always called in sequence.         
Clean coding practice - put each Middleware function in a separate module.      
