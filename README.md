# Polymorph Data Encoding (PDE) Specification + Polymorph Data Language (PDL) Specification 

Polymorph Data Encoding (PDE) is a binary data encoding that is capable of representing a wide variety of structured
data via a compact, fast to read and write binary encoding.

Polymorph Data Language (PDL) is a textual syntax for representing the same data as PDE can represent in binary form.
PDL enables you to convert a PDE file to PDL and view + edit in a text editor, and then convert it back to PDE 
afterward (if you need that). You can also create files directly in PDL and convert them to PDE later, if you need that.

The Polymorph Data Encoding (PDE) specification can be found here:  [pde-specification.md](pde-specification.md)

The Polymorph Data Language (PDL) specification can be found here:  [pdl-specification.md](pdl-specification.md)


## PDE + PDL Implementations

I have started an implementation of PDE (and later PDL) in Java in this GitHub repository:

[Polymorph Fabric - Java](https://github.com/jjenkov/polymorph-fabric-java)


## A Versatile, Flexible, Fast, Compact Data Format
PDE + PDL is designed to be an expressive, flexible, fast, compact alternative to many of the otherwise commonly used data formats.

PDE + PDL is intended both as a 

- Message format in network communication (e.g. API calls) 
- Storage formats for storing data (e.g. application logs or event logs).
- Transport and synchronization formats via ordered streams of records.

PDE is a viable alternative to MessagePack, CBOR, Protobuf, ION (Amazon), Avro and other formats.

PDL is a viable alternative to CSV, JSON, XML and YAML.

PDE + PDL does not require a schema. Both formats are self-describing enough to enable reading and writing of these
formats. It would technically be possible to design a schema mechanism for PDE + PDL, but that is not a goal in 
the short term. Maybe sometime in the future.


PDE + PDL can efficiently represent the following data types and structures:

- Boolean
- Integers
- Floats
- ASCII Text (In Progress)
- UTF-8 Text
- UTC Date + Time
- Bytes
- Objects (or Maps / Dictionaries)
- Tables (or Arrays)
- Trees  (Via Tables Nested Inside Tables)
- Object Graphs (Meaning Nested Objects or Tables Inside Objects)
- Cyclic Object Graphs
- Metadata (In Progress)
- Comments (PDL Only So Far)

Here is how that compares to some of the data formats listed above:

| Data Type            | PDL + PDE | CSV | JSON  | MessagePack | CBOR | Protobuf |
|----------------------|-----------|-----|-------|-------------|------|----------|
| Boolean              | +         | +   | +     | +           | +    | +        |
| Integer              | +         | +   | +     | +           | +    | +        |
| Float                | +         | +   | +     | +           | +    | +        | 
| ASCII                | +         | +   | + (*) | +           | +    | +        |
| UTF-8                | +         | +   | +     | +           | +    | +        |
| UTC                  | +         | (+) |       | +           | +    | +        |
| Bytes                | + (*)     | (+) | (+)   | +           | +    | +        |
| Object               | +         |     | +     | +           | +    | +        |
| Array                | +         |     | +     | +           | +    | +        |
| Table                | +         | +   |       |             |      |          |
| Tree                 | +         |     | + (*) | +           | +    | +        |
| Object Graphs        | +         | +   | +     | +           | +    | +        |
| Cyclic Object Graphs | +         |     |       |             |      |          |
| Metadata             | +         |     |       |             |      |          |
| Comments             | +         |     |       |             |      |          |

Notes:

JSON can contain ASCII characters because JSON can contain UTF-8 characters, and ASCII is a subset of UTF-8.
However, there is no way to signal to a JSON parser that the following field consists **only** of ASCII characters.
ASCII text is faster to parse than UTF-8, so for some use cases this can be beneficial.

There is no official date format for UTC / ISO dates in CSV, but it is possible to define your own.
PDL comes with a built-in representation.

It is possible to represent raw bytes in both CSV and JSON via e.g. Hexadecimal or Base64 encoded strings,
but you will have to know to interpret these strings as bytes and decode them yourself.
PDL comes with built-in support for both Hexadecimal and Base64 encodings, so you know that these tokens
represents raw bytes.

PDL can represent trees more compactly than JSON, provided the children of a field are all the same type.
Thus, the children can be represented using a Table rather than via individual Object fields.

PDE + PDL can represent tabular data efficiently. This is not easy / possible in other data formats.

PDE + PDL can represent cyclic object graphs. This is not possible in most other data formats.


## Streams of Records (Fields)

A PDE or PDL file consists of one or more PDE / PDL fields (records) in a sequential order, just like
records in an application log, event log, Kafka topic, message queue etc.

Each PDE / PDL field has an offset via which you can reference that specific field (record).

The first PDE / PDL field has the offset 0. The second field has the offset 1 etc.

Only fields at the root level in the file / stream have a stream offset. Fields that are nested within other fields
do not get a stream offset.

The field offsets can be used to only read fields from a given offset and forward. This can be useful
if you are reprocessing a file / stream, and you have already processed the first N records earlier.













