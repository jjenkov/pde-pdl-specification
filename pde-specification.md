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

A PDE file (or stream) consists of a stream (sequence) of PDE fields. 

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


| Type Code | Field Name              | Field Description                                                                                                                          |
|-----------|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| 0         | BOOLEAN_NULL            | A single byte with the value 0 - representing a boolean field with a null value.                                                           |
| 1         | BOOLEAN_TRUE            | A single byte with the value 1 - representing a boolean field with the value true.                                                         |
| 2         | BOOLEAN_FALSE           | A single byte with the value 2 - representing a boolean field with the value false.                                                        |
| 3         | INT_NULL                | A single byte representing an Integer field with the value null.                                                                           | 
| 4         | INT_POS_1_BYTES         | An positive integer field which value is 1 byte long.                                                                                      |
| 5         | INT_POS_2_BYTES         | An positive integer field which value is 2 bytes long.                                                                                     |
| 6         | INT_POS_3_BYTES         | An positive integer field which value is 3 bytes long.                                                                                     |
| 7         | INT_POS_4_BYTES         | An positive integer field which value is 4 bytes long.                                                                                     |
| 8         | INT_POS_5_BYTES         | An positive integer field which value is 5 bytes long.                                                                                     |
| 9         | INT_POS_6_BYTES         | An positive integer field which value is 6 bytes long.                                                                                     |
| 10        | INT_POS_7_BYTES         | An positive integer field which value is 7 bytes long.                                                                                     |
| 11        | INT_POS_8_BYTES         | An positive integer field which value is 8 bytes long.                                                                                     |
| 12        | INT_NEG_1_BYTES         | An negative integer field which value is 1 byte long.                                                                                      |
| 13        | INT_NEG_2_BYTES         | An negative integer field which value is 2 bytes long.                                                                                     |
| 14        | INT_NEG_3_BYTES         | An negative integer field which value is 3 bytes long.                                                                                     |
| 15        | INT_NEG_4_BYTES         | An negative integer field which value is 4 bytes long.                                                                                     |
| 16        | INT_NEG_5_BYTES         | An negative integer field which value is 5 bytes long.                                                                                     |
| 17        | INT_NEG_6_BYTES         | An negative integer field which value is 6 bytes long.                                                                                     |
| 18        | INT_NEG_7_BYTES         | An negative integer field which value is 7 bytes long.                                                                                     |
| 19        | INT_NEG_8_BYTES         | An negative integer field which value is 8 bytes long.                                                                                     |
| 20        | FLOAT_NULL              | A 1 byte long field representing a float field with a null value.                                                                          |
| 21        | FLOAT_4_BYTES           | A float field which value is 4 bytes long (32 bit float).                                                                                  |
| 22        | FLOAT_8_BYTES           | A float field which value is 8 bytes long (64 bit float).                                                                                  |
| 23        | BYTES_NULL              | A single byte field representing a Bytes field with the value null.                                                                        |
| 24        | BYTES_0_BYTES           | A single byte field representing a Bytes field with a length of 0 bytes (empty).                                                           |
| 25        | BYTES_1_BYTES           | A bytes field with a value that is 1 byte long.                                                                                            |
| 26        | BYTES_2_BYTES           | A bytes field with a value that is 2 bytes long.                                                                                           |
| 27        | BYTES_3_BYTES           | A bytes field with a value that is 3 bytes long.                                                                                           |
| 28        | BYTES_4_BYTES           | A bytes field with a value that is 4 bytes long.                                                                                           |
| 29        | BYTES_5_BYTES           | A bytes field with a value that is 5 bytes long.                                                                                           |
| 30        | BYTES_6_BYTES           | A bytes field with a value that is 6 bytes long.                                                                                           |
| 31        | BYTES_7_BYTES           | A bytes field with a value that is 7 bytes long.                                                                                           |
| 32        | BYTES_8_BYTES           | A bytes field with a value that is 8 bytes long.                                                                                           |
| 33        | BYTES_9_BYTES           | A bytes field with a value that is 9 bytes long.                                                                                           |
| 34        | BYTES_10_BYTES          | A bytes field with a value that is 10 bytes long.                                                                                          |
| 35        | BYTES_11BYTES           | A bytes field with a value that is 11 bytes long.                                                                                          |
| 36        | BYTES_12_BYTES          | A bytes field with a value that is 12 bytes long.                                                                                          |
| 37        | BYTES_13_BYTES          | A bytes field with a value that is 13 bytes long.                                                                                          |
| 38        | BYTES_14_BYTES          | A bytes field with a value that is 14 bytes long.                                                                                          |
| 39        | BYTES_15_BYTES          | A bytes field with a value that is 15 bytes long.                                                                                          |
| 40        | BYTES_1_LENGTH_BYTES    | A bytes field that uses 1 length byte to represent the length of the field value.                                                          |
| 41        | BYTES_2_LENGTH_BYTES    | A bytes field that uses 2 length byte to represent the length of the field value.                                                          |
| 42        | BYTES_3_LENGTH_BYTES    | A bytes field that uses 3 length byte to represent the length of the field value.                                                          |
| 43        | BYTES_4_LENGTH_BYTES    | A bytes field that uses 4 length byte to represent the length of the field value.                                                          |
| 44        | BYTES_5_LENGTH_BYTES    | A bytes field that uses 5 length byte to represent the length of the field value.                                                          |
| 45        | BYTES_6_LENGTH_BYTES    | A bytes field that uses 6 length byte to represent the length of the field value.                                                          |
| 46        | BYTES_7_LENGTH_BYTES    | A bytes field that uses 7 length byte to represent the length of the field value.                                                          |
| 47        | BYTES_8_LENGTH_BYTES    | A bytes field that uses 8 length byte to represent the length of the field value.                                                          |
| 48        | ASCII_NULL              | A single byte field representing an ASCII field with the value null.                                                                       |
| 49        | ASCII_0_BYTES           | A single byte field representing an ASCII field with a length of 0 bytes (empty).                                                          |
| 50        | ASCII_1_BYTES           | An ASCII field with a value that is 1 byte long.                                                                                           |
| 51        | ASCII_2_BYTES           | An ASCII field with a value that is 2 bytes long.                                                                                          |
| 52        | ASCII_3_BYTES           | An ASCII field with a value that is 3 bytes long.                                                                                          |
| 53        | ASCII_4_BYTES           | An ASCII field with a value that is 4 bytes long.                                                                                          |
| 54        | ASCII_5_BYTES           | An ASCII field with a value that is 5 bytes long.                                                                                          |
| 55        | ASCII_6_BYTES           | An ASCII field with a value that is 6 bytes long.                                                                                          |
| 56        | ASCII_7_BYTES           | An ASCII field with a value that is 7 bytes long.                                                                                          |
| 57        | ASCII_8_BYTES           | An ASCII field with a value that is 8 bytes long.                                                                                          |
| 58        | ASCII_9_BYTES           | An ASCII field with a value that is 9 bytes long.                                                                                          |
| 59        | ASCII_10_BYTES          | An ASCII field with a value that is 10 bytes long.                                                                                         |
| 60        | ASCII_11BYTES           | An ASCII field with a value that is 11 bytes long.                                                                                         |
| 61        | ASCII_12_BYTES          | An ASCII field with a value that is 12 bytes long.                                                                                         |
| 62        | ASCII_13_BYTES          | An ASCII field with a value that is 13 bytes long.                                                                                         |
| 63        | ASCII_14_BYTES          | An ASCII field with a value that is 14 bytes long.                                                                                         |
| 64        | ASCII_15_BYTES          | An ASCII field with a value that is 15 bytes long.                                                                                         |
| 65        | ASCII_1_LENGTH_BYTES    | An ASCII field that uses 1 length byte to represent the length of the field value.                                                         |
| 66        | ASCII_2_LENGTH_BYTES    | An ASCII field that uses 2 length byte to represent the length of the field value.                                                         |
| 67        | ASCII_3_LENGTH_BYTES    | An ASCII field that uses 3 length byte to represent the length of the field value.                                                         |
| 68        | ASCII_4_LENGTH_BYTES    | An ASCII field that uses 4 length byte to represent the length of the field value.                                                         |
| 69        | ASCII_5_LENGTH_BYTES    | An ASCII field that uses 5 length byte to represent the length of the field value.                                                         |
| 70        | ASCII_6_LENGTH_BYTES    | An ASCII field that uses 6 length byte to represent the length of the field value.                                                         |
| 71        | ASCII_7_LENGTH_BYTES    | An ASCII field that uses 7 length byte to represent the length of the field value.                                                         |
| 72        | ASCII_8_LENGTH_BYTES    | An ASCII field that uses 8 length byte to represent the length of the field value.                                                         |
| 73        | UTF_8_NULL              | A single UTF-8 field representing a Bytes field with the value null.                                                                       |
| 74        | UTF_8_0_BYTES           | A single UTF-8 field representing a Bytes field with a length of 0 bytes (empty).                                                          |
| 75        | UTF_8_1_BYTES           | A UTF-8 field with a value that is 1 byte long.                                                                                            |
| 76        | UTF_8_2_BYTES           | A UTF-8 field with a value that is 2 bytes long.                                                                                           |
| 77        | UTF_8_3_BYTES           | A UTF-8 field with a value that is 3 bytes long.                                                                                           |
| 78        | UTF_8_4_BYTES           | A UTF-8 field with a value that is 4 bytes long.                                                                                           |
| 79        | UTF_8_5_BYTES           | A UTF-8 field with a value that is 5 bytes long.                                                                                           |
| 80        | UTF_8_6_BYTES           | A UTF-8 field with a value that is 6 bytes long.                                                                                           |
| 81        | UTF_8_7_BYTES           | A UTF-8 field with a value that is 7 bytes long.                                                                                           |
| 82        | UTF_8_8_BYTES           | A UTF-8 field with a value that is 8 bytes long.                                                                                           |
| 83        | UTF_8_9_BYTES           | A UTF-8 field with a value that is 9 bytes long.                                                                                           |
| 84        | UTF_8_10_BYTES          | A UTF-8 field with a value that is 10 bytes long.                                                                                          |
| 85        | UTF_8_11_BYTES          | A UTF-8 field with a value that is 11 bytes long.                                                                                          |
| 86        | UTF_8_12_BYTES          | A UTF-8 field with a value that is 12 bytes long.                                                                                          |
| 87        | UTF_8_13_BYTES          | A UTF-8 field with a value that is 13 bytes long.                                                                                          |
| 88        | UTF_8_14_BYTES          | A UTF-8 field with a value that is 14 bytes long.                                                                                          |
| 89        | UTF_8_15_BYTES          | A UTF-8 field with a value that is 15 bytes long.                                                                                          |
| 90        | UTF_8_1_LENGTH_BYTES    | A UTF-8 field that uses 1 length byte to represent the length of the field value.                                                          |
| 91        | UTF_8_2_LENGTH_BYTES    | A UTF-8 field that uses 2 length byte to represent the length of the field value.                                                          |
| 92        | UTF_8_3_LENGTH_BYTES    | A UTF-8 field that uses 3 length byte to represent the length of the field value.                                                          |
| 93        | UTF_8_4_LENGTH_BYTES    | A UTF-8 field that uses 4 length byte to represent the length of the field value.                                                          |
| 94        | UTF_8_5_LENGTH_BYTES    | A UTF-8 field that uses 5 length byte to represent the length of the field value.                                                          |
| 95        | UTF_8_6_LENGTH_BYTES    | A UTF-8 field that uses 6 length byte to represent the length of the field value.                                                          |
| 96        | UTF_8_7_LENGTH_BYTES    | A UTF-8 field that uses 7 length byte to represent the length of the field value.                                                          |
| 97        | UTF_8_8_LENGTH_BYTES    | A UTF-8 field that uses 8 length byte to represent the length of the field value.                                                          |
| 98        | UTC_NULL                | A UTC field with a null value.                                                                                                             |
| 99        | UTC_0_BYTES             | A UTC field with an empty value (not sure if this has an actual use case?).                                                                |
| 100       | UTC_2_BYTES             | A UTC field with only a year specified - in its 2 bytes.                                                                                   |
| 101       | UTC_3_BYTES             | A UTC field with a year and month specified - in its 3 bytes.                                                                              |
| 102       | UTC_4_BYTES             | A UTC field with a year, month and day specified - in its 4 bytes.                                                                         |
| 103       | UTC_5_BYTES             | A UTC field with a year, month, day and hour specified - in its 5 bytes.                                                                   |
| 104       | UTC_6_BYTES             | A UTC field with a year, month, day, hour and minutes specified - in its 6 bytes.                                                          |
| 105       | UTC_7_BYTES             | A UTC field with a year, month, day, hour, minute and seconds specified - in its 7 bytes.                                                  |
| 106       | UTC_8_BYTES             | A UTC field with a UTC timestamp in milliseconds in its 8 bytes.                                                                           |
| 107       | UTC_9_BYTES             | A UTC field with a year, month, day, hour, minutes, seconds + milliseconds specified - in its 9 bytes.                                     |
| 108       | UTC_10_BYTES            | A UTC field with a year, month, day, hour, minutes, seconds + nanoseconds specified - in its 10 bytes.                                     |
| 109       | COPY_1_BYTES            | A copy of another field found a relative number of bytes earlier in this PDE stream - using 1 byte to represent the relative offset.       |
| 110       | COPY_2_BYTES            | A copy of another field found a relative number of bytes earlier in this PDE stream - using 2 bytes to represent the relative offset.      |
| 111       | COPY_3_BYTES            | A copy of another field found a relative number of bytes earlier in this PDE stream - using 3 bytes to represent the relative offset.      |
| 112       | COPY_4_BYTES            | A copy of another field found a relative number of bytes earlier in this PDE stream - using 4 bytes to represent the relative offset.      |
| 113       | COPY_5_BYTES            | A copy of another field found a relative number of bytes earlier in this PDE stream - using 5 bytes to represent the relative offset.      |
| 114       | COPY_6_BYTES            | A copy of another field found a relative number of bytes earlier in this PDE stream - using 6 bytes to represent the relative offset.      |
| 115       | COPY_7_BYTES            | A copy of another field found a relative number of bytes earlier in this PDE stream - using 7 bytes to represent the relative offset.      |
| 116       | COPY_8_BYTES            | A copy of another field found a relative number of bytes earlier in this PDE stream - using 8 bytes to represent the relative offset.      |
| 117       | REFERENCE_1_BYTES       | A reference to another field found a relative number of bytes earlier in this PDE stream - using 1 byte to represent the relative offset.  |
| 118       | REFERENCE_2_BYTES       | A reference to another field found a relative number of bytes earlier in this PDE stream - using 2 bytes to represent the relative offset. |
| 119       | REFERENCE_3_BYTES       | A reference to another field found a relative number of bytes earlier in this PDE stream - using 3 bytes to represent the relative offset. |
| 120       | REFERENCE_4_BYTES       | A reference to another field found a relative number of bytes earlier in this PDE stream - using 4 bytes to represent the relative offset. |
| 121       | REFERENCE_5_BYTES       | A reference to another field found a relative number of bytes earlier in this PDE stream - using 5 bytes to represent the relative offset. |
| 122       | REFERENCE_6_BYTES       | A reference to another field found a relative number of bytes earlier in this PDE stream - using 6 bytes to represent the relative offset. |
| 123       | REFERENCE_7_BYTES       | A reference to another field found a relative number of bytes earlier in this PDE stream - using 7 bytes to represent the relative offset. |
| 124       | REFERENCE_8_BYTES       | A reference to another field found a relative number of bytes earlier in this PDE stream - using 8 bytes to represent the relative offset. |
| 125       | KEY_NULL                | A key field with a null value.                                                                                                             |
| 126       | KEY_0_BYTES             | A key field with an empty value.                                                                                                           |
| 127       | KEY_1_BYTES             | A key field with a 1 byte value.                                                                                                           |
| 128       | KEY_2_BYTES             | A key field with a 2 byte value.                                                                                                           |
| 129       | KEY_3_BYTES             | A key field with a 3 byte value.                                                                                                           |
| 130       | KEY_4_BYTES             | A key field with a 4 byte value.                                                                                                           |
| 131       | KEY_5_BYTES             | A key field with a 5 byte value.                                                                                                           |
| 132       | KEY_6_BYTES             | A key field with a 6 byte value.                                                                                                           |
| 133       | KEY_7_BYTES             | A key field with a 7 byte value.                                                                                                           |
| 134       | KEY_8_BYTES             | A key field with a 8 byte value.                                                                                                           |
| 135       | KEY_9_BYTES             | A key field with a 9 byte value.                                                                                                           |
| 136       | KEY_10_BYTES            | A key field with a 10 byte value.                                                                                                          |
| 137       | KEY_11_BYTES            | A key field with a 11 byte value.                                                                                                          |
| 138       | KEY_12_BYTES            | A key field with a 12 byte value.                                                                                                          |
| 139       | KEY_13_BYTES            | A key field with a 13 byte value.                                                                                                          |
| 140       | KEY_14_BYTES            | A key field with a 14 byte value.                                                                                                          |
| 141       | KEY_15_BYTES            | A key field with a 15 byte value.                                                                                                          |
| 142       | KEY_1_LENGTH_BYTES      | A key field with 1 length byte to represent the length of its value.                                                                       |
| 143       | KEY_2_LENGTH_BYTES      | A key field with 2 length bytes to represent the length of its value.                                                                      |
| 144       | OBJECT_NULL             | An object field with a null value.                                                                                                         |
| 145       | OBJECT_1_LENGTH_BYTES   | An object field with 1 length byte to represent the length of its value (nested fields).                                                   |
| 146       | OBJECT_2_LENGTH_BYTES   | An object field with 2 length byte to represent the length of its value (nested fields).                                                   |
| 147       | OBJECT_3_LENGTH_BYTES   | An object field with 3 length byte to represent the length of its value (nested fields).                                                   |
| 148       | OBJECT_4_LENGTH_BYTES   | An object field with 4 length byte to represent the length of its value (nested fields).                                                   |
| 149       | OBJECT_5_LENGTH_BYTES   | An object field with 5 length byte to represent the length of its value (nested fields).                                                   |
| 150       | OBJECT_6_LENGTH_BYTES   | An object field with 6 length byte to represent the length of its value (nested fields).                                                   |
| 151       | OBJECT_7_LENGTH_BYTES   | An object field with 7 length byte to represent the length of its value (nested fields).                                                   |
| 152       | OBJECT_8_LENGTH_BYTES   | An object field with 8 length byte to represent the length of its value (nested fields).                                                   |
| 153       | TABLE_NULL              | An table field with a null value.                                                                                                          |
| 154       | TABLE_1_LENGTH_BYTES    | An table field with 1 length byte to represent the length of its value (nested fields).                                                    |
| 155       | TABLE_2_LENGTH_BYTES    | An table field with 2 length byte to represent the length of its value (nested fields).                                                    |
| 156       | TABLE_3_LENGTH_BYTES    | An table field with 3 length byte to represent the length of its value (nested fields).                                                    |
| 157       | TABLE_4_LENGTH_BYTES    | An table field with 4 length byte to represent the length of its value (nested fields).                                                    |
| 158       | TABLE_5_LENGTH_BYTES    | An table field with 5 length byte to represent the length of its value (nested fields).                                                    |
| 159       | TABLE_6_LENGTH_BYTES    | An table field with 6 length byte to represent the length of its value (nested fields).                                                    |
| 160       | TABLE_7_LENGTH_BYTES    | An table field with 7 length byte to represent the length of its value (nested fields).                                                    |
| 161       | TABLE_8_LENGTH_BYTES    | An table field with 8 length byte to represent the length of its value (nested fields).                                                    |
| 232       | METADATA_NULL           | A metadata field with a null value.                                                                                                        |
| 233       | METADATA_1_LENGTH_BYTES | A metadata field using 1 byte to represent the length of its body (value => nested fields).                                                |
| 234       | METADATA_2_LENGTH_BYTES | A metadata field using 2 bytes to represent the length of its body (value => nested fields).                                               |
| 235       | METADATA_3_LENGTH_BYTES | A metadata field using 3 bytes to represent the length of its body (value => nested fields).                                               |
| 236       | METADATA_4_LENGTH_BYTES | A metadata field using 4 bytes to represent the length of its body (value => nested fields).                                               |
| 237       | METADATA_5_LENGTH_BYTES | A metadata field using 5 bytes to represent the length of its body (value => nested fields).                                               |
| 238       | METADATA_6_LENGTH_BYTES | A metadata field using 6 bytes to represent the length of its body (value => nested fields).                                               |
| 239       | METADATA_7_LENGTH_BYTES | A metadata field using 7 bytes to represent the length of its body (value => nested fields).                                               |
| 240       | METADATA_8_LENGTH_BYTES | A metadata field using 8 bytes to represent the length of its body (value => nested fields).                                               |
| 241       | EXTENSION_1_BYTE_1      | An extension field using 1 extra byte to contain the extended field type.                                                                  |
| 242       | EXTENSION_1_BYTE_2      | An extension field using 1 extra byte to contain the extended field type.                                                                  |
| 243       | EXTENSION_1_BYTE_3      | An extension field using 1 extra byte to contain the extended field type.                                                                  |
| 244       | EXTENSION_1_BYTE_4      | An extension field using 1 extra byte to contain the extended field type.                                                                  |
| 245       | EXTENSION_1_BYTE_5      | An extension field using 1 extra byte to contain the extended field type.                                                                  |
| 246       | EXTENSION_1_BYTE_6      | An extension field using 1 extra byte to contain the extended field type.                                                                  |
| 247       | EXTENSION_1_BYTE_7      | An extension field using 1 extra byte to contain the extended field type.                                                                  |
| 248       | EXTENSION_1_BYTE_8      | An extension field using 1 extra byte to contain the extended field type.                                                                  |
| 249       | EXTENSION_2_BYTES       | An extension field using 2 extra bytes to contain the extended field type.                                                                 |
| 250       | EXTENSION_3_BYTES       | An extension field using 3 extra bytes to contain the extended field type.                                                                 |
| 251       | EXTENSION_4_BYTES       | An extension field using 4 extra bytes to contain the extended field type.                                                                 |
| 252       | EXTENSION_5_BYTES       | An extension field using 5 extra bytes to contain the extended field type.                                                                 |
| 253       | EXTENSION_6_BYTES       | An extension field using 6 extra bytes to contain the extended field type.                                                                 |
| 254       | EXTENSION_7_BYTES       | An extension field using 7 extra bytes to contain the extended field type.                                                                 |
| 255       | EXTENSION_8_BYTES       | An extension field using 8 extra bytes to contain the extended field type.                                                                 |

