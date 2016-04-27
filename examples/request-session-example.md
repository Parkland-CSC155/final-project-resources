# Request Sessions
Many times while building a web application, you'll find yourself needing to store some arbitrary data related to a user's usage of your app, but the data is not critical enough to need special tables in your database. This scenario is perfect for using the `session` functionality of a web framework. 

## What is a Session?
A "Session" is often described as the interaction of a user with your application over a short period of time. Logging into Gmail, reading your email, and then logging out would be a good example of a session you had on Gmail. Often as an application programmer, you'll want to keep track of information specific to a user's session on your application. Things like: 
- are they authenticated
- what was their full name and email
- how many pages did they visit, or requests did they make
- did they buy anything?
- what products did they view and not buy?
- etc...

Almost all frameworks provide some mechanism for storing this arbitrary data during a user's session on your application. Most often we refer to this storage mechanism as a `session` as well. It can be confusing, but through experience you'll get the idea of whether we're talking specifically about the storage mechanism or just the user's time on your app. 

## How does Express support `session`?
Setup
```
var session = require('express-session');
// other modules

var app = express();
// other setup

app.use(session({ 
  resave: false, 
  saveUninitialized: true,
  secret: 'qeurhqjajdfhy23249hhfsaff==' 
}));
```
After we wire up the above package, we will get a `session` object attached to our `req` in our route handlers. The `express-session` module will do the hardwork of storing this `session` object between each request/response.

Usage
```
app.get("/session-example", function(req, res, next){
  
  // ensure that the data on the session
  // has been set for the first request
  if(!req.session.viewCount){
    req.session.viewCount = 0;
  }
  
  req.session.viewCount += 1;
  
  // store arbitrary data that you need between
  // requests, but is not important enough
  // to put into a database
  req.session.chosenIngredients = [
    { id: 1, qty: 3}
  ];
  
  res.send("All good");
});
```
