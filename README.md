# Web-applications theory

Application software - computer implementation of dictionary and grammar for language targeting specific human activity.

Web - network of computers providing resources and services using standard-based interfaces.

Web-applications (that are discussed here) - application software that works in web.


# Web-applications reality

There is gigantic amount of technical complexity covered by popular names and concepts.

Now we need three of them:

 * HTML/CSS - basis for GUI.
 * Javascript - language for development.
 * NodeJS - server framework.

## HTML/CSS

Abomination. Sorry :) but this is just one of the worst implementations of one of the greatest ideas in software development.

Problem - we need graphical UI. More often than not we need inputs, buttons, tables and their crazy combinations.

Solutions:

 * programming motherf*cker right in the code
 * declarative standardized language

While first solution seems good it really sucks when you have have more than 10 elements on your screen, or you want to reuse it, or you want to collaborate with designer, or port it to other platform. I think you have got my point.

And HTML was created with second solution in mind. When the sexiest technology was ...XML. And even if you use erb, jade, wtf, gtfo or other template-engine in the end of the day it will be translated to HTML.

But in its basic state it looks like... it's [1996](http://ekarj.com/internet96.htm) again! That is patched and fixed by duck-tape called Cascade Style Sheets. Standard for prettifying all this XML-based atrocity. Intentions were all good but when you dig a bit dipper you feel like... you want kill something right away.

[This](http://learnlayout.com/) may be useful.

### Simple examples

I am ready to stop whining and post some code.

```html
<!DOCTYPE html>
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Simple web application</title>
	</head>
	<body>
		<div>Hello world!</div>
	</body>
</html>
```

Let's make it more like GUI.

```html
<!DOCTYPE html>
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Simple web application</title>
	</head>
	<body>
		<div>Our simple GUI</div>
		<input type="text" value="simple data"></input>
		<button>Simple button!</button>
	</body>
</html>
```

Let's add a link to another resource.

```html
<!DOCTYPE html>
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Simple web application</title>
	</head>
	<body>
		<div>Our great link</div>
		<div><a href="some_resource">open new stuff</a></div>
	</body>
</html>
```

### Useful examples

So how can we send a POST request from html page?

```html
<!DOCTYPE html>
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Simple web application</title>
	</head>
	<body>
		<form action="this_url_will_be_opened" method="POST">
			<input type="text"	 value="simple data" name="data"></input>
			<input type="submit" value="Send!"></input>
		</form>
	</body>
</html>
```

After clicking "Send!" browser will open address specified in action attribute.

## Javascript

It is not Java and I kind of like it.

### Simple examples

Save this in index.html and open in your browser.

```javascript
<script>
	alert("Hello world!");
</script>
```

You can create structures useful for you current situation.

```javascript
<script>
	var myStructure = {"firstWord": "Hello", "secondWord": "world"};

	alert(myStructure.firstWord + " " + myStructure.secondWord + "!");
</script>
```

I hate that +++ thing. Let's move it!

```javascript
<script>
	var myStructure = {
		"firstWord": "Hello",
		"secondWord": "world",
		"toString": function() {
			return this.firstWord + " " + this.secondWord + "!";
		}
	};

	alert(myStructure.toString());
</script>
```

But I like another approach.

```javascript
<script>
	var concatArray = function(elements) {
		var result = "";

		for (var i = 0; i < elements.length; i++)
			result += elements[i];

		return result;
	}

	alert(concatArray(["Hello", " ", "world", "!"]));
</script>
```

One more trick :) with functions.

```javascript
<script>
	var concatArray = function(elements) {
		var result = "";

		for (var i = 0; i < elements.length; i++)
			result += elements[i];

		return result;
	}

	var concatArrayWithCallback = function(data, callback) {
		callback(concatArray(data));
	}

	concatArrayWithCallback(
		["Hello", " ", "world", "!"],
		function(data) {
			alert(data);
		}
	);
</script>
```

Tastes differ. But composing program from small understandable pieces works for me.

### Useful examples

