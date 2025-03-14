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

Using these three rules, I came up with the following syntax for POS0:

    # single-line comment;
    
    !; !0; !1;
    +123;
    %123.45;
    /123.45;
    "Hello world;
    @2030-12-31T23:59:59.999;
    
    {;.f1; +123; .f2; "val2;};
    {;+123; "val2;};
    
    [;.c1; .c2; .c3;
     +123; "val2; @2023;
     +456; "val5; @2024; ];
    
    {; .name; "Aya;
      .children; [;
        .name; .children;
        "Gretchen; [;
          .name; .children;
          "Rami; [;];
          "Fana; [;];
        ];
        "Hansel; [;
          .name; .children;
          "Gordia; [;];
          "Victor; [;];
        ];
      ];
    };
    
    *id;(;+0;);
    {; .name; "Parent;
      .child; {;
        .parent; *ref;(;+0;);
      };
    };
    
    *id;(;+0;);
    {; .name; "mother;
      .parent; o;
      .children; [;
        .name;     .parent;  .children;
        "child1;  *ref;(;0;);   [;];
        "child2;  *ref;(;0;);   [;];
      ];
    };


As you can see, it is in some ways similar to JSON - but with a few differences. First of all, the first token type
character is different. But - also the fact that all tokens are at least 2 characters long is a difference. Thus,
objects and arrays are demarcated like this:

    {;
    };

    [;
    ];

This is not a super beautiful syntax, nor as compact as it could be, but it has the advantage of being very easy and
fast to tokenize. This is all the code needed tokenize the above script:

    public void tokenizeMinified(Utf8Buffer buffer, TokenizerListener listener){
        buffer.skipWhiteSpace();

        while(buffer.hasMoreBytes()){
            int tokenStartOffset = buffer.tempOffset;
            buffer.tempOffset++;
     
            while(buffer.hasMoreBytes()){
                if(buffer.nextCodepointAscii() == ';'){
                    break;
                }
            }
     
            listener.token(tokenStartOffset, buffer.tempOffset, buffer.buffer[tokenStartOffset]);
            buffer.skipWhiteSpace();
        }
    }

This is only around 16 lines of code ! 

Not only is this code easy to write - it also executes quite fast! Since there are not that many branches in this
code, and the branches that are - are reasonably predictable especially for longer tokens - the execution speed of
this tokenizer is pretty good. 



### Parser Optimized Syntax 1

The second parser optimized syntax I experimented with for PDL is what I call Parser Optimized Syntax 1 (POS1).

The purpose of the POS1 syntax variation was to get object and array demarcations down to one character, like this:

    {
    }

    [
    ]

Additionally, the named token syntax should also be abbreviated from

    *id;(;+0;);
    *ref;(;0;);

to

    *id(+0;)
    *ref(+0;)

Notice how the ( character is now used as the named token end marker - e.g. to mark the end of the *id(  token .
Also notice how the ) character is now a single character token.

This syntax variation looks more like what we are used to from JSON and JavaScript etc. 

To tokenize this syntax variation reasonably easy and fast, I had to introduce a lookup table. 
I use the token type character to lookup what character is used to mark the end of that token.

Here is what the code looks like to tokenize the above syntax variation:

    public class PdlPfv2Tokenizer {
    
        private static final byte[] tokenEndCharacters = new byte[128];
    
        {
            for(int i=0; i<tokenEndCharacters.length; i++) {
                tokenEndCharacters[i] = ';';
            }
            tokenEndCharacters['{'] = '{';
            tokenEndCharacters['}'] = '}';
            tokenEndCharacters['['] = '[';
            tokenEndCharacters[']'] = ']';
            tokenEndCharacters['<'] = '<';
            tokenEndCharacters['>'] = '>';
            tokenEndCharacters['('] = '(';
            tokenEndCharacters[')'] = ')';
            tokenEndCharacters['*'] = '(';  //this is new compared to the PdlPfvTokenizer
    
        }
    
        public void tokenize(Utf8Buffer buffer, TokenizerListener listener) {
            TokenIndex64Bit tokenIndex = (TokenIndex64Bit) listener;
    
            buffer.skipWhiteSpace();
            while(buffer.hasMoreBytes()){
                int tokenStartOffset = buffer.tempOffset;
                int endCharacter = tokenEndCharacters[buffer.buffer[tokenStartOffset]];
    
                while(buffer.hasMoreBytes()){
                    if(buffer.nextCodepointAscii() == endCharacter){
                        break;
                    }
                }
    
                listener.token(tokenStartOffset, buffer.tempOffset, buffer.buffer[tokenStartOffset]);
                buffer.skipWhiteSpace();
            }
        }
    }

Notice how the token type character is now also examined to see if it is the token end marker character. 
This means that for each token - one more character is checked. 

The token type character double check is only a performance penalty for tokens that actually both have a token type character 
and a separate end marker character. For these kinds of tokens the token type character gets checked 2 times and the
token end marker character gets checked 1 time. That is 3 boundary checks per token.

For the tokens where the token type character is also the token end marker character - 2 characters checks are performed
(against the same character) just as if that token had consisted of 2 separate characters.
That is only 2 boundary checks per token.

This syntax is a bit slower to tokenize than POS0 - but the syntax is a bit easier to read and write.
However, since some of the tokens require less characters to represent - you gain a little speed from 
having fewer bytes to store, load or transport. Even with the saved characters this tokenizer is slower
than the POS0 tokenizer.


### Parser Optimized Syntax 2

The third syntax variation I am currently experimenting with - I call Parser Optimized Syntax 2 (POS2).

The purpose of the POS2 syntax variation is to get rid of the * type character in front of named tokens.
Thus, the named token

    *ref(+0;)

would become

    ref(+0;)

Additionally, this syntax variation could be used to get rid of the + type character in front of positive integers,
so the above named token parameter would be encoded like this:

    ref(0;)

This is one step closer to what we are used to from programming languages.

To be able to tokenize this syntax and still determine the token type reasonably fast, one more
lookup table lookup would need to be added. The real type of a token would have to be looked up
by taking the first character in the token and looking up its actual type in a lookup table. 

For tokens that still has a token type character in the beginning - the value stored in the lookup
table will simply be the same as the first character of the token. Thus, for such tokens the lookup
is actually wasted.

For tokens that do not have a token type character in the beginning (named tokens and positive integer tokens) - 
the value stored in the lookup table will be different from the first character of the token. This lookup will
thus not be wasted.

The lookup table will look something like this (not finalized):

    byte[] tokenTypes = new byte[128];
    
    tokenTypes['a'] = '*';
    ...
    tokenTypes['z'] = '*';

    tokenTypes['A'] = '*';
    ...
    tokenTypes['Z'] = '*';

    tokenTypes['0'] = '+';
    ...
    tokenTypes['9'] = '+';

Looking up the first character of the named token "ref(" will result in a lookup with the character 'r' as index.
This will return the value '*' for that token type. 

Similarly, looking up the first character of the positive integer token "678" will result in a lookup with the character
'r' as index. This will return the value '+' for that token type.

The lookup code will look something like this (not finalized):

    int endCharacter = tokenEndCharacters[buffer.buffer[tokenStartOffset]];
    int tokenType    = tokenTypes[buffer.buffer[tokenStartOffset]];

This results in one extra lookup per token compared to the POS1 syntax - but saves the * and + characters in front
of named tokens and positive integer tokens.

Alternatively, the endCharacter and tokenType lookup tables could be merged into one big table. Instead of being
2 tables of bytes - it could be one table of shorts (2 bytes). As you can see, the first character of a token is
used as key in both tables - so the 2 lookups of 1 byte could be reduced to 1 lookup of 2 bytes. First byte (lsb)
could contain the token type - and the second byte (msb) could contain the end character.

With a merged lookup table the lookup code would something like this:

    int endCharacter = typesAndEndCharacters[buffer.buffer[tokenStartOffset]];
    int tokenType = endCharacter;

    tokenType = 0xFF & tokenType;      //remove msb containing end character
    endCharacter = endCharacter >> 8;  // remove lsb containing token type
    
Possibly, in assembly language you could move the endCharacter lookup value into a CPU register, and then only move
the lsb from that register into another register that would now contain the token type (only the lsb). That might be
slightly faster (1 instruction less) than first copying the whole endCharacter register (with the combined value)
to another register, and then AND the msb byte out.

As mentioned, the POS2 syntax variation is not finalized and has not been tested (implemented) nor benchmarked.
More info will be added when this work has been carried out.



