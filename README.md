Apoc
====

Apoc is a node module and a command-line tool to make Cypher queries easier.

## Installation

As a node module:

```
$ npm install apoc --save
```

As a command-line tool:

```
$ npm install apoc -g
```

## Apoc Cypher File

An Apoc Cypher file is a text file with **.acf** extension, which contains cypher queries in it. It supports comments using `//`, and can include other acf files from the main file. Here is an example.

The contents of the main acf file:

```
// in-file query
create (m: ApocTestMember {
    name: 'El Capitan'
})

// groups
include groups.acf

// roles
include roles.acf
```

The contents of groups.acf:

```
create (g:ApocTestGroup {
    name: 'Hackers'
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

As a node module:

```
var apoc = require('apoc')
var query = apoc('index.acf')
```

When used as a node module, apoc will generate the query only. It won't send the query to the Neo4j server. Future versions will support query execution too.

From the commandline:

```
$ apoc index.acf
```

Executing an acf file from the commandline with apoc is executing the contents on the Neo4j server. Excellent for writing your queries in the acf files and executing them from the commandline, while taking advantage of acf features.

## License

Copyright (c) 2014 Hage Yaapa &lt;captain@hacksparrow.com&gt;

Permission to use, copy, modify, and/or distribute this software for any purpose with or without fee is hereby granted, provided that the above copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.
