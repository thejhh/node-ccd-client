
CCD JSON Protocol
=================

This is documentation for CCD (Control Center Daemon) protocol which has been 
developed as a generic API for our clients to manage Sendanor's Services.

Requests
--------

Requests are standard HTTP POST requests to `https://ccd.sendanor.com/ccd.fcgi` with 
request body in JSON format and `Content-Type` set as `application/json`.

Request Format
--------------

Request is an array of command objects.

Single `help` request looks in JSON like this:

	[{"command":"help"}]

Request for `dummy foo=bar` has an optional `options` property:

	[{"command":"dummy","options":{"foo":"bar"}}]

After a successful `login` command, each command is required to have a `session_id` property. The value is returned from the `login` command.

	[{"session_id":"","command":"dummy","options":{"foo":"bar"}}]

Response Format
---------------

The response is a JSON object with `messages` and/or `records` properties. These 
properties are arrays.

### Records

Record object is an array of objects with key-value pairs.

Please note: The format may look bizarre because it was originally designed for 
XML and transfered into JSON by Perl module. There will most likely be an 
optional redesigned response format in the future.

A response for `dummy` command might look in JavaScript like:

	{"command":"dummy"}

...and as a CCD response:

	{"records":[{"command":"dummy"}]}

A response for `dummy foo=bar` might look in JavaScript like:

	{"command":"dummy","options":{"foo":"bar"}}

...and as a CCD-response:

	{"records":[{"key1":"value1"},{"key2":{"foo":"bar"}}]}

### Messages and Errors

Errors are returned inside property `messages` which is an array:

	{"messages":[...]}

Each message is an object with properties `type` and `subject`:

	{"type":"error","subject":"Internal Error"}

Complete JSON response for `Internal Error` might look like this:

	{"messages":[{"type":"error","subject":"Internal Error"}]}

API Command Reference
---------------------

CCD has been designed to provide the API reference to all commands 
automatically from the server itself. The user can use the `help` command or 
[our web manual](https://ccdmanual.sendanor.com/) to access the API reference 
documentation.
