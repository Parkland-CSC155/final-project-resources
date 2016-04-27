#Browser JavaScript vs NodeJS JavaScript

If you've done jQuery or taken CSC175 before, you might think to yourself - "Hey, I can use JavaScript to make my EJS/Jade pages even more interactive" - which is true. However, you need to have a firm understanding of the difference between JavaScript for the Browser and JavaScript with NodeJS. 

### Server-Side vs Client-Side
__Server-side Code__

In most web frameworks you have what we call "Server-side" code. Common languages/frameworks for this would be: 
- PHP
- ASP.NET/C#
- Ruby/Ruby-on-Rails
- Java/Swing

These languages and frameworks run __ON THE SERVER__ and handle the incoming and outgoing HTTP requests that you make to the webserver. They are where database queries are made, files are created/updated, and all the other sorts of things that happen on the "Back-end" of a Client-Server relationship. __This is where NodeJS lives - it is a platform to run JavaScript on a Server__. NodeJS provides access to low-level system functions (like reading/writing files, listening to network requests, etc...) that typically can only work when executing on a physical computer. 

__Client-side Code__
If you've done jQuery or taken our CSC 175 course, you've spent some time writing JavaScript for the _Browser_. Why do we say the _Browser_ like it is something special? Because its a __VERY__ different environment than a Server. The Browser only knows how to talk HTTP over a network connection, thus the only way for it do things is by requesting webpages, image files, css files, or JavaScript files over HTTP and then loading and running those. If a JavaScript file wants to do something fancy, it is usually limited to manipulatin the HTML on the webpage the Browser has loaded, or making some __VERY__ limited HTTP requests through a process called AJAX. When writing JavaScript for the Browser, here are some DOs and DONTs:

DOs
- work with the `document` object to manipulate the HTML rendered in the browser
- use libraries like jQuery to add functionality to your page
- load additional data and send data back and forth with the server using AJAX.

DONTs:
- make direct SQL queries to a database
- call NodeJS functions like `fs.readFile`
- directly call express routes/functions on the server (you can through AJAX, but more on that below)

#### A bad comparison, but it gets the point across
If you're looking for a visualization, you can think of the Browser as being a customer in a restaurant (a client). There might be several (even hundreds) of customers (clients) all working with a single waiter (a server). That waiter (server) takes orders into the kitchen, does some work, and then returns with some food (data) or an apology for not getting your food (an error). JavaScript on the client-side of things can only make the food prettier or give the waiter (server) questions/comments that have to go back to the kitchen for an answer. JavaScript on the server-side (and all server-side languages) would be the chef in the kitchen doing all the hardwork and then sending food (data) back out to the customers(clients). 

## So How Do Setup Express to add Client-Side JavaScript to my App?
Typically, we'll put all of our web app's static files (CSS, JavaScript, and Images) into a folder called `public` in our app's folder structure. ie: 
```
/
  /routes
  /views
  /node_modules
  /public
    /img
    /css
    /js <-- here is where you put Client-side JavaScript files
  app.js
```
If you set your app up like that, then you can use the [Express Static Module for serving those files up properly](http://expressjs.com/en/starter/static-files.html)

## How do I tell my EJS/Jade Views to reference my Client-Side JavaScript files?
You can go into your EJS views and add something like the following: 
```
<script type="text/javascript" src="js/name-of-my-javascript-file.js"></script>
```

## What's this AJAX Concept you mentioned?
If you haven't taken CSC 175, and you don't know what AJAX is, then this might a concept that you would want to leave to others. You __DO NOT HAVE TO USE AJAX TO COMPLETE THIS PROJECT__ - I just know that some will want to use it. [This does a fantastic job at explaining this concept and how to use it](https://www.lynda.com/Developer-tutorials/JavaScript-and-AJAX/114900-2.html)

## I Know What AJAX is, how do I use it with express?
It pretty much boils down to doing two things: 
1. Define a server-side express route where an AJAX request can be directed
2. Write some client-side JavaScript to make an HTTP request to the previous route and handle the response

######1 - The Server-Side
```
// This is an express route defined within a NodeJS express application
// you can use GET, POST, PUT, DELETE
app.get("/an-ajax-route", function(req, res, next){

  // some arbitrary data that you might have loaded from a database
  var car = { make: "Chevrolet", model: "Cruze" };
  
  // express gives us a `json` function for sending JSON-formatted data back in our response
  res.json(car);

});
```

######2 - The Client Side
```
// the $ sign is representative of jQuery. jQuery contains the easiest AJAX functionality to get going with.
// Here we simply send an HTTP GET request back to our express app over the network, and then 
// use the `car` JSON data that it returns in our app. 
$.get("/an-ajax-route", function(data){
  
  var car = data; // this should be the JSON that was sent back from the server
  console.log(car.make);
  console.log(car.model);
  
});
```

The important point for the above code is that doing AJAX between the client and the server can be simple, but you can only do it over HTTP requests using an AJAX approach. If you aren't comfortable doing this, then just doing simple FORM posts and storing the data between requests, works just fine!
