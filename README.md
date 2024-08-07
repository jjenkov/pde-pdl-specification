# Polymorph Data Encoding (PDE) Specification + Polymorph Data Language (PDL) Specification 

Polymorph Data Encoding (PDE) is a binary data encoding that is capable of representing a wide variety of structured
data via a compact, fast to read and write binary encoding.

Polymorph Data Language (PDL) is a textual syntax for representing the same data as PDE can represent in binary form.
PDL enables you to convert a PDE file to PDL and view + edit in a text editor, and then convert it back to PDE 
afterward (if you need that). You can also create files directly in PDL and convert them to PDE later, if you need that.


# A Versatile, Flexible, Fast, Compact Alternative to CSV, XML, JSON, MessagePack, CBOR, Protobuf, ION, Avro etc. 
PDE + PDL are aiming at becoming the most expressive data representations available today.

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

| Data Type            | PDL + PDE | JSON  |
|----------------------|-----------|-------|
| Boolean              | +         | +     |
| Integer              | +         | +     |
| Float                | +         | +     |
| ASCII                | +         | + (*) |
| UTF-8                | +         | +     |
| UTC                  | +         |       |
| Bytes                | +         | (+)   |
| Object               | +         | +     |
| Table                | +         |       |
| Tree                 | +         | +     |
| Object Graphs        | +         | +     |
| Cyclic Object Graphs | +         |       |
| Metadata             | +         |       |
| Comments             | +         |       |

Notes:

JSON can contain ASCII characters because JSON can contain UTF-8 characters, and ASCII is a subset of UTF-8.
However, there is no way to signal to a JSON parser that the following field consists **only** of ASCII characters.
ASCII text is faster to parse than UTF-8, so for some use cases this can be beneficial.

PDL can represent trees more compactly than JSON, provided the children of a field are all the same type.
Thus, the children can be represented using a Table rather than via individual Object fields.












