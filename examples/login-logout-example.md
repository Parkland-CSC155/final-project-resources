# User Authentication (Login/Logout) Example
This is not a dissertation on application security - that concept deserves significantly more research on your part. However, there are ways that we can easily add login/logout functionality to our application without jeopardizing the security of our work. 

One of the packages that help ensure we get user authentication right, is [Passport](http://passportjs.org/). It comes with several, pre-built authentication strategies that range from Facebook to Google to rolling your own. 

On the side of getting something working with Passport's "LocalStrategy" (used for authenticating users in a custom database), a great example can be found [here](https://github.com/passport/express-4.x-local-example/blob/master/server.js)
