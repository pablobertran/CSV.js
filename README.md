CSV.js
======

Simple CSV parsing/encoding in JavaScript. Full [RFC 4180](http://tools.ietf.org/html/rfc4180) compliance.

Compatible with browsers, AMD, and NodeJS.


Installation
------------

Download `csv.min.js` and reference to it using your preferred method.

If you use **Bower**, or **npm**, install the `comma-separated-values` package.


Instantiation
-------------

Create a CSV instance by running `var csv = new CSV();`. You can supply options with the format `var csv = new CSV({ option: value });`.

Available options:
```javascript
{
  delimiter: string          // The character(s) separating values in a row. Defaults to ','.
  header: boolean or array   // First row of CSV contains headers (true), or not (false). Supply your own header (array). Defaults to false.
  replace: boolean           // Replace the first row of data with the supplied header (true), or not (false). Defaults to false.
  stream: function           // A function to call after every row is parsed. Defaults to undefined.
  done: function             // A function to call after all rows are parsed. Defaults to undefined.
}
```

You can update an option's value any time after instantiation with `csv.set(option, value)`.


Parsing
-------

If the CSV contains headers, `csv.parse()` parse will return objects with properly-set properties.

With the following example:

```javascript
var csv = new CSV({ header: true });
csv.parse('\
  "year","age","status","sex","people"\r\n\
  1850,20,0,1,1017281\r\n\
  1850,20,0,2,1003841\r\n\
  1850,25,0,1,862547\r\n\
  1850,25,0,2,799482\r\n\
  1850,30,0,1,730638\r\n\
  1850,30,0,2,639636\r\n\
  1850,35,0,1,588487\r\n\
  1850,35,0,2,505012\r\n\
  1850,40,0,1,475911\r\n\
  1850,40,0,2,428185\r\n\
');
```

Optionally, supplying your own header field:

```javascript
var csv = new CSV({ header: ["year","age","status","sex","people"] });
csv.parse('\
  1850,20,0,1,1017281\r\n\
  1850,20,0,2,1003841\r\n\
  1850,25,0,1,862547\r\n\
  1850,25,0,2,799482\r\n\
  1850,30,0,1,730638\r\n\
  1850,30,0,2,639636\r\n\
  1850,35,0,1,588487\r\n\
  1850,35,0,2,505012\r\n\
  1850,40,0,1,475911\r\n\
  1850,40,0,2,428185\r\n\
');
```

The response would be:

```javascript
[
  {year: 1850, age: 20, status: 0, sex: 1, people: 1017281},
  {year: 1850, age: 20, status: 0, sex: 2, people: 1003841},
  ...
]
```

If `header` is set to false, the response will contain arrays as opposed to objects.


Encoding
--------

Pass an **array of objects** to `csv.encode()` to get CSV, sans headers.
Run `csv.set("header", true)` to add headers.

Pass an **array of arrays** to `csv.encode()` to get CSV, sans headers.
Run `csv.set("header", headerValues)`, where `headerValues` is an array with your own header values.


Events
------

If memory is an issue, you can have CSV.js call a function after each row is parsed (or encoded) by setting `stream` to a function that receives an object or an array, as appropriate.

When all the CSV has been parsed (or encoded), CSV.js will call `done`, and supply the full response. **Note that the response will be empty if `stream` has been set**.
