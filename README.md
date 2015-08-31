Apoc
====

Apoc is a node module and a command-line tool to make Cypher queries dynamic and powerful. It adds the following features on top of Cypher.

* Comments using `//`
* JavaScript code within backticks
* Variables within %% (when used as a node module)
* Include other acf files from the main file

## Installation

As a node module:

```
$ npm install apoc --save
```

As a command-line tool:

```
$ npm install apoc -g
```

## Configuration

Apoc will look for your neo4j configuration details in two places - environment variables and apoc.yml in your home directory, in that order.

**Environment variables**

NEO4J_HOST
NEO4J_PORT
NEO4J_USERNAME
NEO4J_PASSWORD

**apoc.yml**:

```
host: 192.168.0.8
port: 2902
username: neo4j
password: neo4j
```

Hostname and port will default to 127.0.0.1 and 4747, respectively.

## Apoc Cypher File

An Apoc Cypher file is a text file with **.acf** extension, which contains Cypher queries in it with extended acf features. Here is an example.

The contents of the main acf file:

```
create (m: ApocTestMember {
  id: '`Date.now()`', // JavaScript code
  name: 'El Capitan',
  twitter: '%twitter%' // template code
})

// groups - can include files relative to the acf file ONLY
include groups.acf

// roles
include roles.acf
```

The contents of groups.acf:

```
create (g:ApocTestGroup {
  name: 'Hackers',
  id: '`Math.floor(Math.random()*1000)`'
})
```

The contents of roles.acf:

```
create (r: ApocTestRole {
  name: 'Designer'
})
```

The consolidated content is generated as a single query, which makes the identifiers defined in individual files global.

Future versions of apoc will enable including acf files from non-main files and writing Cypher queries even easier.

## Usage

### As a node module

Apoc exposes two methods:

|Name|Description
|----|----------
|**apoc.query**(query \| apoc file, [variables], [port], [host])| For making custom queries.
|**apoc.insert**(objects, [port], [host])| For insert (including bulk inserts). Returns a promise.

**apoc.query()** accepts a cypher query, or an acf file path and an optional object to with the variables for the template system.

It returns an object with the following properties:

|Name|Description
|----|----------
|**text**|Text of the consolidated query.
|**exec()**|Method to execute the query at the server. It returns a promise.

Usage example (inline cypher query):

```
var apoc = require('apoc')

// specify a cypher query and execute it on the server
apoc.query('match (n) return n').exec().then(function (response) {
  console.log(response)
}, function (fail) {
  console.log(fail)
})
```

Usage example (with acf file):

```
var apoc = require('apoc')

// generate the consolidated cypher query from the acf file
var query = apoc.query('index.acf', { twitter: '@hacksparrow' })

// the consolidated query
console.log(query.text)

// execute the query at localhost:7474
query.exec().then(function (response) {
  console.log(response)
}, function (fail) {
  console.log(fail)
})

```

**apoc.insert()** accepts an array of objects to be inserted, and optional `port` and `host` params.  

```
var users = [{name: 'A'}, {name: 'B'}, {name: 'C'}]
apoc.insert(users).then(function (response) {
  console.log(response)
}, function (fail) {
  console.log(fail)
})
```

### From the commandline

```
$ apoc index.acf
```

Execute acf files with apoc like they were shell scripts. Excellent for writing your queries in the acf files and executing them from the commandline, while taking advantage of acf features.

## API

Apoc provides useful and commonly used algorithms and functions.

**md5**

```
create (r1: ApocTestRole {
  name: 'Designer',
  id: '`md5('apple')`'
})

```

**[bcrypt](https://www.npmjs.org/package/bcrypt)**

```
create (g: ApocTestUser {
  name: 'Apoc',
  secret: '`bcrypt.hashSync('sekr37', bcrypt.genSaltSync(10))`'
})
```

Many more will be added.

## Development

Check out this repo

make sure to run npm test when you have made any changes
make sure to add your own unit test and run npm test

## License (MIT)

Copyright (c) 2015 Hage Yaapa

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
