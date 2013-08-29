absurd
======

Writing your CSS in JavaScript. That's it!

## Installation

	npm install -g absurd

## Usage

Absurd is really simple. It just gets instructions and base on them produces css. So, it's just a matter of personal choice where to use it. You may want to integrate the module directly into your application. If that's not ok for you then you are able to compile the css by using the command line.

### Inline styles

	var Absurd = require("absurd");
	Absurd(function(A) {
		A.add({
			body: {
				'margin-top': '20px',
				'padding': 0,
				'font-weight': 'bold',
				section: {
					'padding-top': '20px'
				}
			}
		});
	}).compile(function(err, css) {
		/* 
			css contains:

			body {
				margin-top: 20px;
				padding: 0;
				font-weight: bold;
			}
			body section {
				padding-top: 20px;
			}

		*/
	});

### Puting the styles in an external file

*/css/styles.js*

	module.exports = function(A) {
		A.add({
			body: {
				'margin-top': '20px',
				'padding': 0,
				'font-weight': 'bold',
				section: {
					'padding-top': '20px'
				}
			}
		});
	}

*/app.js*

	Absurd("./css/styles.js").compile(function(err, css) {
		// ...
	});

### Compiling to file

	Absurd("./css/styles.js").compileFile("./css/styles.css", function(err, css) {
		// ...
	});

### Using through command line interface

	// outputs the css in the console
	absurd -s [source file] 

	// outputs the css to a file
	absurd -s [source file] -o [output file]

## Syntax

In the context of JavaScript the closest thing to pure CSS syntax is JSON format. So, every valid JSON is actually valid Absurd syntax. As you can see above, there are two ways to send styles to the module. In both cases you have an access to the Absurd's API. For example:

	Absurd(function(API) {
		API.add({});
	});

or in an external js file

	module.exports = function(API) {
		API.add({});
	}

The *add* method accepts valid JSON. This could be something like

	body: {
		'margin-top': '20px',
		'padding': 0,
		'font-weight': 'bold',
		section: {
			'padding-top': '20px'
		}
	}

Every object defines a selector. Every property of that object could be a property and its value or another object. For example the JSON above is compiled to:

	body {
		margin-top: 20px;
		padding: 0;
		font-weight: bold;
	}
	body section {
		padding-top: 20px;
	}

### Pseudo classes

It's similar like the example above:

	a: {
		'text-decoration': 'none',
		'color': '#000',
		':hover': {
			'text-decoration': 'underline',
			'color': '#999'
		},
		':before': {
			content: '"> "'
		}
	}

Is compiled to:

	a {
		text-decoration: none;
		color: #000;
	}
	a:hover {
		text-decoration: underline;
		color: #999;
	}
	a:before {
		content: "> ";
	}

### Importing

Of course you can't put everything in a single file. That's why there is *.import* method:

	/css/index.js

	module.exports = function(API) {
		API.import(__dirname + '/config/colors.js');
		API.import(__dirname + '/config/sizes.js');
		API.import([
			__dirname + '/layout/grid.js',
			__dirname + '/forms/login-form.js',
			__dirname + '/forms/feedback-form.js'
		]);
	}

## Testing

The tests are placed in */tests* directory. Install [jasmine-node](https://github.com/mhevery/jasmine-node) and run

	jasmine ./tests