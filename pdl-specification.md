# Polymorph Data Language (PDL) Specification

Polymorph Data Language (PDL) is a textual data encoding that is capable of representing the same data types as
the Polymorph Data Encoding (PDE). You can convert from the binary PDE to PDL, and from PDL to PDE. 

See more about the purpose of PDE in the README file of this repository, here:

[README.md](README.md)

Polymorph Data Language can also be used by itself as an alternative to CSV, JSON, XML and YAML.
See more about how PDL compares to other data formats in the README file here:

[README.md](README.md)



## Polymorph Data Language Use Cases 

PDL is useful for debug purposes, where data in PDE can be converted to PDL to make the data easier to read.

PDL can also be useful for hand-editing smaller documents such as configuration files or smaller data sets.

You could also use PDL as an alternative to JSON, YAML, CSV or XML, if you prefer to stay with a fully textual data format.
PDL has more features than JSON, YAML, CSV and XML, so you would get more options with PDL than with any of these formats.


## Syntax Variations and Performance

When I started designing the syntax for PDL I started out with a syntax that was similar to the syntax of JSON.
However, as I started optimizing the tokenizer for this syntax, I realized that one of the biggest performance killers was
branch mis-predictions. Every time I either removed a branch, or made it more predictable, performance went up.
Of course there were other optimizations too - but the branch mis-predictions seemed to be the biggest issue.

Having realized that, I started looking into re-designing the PDL syntax to make it easier to parse. 
In other words, to make it parseable with fewer branches in the code. I explain this design process and the
resulting syntaxes in the following 


## Syntax Designs

So far I have experimented with 3 different syntax designs for PDL. I call them:

- POS0
- POS1
- POS2

POS is short for "Parser Optimized Syntax".


### Tokenizer Challenges

The 2 biggest challenges I have come across in tokenizer designs are:

 - Finding the end of a token.
 - Determining the type of the token.

Finding the beginning of a token is typically just scanning over any (if any) white spaces at the beginning
of the input text - or since the end of the previous token. The next non-white space character is the beginning
of the next token.

Finding the end of that token, however, is not always easy. Where a token ends depends on the syntax. In JSON,
a token can end at a white space, a colon (:), a comma (,) , a quote character, a bracket ({}) etc. This means,
that you will have to check each character found during tokenization of a token against more than one possible
end-of-token delimiting character - to determine if the given character is part of the token or not.

There are several ways to do this more or less cleverly. However, the smartest way I found was to make a small
decision about the syntax - which resulted in Parser Optimized Syntax 0:

- The first character of a token specifies the token type.
- All tokens always end with the same character (I chose semicolon (;) ).

Thus, a token could look like this:

+123;

The + tells that this is a positive integer token. The ; marks the end of that token.

I will get into a bit more detail about POS0, POS1 and POS2 in the following sections.


### Parser Optimized Syntax 0

As mentioned above, the first parser optimized syntax I experimented with is the syntax I call 0 - 
or Parser Optimized Syntax 0 (POS0).

In POS0 the following is true for all tokens in the language:

- The first character of a token specifies the token type.
- All tokens end with a semicolon character (;) .
- String tokens that would normally contain semicolon character within the token value - will need to escape the semicolon, so no semicolons occur anywhere else than as token end markers.








