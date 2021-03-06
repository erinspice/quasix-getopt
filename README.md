# Tests
[![Build Status](https://travis-ci.org/erinspice/quasix-getopt.svg?branch=master)](https://travis-ci.org/erinspice/quasix-getopt)

# Install
    npm install quasix-getopt

# Description
This module provides dependency-free parsing of a simple subset of POSIX
command line argument syntax.

This module does not parse POSIX.1-2008 or any other official
specification. This module parses the following simple POSIX-like
constructs.

## Use
Parse process.argv and return an object with the command line
arguments in it. This module does not enforce any required options
or value formats. It blindly parses assuming a standardish POSIX format
and returns all values passed to the command.

    const quasix = require('quasix-getopt')
    const options = quasix.parse()

## Examples
* single-dash arguments

```
    // process.argv:
    // ['cmd', 'file', '-a']
    {
        a: true
    }
```

* single-dash combined arguments

```
    // process.argv:
    // ['cmd', 'file', '-zxvpf']
    {
        z: true,
        x: true,
        v: true,
        p: true,
        f: true
    }
```

* single-dash combined arguments with a bare value following

```
    // process.argv:
    // ['cmd', 'file', '-zxvpf', 'blah.tgz']
    {
        z: true,
        x: true,
        v: true,
        p: true,
        f: true,
        _extras: ['blah.tgz']
    }
```

* single-dash arguments with value

```
    // process.argv:
    // ['cmd', 'file', '-o', 'data.txt']
    {
        o: 'data.txt'
    }
```

* single-dash arguments with value delimited with equal `=`

```
    // process.argv:
    // ['cmd', 'file', '-o=data.txt']
    {
        o: 'data.txt'
    }
```

* double-dash arguments

```
    // process.argv:
    // ['cmd', 'file', '--verbose']
    {
        verbose: true
    }
```

* double-dash arguments with value

```
    // process.argv:
    // ['cmd', 'file', '--outfile', 'data.txt']
    {
        outfile: 'data.txt'
    }
```

* double-dash arguments with value delimited with equal `=`

```
    // process.argv:
    // ['cmd', 'file', '--outfile=data.txt']
    {
        outfile: 'data.txt'
    }
```

* bare keywords

```
    // process.argv:
    // ['cmd', 'file', 'add', 'default', 'gw', '192.168.1.10']
    {
        _extras: ['add', 'default', 'gw', '192.168.1.10']
    }
```

### Counter-examples

These syntaxes are not parsed as expected.

* single-dash multi-letter option names, like route's `-net` argument. Example: `route add -net 12.34.0.0 netmask 255.255.0.0 gw 12.34.56.1` Result:

```
    {
        n: true,
        e: true,
        t: true,
        _extras: ['add', '12.34.0.0', 'netmask', '255.255.0.0', 'gw', '12.34.56.1']
    }
```
