# Java IO

## I/O Streams

An _I/O Stream_ represents an input source or an output destination. A stream can represent many different kinds of sources and destinations, including disk files, devices, other programs, and memory arrays.

Streams support many different kinds of data, including simple bytes, primitive data types, localized characters, and objects. Some streams simply pass on data; others manipulate and transform the data in useful ways.

No matter how they work internally, all streams present the same simple model to programs that use them: A stream is a sequence of data. A program uses an _input stream_ to read data from a source, one item at a time:

![Reading information into a program.](https://docs.oracle.com/javase/tutorial/figures/essential/io-ins.gif)



A program uses an _output stream_ to write data to a destination, one item at time:

![Writing information from a program.](https://docs.oracle.com/javase/tutorial/figures/essential/io-outs.gif)

The data source and data destination pictured above can be anything that holds, generates, or consumes data. Obviously this includes disk files, but a source or destination can also be another program, a peripheral device, a network socket, or an array.

[https://docs.oracle.com/javase/tutorial/essential/io/index.html](https://docs.oracle.com/javase/tutorial/essential/io/index.html)
