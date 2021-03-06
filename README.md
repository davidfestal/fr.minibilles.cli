`fr.minibilles.cli` allows to create command line interfaces (CLI) using simple annotations.

[![Build Status](https://travis-ci.org/jeancharles-roger/fr.minibilles.cli.svg?branch=master)](https://travis-ci.org/jeancharles-roger/fr.minibilles.cli)

# Getting Started

First import the module: `import fr.minibilles.cli;`.

Using `option` and `parameters` annotation you can simply create a CLI:

Here is a simple example for a declaration:

```ceylon
"Simple example for command line"
parameters({`value files`})
shared class Test(
	"Files to process"
	shared [String*] files,
	
	"Shows this help"
	option("help", 'h')
	shared Boolean help,
	
	"Presents version"
	option("version", 'v')
	shared Boolean version,
	
	"Show some lines"
	option("show", 's')
	shared Integer showLine
) { }
```

This is how to use it:

```ceylon
// prints help
print(help<Test>("test"));

// parses some arguments
value [test, errors] = parseArguments<Test>(["-h", "--show", "10", "file1.txt", "file2.txt"]);

// prints the result and the errors if any
if (exists test) {
	print(optionsAndParameters(test));
}
print(errors);
```

The result:

```
Usage: test [options] [files]
  Simple example for command line
  where:
    
  - --help | -h : Shows this help
  - --show=value | -s value: Show some lines
  - --version | -v : Presents version

Test:
-help: true
-showLine: 10
-version: false
-files: [file1.txt, file2.txt]
[]
```

# Supported types

Here are the supported types:

- `String`
- `Boolean`
- `Float`
- `Integer`
- Case objects will search for the object with the matching `string`, for instance:

```ceylon
interface Command of start|stop|restart {}
object start satisfies Command { 
	shared actual String string = "start";
}
object stop satisfies Command {
	shared actual String string = "stop";
}
object restart satisfies Command {
	shared actual String string = "restart";
}
```

For any other type, a `creator` annotation can be added to set a creator function. It reference a function taking a `String` as parameter and retreiving the value type or nothing. For instance:

```ceylon
option("source") creator(`function parsePath`)
shared Path source = parsePath("/")
```

`Sequential` allows to defines multiple arguments: `[String+] files`. (see limitations for multiple arguments).

# Known limitations

- Multiple value can only be `[String*]` for now (will be fixed later on).
- In the `parameters` list the first sequential value found will use all the remaining arguments.
- Multiple parameters must be of type `Sequential`, `Iterable` isn't supported.
- Inheritance declaration haven't been tested (to be done).
- Case object won't be correctly printed for the help (to be done).

# Things to come

- Read the same options from a JSON configuration file.

# Examples

Here are some examples:

- [Test](https://github.com/jeancharles-roger/fr.minibilles.cli/blob/master/source/examples/fr/minibilles/cli/test.ceylon)
- [Grep](https://github.com/jeancharles-roger/fr.minibilles.cli/blob/master/source/examples/fr/minibilles/cli/grep.ceylon)
- [Server](https://github.com/jeancharles-roger/fr.minibilles.cli/blob/master/source/examples/fr/minibilles/cli/server.ceylon)

