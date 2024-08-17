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



## PDE Type Codes 
The first byte of a PDE field is the PDE field's type code. The available PDE field type codes are listed in the table below. 


| Type Code | Field Name      | Field Description                                                                   |
|-----------|-----------------|-------------------------------------------------------------------------------------|
| 0         | BOOLEAN_NULL    | A single byte with the value 0 - representing a boolean field with a null value.    |
| 1         | BOOLEAN_TRUE    | A single byte with the value 1 - representing a boolean field with the value true.  |
| 2         | BOOLEAN_FALSE   | A single byte with the value 2 - representing a boolean field with the value false. |
| 3         | INT_NULL        | A single byte representing an Integer field with the value null.                    | 
| 4         | INT_POS_1_BYTES | An positive integer field which value is 1 byte long.                               |
| 5         | INT_POS_2_BYTES | An positive integer field which value is 2 bytes long.                              |
| 6         | INT_POS_3_BYTES | An positive integer field which value is 3 bytes long.                              |
| 7         | INT_POS_4_BYTES | An positive integer field which value is 4 bytes long.                              |
| 8         | INT_POS_5_BYTES | An positive integer field which value is 5 bytes long.                              |
| 9         | INT_POS_6_BYTES | An positive integer field which value is 6 bytes long.                              |
| 10        | INT_POS_7_BYTES | An positive integer field which value is 7 bytes long.                              |
| 11        | INT_POS_8_BYTES | An positive integer field which value is 8 bytes long.                              |
| 12        | INT_NEG_1_BYTES | An negative integer field which value is 1 byte long.                               |
| 13        | INT_NEG_2_BYTES | An negative integer field which value is 2 bytes long.                              |
| 14        | INT_NEG_3_BYTES | An negative integer field which value is 3 bytes long.                              |
| 15        | INT_NEG_4_BYTES | An negative integer field which value is 4 bytes long.                              |
| 16        | INT_NEG_5_BYTES | An negative integer field which value is 5 bytes long.                              |
| 17        | INT_NEG_6_BYTES | An negative integer field which value is 6 bytes long.                              |
| 18        | INT_NEG_7_BYTES | An negative integer field which value is 7 bytes long.                              |
| 19        | INT_NEG_8_BYTES | An negative integer field which value is 8 bytes long.                              |
| 20        | FLOAT_NULL      | A 1 byte long field representing a float field with a null value.                   |
| 21        | FLOAT_4_BYTES   | A float field which value is 4 bytes long (32 bit float).                           |
| 22        | FLOAT_8_BYTES   | A float field which value is 8 bytes long (64 bit float).                           |
