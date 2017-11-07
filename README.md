# ld-json
Line delimited JSON for large data sets

## Why
On a standard implementation of NodeJS trying to stringify a large JSON object can result in a `RangeError: Invalid string length` or `JavaScript heap out of memory` error. This is because the size of the object that is being converted cannot fit in memory, you can alleviate this to an extent by changing the memory limits of the Node process but if you are in the cloud, it can be costly to run instances with large memory resources.

A better way is using ld-json (Line Delimetered JavaScript Object Notation). It is a version of a flat JSON document that is not encapsulated inside a root object.

*Traditional JSON*
```json
{ "data": [
    { "key": 1, "value": "first row" },
    { "key": 2, "value": "second row" }
  ]
}
```

*LD-JSON*
```json
{ "key": 1, "value": "first row" }
{ "key": 2, "value": "second row" }
```

Traditional JSON needs the entire document to be stored in memory before you can work with it, where LD-JSON can utalise streams and buffers so that it doesn't need to be kept in memory allowing smaller instances and less configuration. There are downsides to not being stored in memory mainly around speed but there are some scenarios where ld-json would be optimal.

## Scenario 1 - Reading

Most analytic packages will distribute clicks with CSV/TSV files and these work well for the most part, but having the data explicity mapped in JSON format is also useful.

Say you have an array of 250,000 log lines inside an json file (around 200mb) load into your project, filter and count the results takes around 6.227 seconds.

The equivalent in ld-json runs in just 2.824 seconds. Not only is this a huge performance jump
