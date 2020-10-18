## Node.js Techniques -   Role Based Authorization at the API level

> APIs are everywhere and have many different faces and capabilities.

Essentially APIs act as transports vehicles 🚗 that pull information from one source and feed it to another. For example, [Google maps](https://maps.google.ng/) API used in navigation systems or [Stripe](https://stripe.com/) API used for Online payments, etc. 

APIs in a lot of applications serve as data points for client-based applications 🖥️ or other API clients, with read-write permissions; and as such certain endpoints need to be protected to provide access to privileged users of the application. 

In this tutorial, we'll go through a simple implementation of role-based authorization or access control in a Node.js API with the Express framework. In this example, I'll work with  JWT,  for token-based authentication in Node.js.

**Assumptions** : This tutorial assumes the audience has some knowledge of [Nodejs](https://nodejs.org/) and the [Express](https://expressjs.com/) framework

## Prerequisites
- Install [Nodejs](https://nodejs.org/) on your machine
- Intall [jsonwebtoken - npm](https://www.npmjs.com/package/jsonwebtoken) into your project directory
```
npm i jsonwebtoken
 ```
## Let's set up the project 

- ### Create  Node.js Helpers 
`Base_dir/helpers/authhelpers`

This directory contains helper functions written as independent and reusable bits of code invoked within a ```router``` or ```controller``` file when needed.
```
// function to decode token
exports.decoder = (token) => jwt.decode(token);

const mustBeLoggedIn = (req, res, next) => {
  let token = req.headers["x-access-token"] || req.headers.authorization || req.body.token;
  // Express headers are auto converted to lowercase
  if (token && token.startsWith("Bearer ")) {
    // Remove Bearer from string
    token = token.slice(7, token.length).trimLeft();
  }
  try {
//  we extract the JWTSECRET from the .env file
    req.apiUser = jwt.verify(token, process.env.JWTSECRET);
    res.locals.apiUser = req.apiUser;
    // res.locals is guaranteed to hold state over the life of a request.
    next();
  } catch (error) {
    res.status(401).json({
      status: false,
      message: "Sorry, you must provide a valid token."
    });
  }
};


```
**N.B** 
The ```JWTSECRET ``` property in the ```.env``` is used by the API to sign and verify the JWT token. do update it with your unique random string to restrict unauthorized access to your application.


- ### Implement the authhelper function as a Nodejs Middleware in the API routes
`Base_dir/routes/<routesname>.js`

This middleware is written to intercept a request for a route. the middleware here,  checks if the bearer token is sent as header or body information if the token is still valid and if so, sends the token payload such as ```user_id and role``` to another method within the request life cycle.

The middleware as used in this snippet is called ``` mustBeLoggedIn ```
```
const express = require("express");
const { retrieveAllUsers } = require("../controllers/Controller");
const { mustBeLoggedIn } = require("../helpers/authhelper");

const router = express.Router();

router.get("/", mustBeLoggedIn, retrieveAllUsers);

module.exports = router;


```
The above routes are protected with a middleware that checks for a bearer token and extracts the payload, coupled with an error handler.
 
If there are no errors in the payload extracted from the token, we send this payload containing user data to the controller.

- ### Access the Nodejs route Controller
`Base_dir/controllers/<controllersname>.js`

This controller defines all routes for the API, the route definitions are grouped together at the top of the file and the route implementations are below.

Routes that use the ``` mustBeLoggedIn``` middleware are restricted to authenticated users, if the role is included (e.g. authorize(Role.Admin or Role.Superuser)) then the route is restricted to users in the specified role/roles.

 Otherwise, if the role is not included (e.g. ``` mustBeLoggedIn()```) then the route is restricted to all authenticated users regardless of their role. 
Thus Routes that don't use the authorize middleware are publicly accessible.

```
const jwt = require("jsonwebtoken");
const User = require("../models/users");
  
const retrieveAllUsers = async (req, res) => {
// the payload passed as request state from the middleware 
  if (res.locals.apiUser.role) {
    try {
      await User.find({ role: "Admin" }, {
        password: 0,
        dateCreated: 0,
        __v: 0
      })
        .then((response) => {
          res.status(200).json({
            status: true,
            message: "Succesfully Fetched all User data",
            data: response
          });
        })
        .catch((error) => {
          res.status(404).json({
            status: false,
            message: error.message,
            data: null,
            errors: error
          });
        });
    } catch (error) {
      res.status(500).json({
        status: false,
        message: "Server error, try again later."
      });
    }
  }
};

module.exports = retrieveAllUsers

```

This is a simple implementation of a role-based Authorization or access control on an API.

I want to learn about the awesome projects you build with larger-scale implementations of access control for application  APIs 😃.

Thanks for the audience and I hope you found this article helpful 🤗. feel free to reach out to [Github](https://github.com/nextwebb), [Twitter](https://twitter.com/i_am_nextwebb) and [LinkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144/).
Do drop a like, comment, and share 😌.

### Learn more 
- [Json Web Token](https://www.npmjs.com/package/jsonwebtoken)
- [ Role Based Authorization in Nodejs Tutorial with Example API ](https://jasonwatmore.com/post/2018/11/28/nodejs-role-based-authorization-tutorial-with-example-api)















