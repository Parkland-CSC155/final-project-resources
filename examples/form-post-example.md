# Posting Data Using Express and HTML Forms
In order to handle user input, you'll need to `POST` data to your web application using an HTML form here and there. In order for your app to handle this, there are a few things you need to do:

## Setup
1. In your `app.js` (or your main code file), pull in the `body-parser` npm package and set it up with express: 
```
var express = require('express');
// ... other setup
var bodyParser = require('body-parser');

var app = express();

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));

// ... add your routes 
```

2. Define a route that will give the User an HTML page with a form
```
app.get("/a-url-for-my-form-page", function(req, res, next){
  res.render("my-view-with-a-form", { title: "Express Form" })
});
```

3. Create a `View` that has a form in it
```
<form action="/the-url-to-post-the-form-to" method="post">
  <label>Some Text Box</label>
  <input type="text" name="exampleInput" />
  <button type="submit">Submit</button>
</form>
```

4. Define a route that will handle the `POST`ed form 
```
app.post("/the-url-to-post-the-form-to", function(req, res, next){
  
  var exampleInput = req.body.exampleInput;
  console.log(exampleInput); // should have whatever was typed in the text box on the form
  
  // send the user back to a confirmation page
  res.redirect("/");
});
```