Now will be the lousiest part of javascript usage. It is called [DOM API](#dom-api). Most people prefer to call it jQuery.

Problem - make something happen on button click!

Solutions:

 * extend some instance of some class
 * use some magical object to get another special object to set special field to special function

Wann guess? Yeap, we've got the second case.

```html
<!DOCTYPE html>
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Simple web application</title>
		<script>
			var button = document.getElementsByTagName("button")[0];

			button.onclick = function(e) {alert("clicked!");}
		</script>
	</head>
	<body>
		<div>Our simple GUI</div>
		<input type="text" value="simple data"></input>
		<button>Simple button!</button>
	</body>
</html>
```

But know what is a problem? It aint works.

If you will open developer tools in your preferred browser. like F12 or Ctrl+Shift+i. And point to console you would see:

```
TypeError: button is undefined
```

Actually our button variable is empty. Because whole page is computed like scenario from top to the bottom. And at the moment of our search there is no button element yet.

Here comes the band-aid.

```html
<!DOCTYPE html>
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Simple web application</title>
		<script>
			window.onload = function() {
				var button = document.getElementsByTagName("button")[0];

				button.onclick = function(e) {alert("clicked!");}
			}
		</script>
	</head>
	<body>
		<div>Our simple GUI</div>
		<input type="text" value="simple data"></input>
		<button>Simple button!</button>
	</body>
</html>
```

One more thing and we can interact with our input element.

```html
<!DOCTYPE html>
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Simple web application</title>
		<script>
			window.onload = function() {
				var button = document.getElementsByTagName("button")[0];

				var input = document.getElementsByTagName("input")[0];

				button.onclick = function(e) {alert(input.value);}
			}
		</script>
	</head>
	<body>
		<div>Our simple GUI</div>
		<input type="text" value="simple data"></input>
		<button>Simple button!</button>
	</body>
</html>
```

Cool things ahead! Stay tuned.

## NodeJS

We are talking about [this](https://nodejs.org/) thing.

You should download binary for your platform and bla-bla-bla. They have really great tutorial on their site. Yet I am gonna copy it here.

### Simple examples

App that responds with "Hello World" for every request.

```javascript
require('http').createServer(
	function (req, res) {
		res.writeHead(200, {'Content-Type': 'text/plain'});
		res.end('Hello World\n');
	}
).listen(1337, '127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
```

Put the code into a file server.js and call it with node executable from the command line:

```shell
% node server.js
```

If everything is fine you should see:

```shell
Server running at http://127.0.0.1:1337/
```

Improved yet naive version that sends our index.html file on every request.

```javascript
var fs = require("fs");

require("http").createServer(
	function (req, res) {
		fs.readFile("./index.html", function (err, data) {
			if (err) {
				res.writeHead(404);
				res.end('Not found' + err);
			}

			res.end(data);
		});
	}
).listen(1337, '127.0.0.1');
console.log('Our super server running at http://127.0.0.1:1337/');
```

### Useful examples

Let's read data that was sent with POST request and send it back to browser.

```javascript
var fs = require("fs");

require("http").createServer(
	function (req, res) {
		if (req.method == 'POST') {
			console.log("POST request on: " + req.url);

			var body = '';
			req.on('data', function (data) {
				body += data;
			});

			req.on('end', function () {
				res.writeHead(200);
				res.end("data: " + body);
			});
		}
		else if (req.method == 'GET') {
			console.log("GET request on: " + req.url);

			fs.readFile("./index.html", function (err, data) {
				if (err) {
					res.writeHead(404);
					res.end('Not found' + err);
				}

				res.end(data);
			});
		}
	}
).listen(1337, '127.0.0.1');
console.log('Our sophisticated server running at http://127.0.0.1:1337/');
```

It is done using NodeJS mechanism of events. On each "data" event we read part of request-body and append it to variable body. When event "end" is triggered it means that reading is finished and we can send the result to a client.

But what if we want to provide different files on differen queries?

```javascript
var fs = require("fs");

require("http").createServer(
	function (req, res) {
		if (req.method == 'POST') {
			console.log("POST request on: " + req.url);

			var body = '';
			req.on('data', function (data) {
				body += data;
			});

			req.on('end', function () {
				res.writeHead(200);
				res.end("data: " + body);
			});
		}
		else if (req.method == 'GET') {
			console.log("GET request on: " + req.url);

			if (req.url == '/some_resource') {
				res.writeHead(200);
				res.end("some new stuff");
			}
			else {
				fs.readFile("./index.html", function (err, data) {
					if (err) {
						res.writeHead(404);
						res.end('Not found' + err);
					}

					res.end(data);
				});
			}
		}
	}
).listen(1337, '127.0.0.1');
console.log('Our sophisticated server running at http://127.0.0.1:1337/');
```

## Popular names and concepts

### HTTP

To me HyperText means nothing but Transfer Protocol sounds promising.

So if we have two computers, how they gonna understand each other? Thay have a special format for their messages!

For example you want some index.html page:

```
GET http://127.0.0.1:1337/index.html HTTP/1.1
Host: 127.0.0.1:1337
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:35.0) Gecko/20100101 Firefox/35.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://127.0.0.1:1337/
Connection: keep-alive

This is body section. It must end with newline.

```

There are FIVE sections:

 * first word is method name (GET, POST, PUT, DELETE, etc)
 * then goes address of required resource
 * HTTP-Version
 * each line of "name: value" represents header field (section ends with newline)
 * additional content for request

[This](http://www.w3.org/Protocols/rfc2616/rfc2616-sec5.html) may be useful for most boring people.

In best case scenario after sending request you will get response.

```
HTTP/1.1 200 OK
Content-Type: text/html

This is body section. It must end with newline.

```

First line represents status of request. There are plenty of other codes. You can find 404 :).

### DOM API

This name stands for Domain Object Model aaaaand it says nothing to me too.

Short smart answer - browser-implemented API for interacting with HTML-document.

When you load your html-page browser treats it as tree-structure of DOM nodes.

It abstracts user behaviour as sequence of events which can be processed by your javascript code.

### URL

You can read [wikipedia](http://en.wikipedia.org/wiki/Uniform_resource_locator) as for me it is just an address of resource or service.