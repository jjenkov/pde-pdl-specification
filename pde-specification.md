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

Here are some examples in hexadecimal representation, showing

- A PDE field consisting of only 1 byte (the field type code byte)
- A PDE field consisting of 1 byte (the field type code byte) and some value bytes
- A PDE field consisting of 1 byte (the field type code byte), some length bytes and some value bytes

Remember, all integers, floats etc. are encoded using little endian encoding, meaning the least significant byte of the number is the first byte in the sequence.


    00            # A boolean null field
    01            # A boolean true field
    02            # A boolean false field

    04   A3       # A positive integer field with a 1 byte value
    05   A3 0E    # A positive integer field with a 2 byte value - little endian encoded ( => 0EA3 => 3747 in decimal)

    29   00 01   XX XX XX XX ...    # A bytes field with 2 length bytes (0 and 1 in little endian => 0100 as hex number => 256 in decimal) and the XX'es 
                                    # represents the value bytes (there should be 256 in total - because that is what the length bytes say).
    



## PDE Type Codes 
The first byte of a PDE field is the PDE field's type code. The available PDE field type codes are listed in the table below. 


| Type Code | Field Name           | Field Description                                                                   |
|-----------|----------------------|-------------------------------------------------------------------------------------|
| 0         | BOOLEAN_NULL         | A single byte with the value 0 - representing a boolean field with a null value.    |
| 1         | BOOLEAN_TRUE         | A single byte with the value 1 - representing a boolean field with the value true.  |
| 2         | BOOLEAN_FALSE        | A single byte with the value 2 - representing a boolean field with the value false. |
| 3         | INT_NULL             | A single byte representing an Integer field with the value null.                    | 
| 4         | INT_POS_1_BYTES      | An positive integer field which value is 1 byte long.                               |
| 5         | INT_POS_2_BYTES      | An positive integer field which value is 2 bytes long.                              |
| 6         | INT_POS_3_BYTES      | An positive integer field which value is 3 bytes long.                              |
| 7         | INT_POS_4_BYTES      | An positive integer field which value is 4 bytes long.                              |
| 8         | INT_POS_5_BYTES      | An positive integer field which value is 5 bytes long.                              |
| 9         | INT_POS_6_BYTES      | An positive integer field which value is 6 bytes long.                              |
| 10        | INT_POS_7_BYTES      | An positive integer field which value is 7 bytes long.                              |
| 11        | INT_POS_8_BYTES      | An positive integer field which value is 8 bytes long.                              |
| 12        | INT_NEG_1_BYTES      | An negative integer field which value is 1 byte long.                               |
| 13        | INT_NEG_2_BYTES      | An negative integer field which value is 2 bytes long.                              |
| 14        | INT_NEG_3_BYTES      | An negative integer field which value is 3 bytes long.                              |
| 15        | INT_NEG_4_BYTES      | An negative integer field which value is 4 bytes long.                              |
| 16        | INT_NEG_5_BYTES      | An negative integer field which value is 5 bytes long.                              |
| 17        | INT_NEG_6_BYTES      | An negative integer field which value is 6 bytes long.                              |
| 18        | INT_NEG_7_BYTES      | An negative integer field which value is 7 bytes long.                              |
| 19        | INT_NEG_8_BYTES      | An negative integer field which value is 8 bytes long.                              |
| 20        | FLOAT_NULL           | A 1 byte long field representing a float field with a null value.                   |
| 21        | FLOAT_4_BYTES        | A float field which value is 4 bytes long (32 bit float).                           |
| 22        | FLOAT_8_BYTES        | A float field which value is 8 bytes long (64 bit float).                           |
| 23        | BYTES_NULL           | A single byte field representing a Bytes field with the value null.                 |
| 24        | BYTES_0_BYTES        | A single byte field representing a Bytes field with a length of 0 bytes (empty).    |
| 25        | BYTES_1_BYTES        | A bytes field with a value that is 1 byte long.                                     |
| 26        | BYTES_2_BYTES        | A bytes field with a value that is 2 bytes long.                                    |
| 27        | BYTES_3_BYTES        | A bytes field with a value that is 3 bytes long.                                    |
| 28        | BYTES_4_BYTES        | A bytes field with a value that is 4 bytes long.                                    |
| 29        | BYTES_5_BYTES        | A bytes field with a value that is 5 bytes long.                                    |
| 30        | BYTES_6_BYTES        | A bytes field with a value that is 6 bytes long.                                    |
| 31        | BYTES_7_BYTES        | A bytes field with a value that is 7 bytes long.                                    |
| 32        | BYTES_8_BYTES        | A bytes field with a value that is 8 bytes long.                                    |
| 33        | BYTES_9_BYTES        | A bytes field with a value that is 9 bytes long.                                    |
| 34        | BYTES_10_BYTES       | A bytes field with a value that is 10 bytes long.                                   |
| 35        | BYTES_11BYTES        | A bytes field with a value that is 11 bytes long.                                   |
| 36        | BYTES_12_BYTES       | A bytes field with a value that is 12 bytes long.                                   |
| 37        | BYTES_13_BYTES       | A bytes field with a value that is 13 bytes long.                                   |
| 38        | BYTES_14_BYTES       | A bytes field with a value that is 14 bytes long.                                   |
| 39        | BYTES_15_BYTES       | A bytes field with a value that is 15 bytes long.                                   |
| 40        | BYTES_1_LENGTH_BYTES | A bytes field that uses 1 length byte to represent the length of the field value.   |
| 41        | BYTES_2_LENGTH_BYTES | A bytes field that uses 2 length byte to represent the length of the field value.   |
| 42        | BYTES_3_LENGTH_BYTES | A bytes field that uses 3 length byte to represent the length of the field value.   |
| 43        | BYTES_4_LENGTH_BYTES | A bytes field that uses 4 length byte to represent the length of the field value.   |
| 44        | BYTES_5_LENGTH_BYTES | A bytes field that uses 5 length byte to represent the length of the field value.   |
| 45        | BYTES_6_LENGTH_BYTES | A bytes field that uses 6 length byte to represent the length of the field value.   |
| 46        | BYTES_7_LENGTH_BYTES | A bytes field that uses 7 length byte to represent the length of the field value.   |
| 47        | BYTES_8_LENGTH_BYTES | A bytes field that uses 8 length byte to represent the length of the field value.   |
