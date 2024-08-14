# Polymorph Data Encoding (PDE) Specification

Polymorph Data Encoding (PDE) is a binary data encoding that is versatile, flexible, compact and fast to read and write.
See more about the purpose of PDE in the README file here:

[README.md](README.md)

Polymorph Data Encoding is an alternative to MessagePack, CBOR, Protobuf, ION, Avro and other binary data formats.
See more about how PDE compares to other data formats in the README file here:

[README.md](README.md)

## PDE Tutorial
The PDE encoding is explained in a more tutorial style here:

[https://jenkov.com/tutorials/polymorph-data/polymorph-data-encoding.html](https://jenkov.com/tutorials/polymorph-data/polymorph-data-encoding.html)

## PDE Overview

A PDE file (or stream) consists of a stream of PDE fields. 

Some fields are atomic, meaning they only contain raw data inside them. Other fields are composite, meaning
they contain other PDE fields inside them.

Each PDE field has a type, length and value. The type, length and value can all be read from the bytes in
the PDE byte stream.

A PDE field starts with a single byte (the type code) that tells the type + some length information.
The length information is either the exact length in bytes of the value, or the number of length bytes used
to encode the actual length in bytes of the value.



| Type Code | Field Name    | Field Description                                                                   |
|-----------|---------------|-------------------------------------------------------------------------------------|
| 0         | BOOLEAN_NULL  | A single byte with the value 0 - representing a boolean field with a null value.    |
| 1         | BOOLEAN_TRUE  | A single byte with the value 1 - representing a boolean field with the value true.  |
| 2         | BOOLEAN_FALSE | A single byte with the value 2 - representing a boolean field with the value false. |
