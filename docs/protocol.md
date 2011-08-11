
CCD JSON Protocol
=================

This is documentation for CCD (Control Center Daemon) protocol which has been 
developed as a generic API for our clients to manage Sendanor's Services.

Request
-------

Requests are standard HTTP POST requests to `https://ccd.sendanor.com/ccd.fcgi` with 
request body in JSON format and `Content-Type` set as `application/json`.

Request body
------------

Request is an array of command objects.

Single `help` command looks in JSON like this:

	[{"command":"help"}]

Request for `ccd dummy foo=bar` has an optional `options` property:

	[{"command":"dummy","options":{"foo":"bar"}}]

After a successful `login` command, each command is required to have a `session_id` property. The value is returned from the `login` command.

	[{"session_id":"","command":"dummy","options":{"foo":"bar"}}]

Reply
-----

The reply is a JSON object with `messages` and/or `records` properties. These 
properties are arrays.

Record Replies
--------------

Record object is an array of objects with key-value pairs.

Please note: The format may look bizarre because it was originally designed for 
XML and transfered into JSON by Perl module. There will most likely be an 
optional redesigned reply format in the future.

A reply for `dummy` might look in JavaScript like:

	{"command":"dummy"}

...and as a CCD reply:

	{"records":[{"command":"dummy"}]}

A reply for `dummy foo=bar` might look in JavaScript like:

	{"command":"dummy","options":{"foo":"bar"}}

...and as a CCD-reply:

	{"records":[{"key1":"value1"},{"key2":{"foo":"bar"}}]}

Messages and Errors
-------------------

Errors are returned inside property `messages` which is an array:

	{"messages":[...]}

Each message is an object with properties `type` and `subject`:

	{"type":"error","subject":"Internal Error"}

Full JSON reply for Internal Error might look like this:

	{"messages":[{"type":"error","subject":"Internal Error"}]}
