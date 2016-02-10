#Bluemix Integration Services Lab Cheat Sheet

This document will have all the links, code snippets and notes that you will need to complete our "Hands-On with Bluemix Integration Services Lab"

## Important Links

Access Bluemix - http://bluemix.net

Public Temperature Conversion WSDL - http://webservices.daehosting.com/services/TemperatureConversions.wso?WSDL

Download SOAP UI - https://www.soapui.org/

Bootstrap CSS CDN URL - https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css

Bootstrap JS CDN URL - https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css

## Commands

```shell

cf api <Bluemix URL> for setting your Bluemix EndPoint

cf login for logging in to your Bluemix Account

cf push to push your application to the Bluemix Cloud

```

## Code Snippets

The following code snippets will help you in creating the final Consumer Application. 

### package.json

```javascript
{
"name": "NodejsStarterApp",
"version": "0.0.1",
"description": "A sample nodejs app for Bluemix", "scripts": {
"start": "node app.js" },
"dependencies": { "cfenv": "^1.0.3",
"ejs": "^2.4.1", "express": "4.12.x",
"https": "^1.0.0", "path": "^0.12.7", "request-json": "^0.5.5"
},
"repository": {}, "engines": {
"node": "0.12.x" }
}
```

The above is to add dependencies for your application. 

### app.js 

```javascript
/*eslint-env node*/
// This application uses express as its web server // for more info, see: http://expressjs.com
var express = require('express');
var path=require('path');
// cfenv provides access to your Cloud Foundry environment // for more info, see: https://www.npmjs.com/package/cfenv var cfenv = require('cfenv');
// create a new express server var app = express();
// serve the files out of ./public as our main files app.use(express.static(__dirname + '/public')); var routes = require('./routes/index');
var users = require('./routes/users'); app.set('views', path.join(__dirname, 'views')); app.set('view engine', 'ejs');
app.use('/', routes);
app.use('/users', users);
// get the app environment from Cloud Foundry
var appEnv = cfenv.getAppEnv();
// start server on the specified port and binding host app.listen(appEnv.port, '0.0.0.0', function() {
// print a message when the server starts listening console.log("server starting on " + appEnv.url);
});
```

### Stylesheet/style.css

```css
body {
padding: 50px;
font: 14px "Lucida Grande", Helvetica, Arial, sans-serif;
}
a{
color: #00B7FF;
}

.modal-footer {
  border-top : 0px;
}
```
### routes/index.js
```javascript
var express = require('express');
var https=require('https');
var request = require('request-json');
var client = request.createClient('http://localhost:8888/'); var router = express.Router();

/* GET users listing. */
router.get('/', function(req, response, next) {
var data = { 'Fahrenheit':'25'
};
client.post('<your-ibm-api-management-URL> ', data, function(err, res, body) {
response.render('index',{title:body.Celsius});
});
});
module.exports = router;
```
### routes/users.js

```javascript
var express = require('express');
var https=require('https');
var request = require('request-json');
var client = request.createClient('http://localhost:8888/'); var router = express.Router();
/* GET users listing. */
router.get('/', function(req, response, next) {
var data = { 'Fahrenheit':'25'
};
client.post(‘<your-ibm-api-management-URL>’, data, function(err, res, body) {
response.render('index',{title:body.Celsius});
});
});
module.exports = router;
```

### views/index.ejs

```html
<html lang="en">
<head>
<meta http-equiv="content-type" content="text/html; charset=UTF-8">
<meta charset="utf-8">
<title>Fahrenheit to Celsius Converter</title>
<meta name="generator" content="Bootply" />
<meta name="viewport" content="width=device-width, initial-scale=1,
maximum-scale=1">
<link href="stylesheets/bootstrap.min.css" rel="stylesheet"> <!--[if lt IE 9]>
<script src="//html5shim.googlecode.com/svn/trunk/html5.js"></script> <![endif]-->
<link href="stylesheets/styles.css" rel="stylesheet"> </head>
<body> <!--login modal-->
<form action="/users" method=get>
<div id="loginModal" class="modal show" tabindex="-1" role="dialog" aria-hidden="true">
<div class="modal-dialog"> <div class="modal-content">
<div class="modal-header" style="background:black">
<h2 class="text-center" style="color:white"><b>Miracle Software Systems</b></h2>
<h4 class="text-center" style="color:#3498DB"><b>Fahrenheit to Celsius
Converter</b></h4> </div>
<div class="modal-body" style="background:#3498DB"> <form class="form col-md-12 center-block">
<div class="form-group" ><br><br>
<center style="color:black"><h3><b>Celsius Result</b> : <%= title %></h3>
</div>
</form> </div>
<div class="modal-footer" style="background:#3498DB"> <div class="col-md-12">
</div> </div>
</div>
</div> </div> </form>
<!-- script references --> <script
src="//ajax.googleapis.com/ajax/libs/jquery/2.0.2/jquery.min.js"></script> <script src="js/bootstrap.min.js"></script>
</body> </html>
```
