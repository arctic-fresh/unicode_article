# What Is Unicode?

**Unicode** is the universal standard for encoding characters. It was suggested in 1991 by the Unicode Consortium. Today, it is the most commonly used encoding standard in the world. The widespread use of Unicode helped to solve the issues of the older encodings:

* Incorrect character decoding
* Insufficient number of supported characters
* Incompatibility between different encodings

Unicode 12.0 includes all modern and most historical scripts, graphic symbols, and even emoji.    

Unicode is supported by all modern operating systems, programming languages, libraries, and web browsers.


# General Structure

Each Unicode character is provided a unique **code point** – a hexadecimal number with a “U+” prefix:

> **U+0056 – V**<br> 
> **U+0411 - Б**   
> **U+4FE1 – 信**<br>
> **U+1F605 – 😅**

A code point must consist of at least four digits, so zeros are added before one-, two-, and three-digit numbers. 

The full range of code points is called the **codespace** and includes the values from 0 to 10FFFF. For convenience, the codespace has been divided into **17 planes**, each comprised of 65,000 code points.

**Plane 0, or the Basic Multilingual Plane (BMP): U+0000–​U+FFFF.** BMP contains characters from all modern scripts, as well as many historical characters. It also includes non-printing characters used for correct encoding. Almost any textual data consists primarily of characters from the BMP.

**Plane 1, or the Supplementary Multilingual Plane (SMP): U+10000–​U+1FFFF.** SMP contains characters which do not fit into the BMP or are rarely used: ancient scripts, some of the lesser-used modern scripts, and special-purpose invented scripts. 

**Plane 2, or the Supplementary Ideographic Plane (SIP): U+20000–​U+2FFFF.** SIP contains Chinese, Japanese, and Korean characters that were not included in the BMP – most of them are extremely rare characters of historical interest only.

**Planes 3 to 13: U+30000–​U+3FFFF.** These planes are unassigned and reserved for future use.                

**Plane 14, or the Supplementary Special-purpose Plane (SSP): U+E0000–​U+EFFFF.** SSP contains special non-printing characters that did not fit into the BMP – tags and additional variation selectors used for encoding some complex scripts.      

**Planes 15 and 16 – Private Use Plans: U+F0000–​U+10FFFF.** The third parties can use the code points on these planes to assign them to any characters. The decoding of such characters requires an agreement between two parties on their interpretation.


# Unicode Encodings

In Unicode, the code points (U+xxxx) can be represented in bytes in three different ways. The three encoding types are called UTF-8, UTF-16, and UTF-32. UTF stands for “Unicode Transformation Format”. The number in the name indicates the number of bits in the smallest code unit used for encoding a character. 

There used to be more UTF encodings, but they are no longer supported by the Unicode Standard.   

Below, you’ll find more information about the UTF-8 and UTF-16 encodings.

## UTF-8

### Description

In UTF-8, a single character is encoded using from 1 to 4 bytes (8, 16, 24, or 32 bits). The number of bytes in one character depends on a code point assigned to this character.

| Code points range | Number of bytes |
| ----------------- | --------------- |
| U+0000–U+007F     | 1 (8 bit)       |
| U+0080–U+07FF     | 2 (16 bit)      |
| U+0800–U+FFFF     | 3 (24 bit)      |
| U+10000–U+10FFFF  | 4 (32 bit)      |

The 128 characters in the range U+0000–U+007F are encoded using only 1 byte (8 bits). They correspond with ASCII – one of the oldest encoding standards which includes all basic Latin characters, numbers, punctuation marks, the most used symbols, and some control (non-printable) characters. In ASCII, a character also requires 1 byte. UTF-8 was specially designed to be ASCII-compatible, and this is the key advantage of this encoding: you can still work with ASCII-based legacy programs.  

The code points in the range U+0080–U+07FF represent additional Latin characters and most non-Latin characters used in modern languages, except for Asian characters: Cyrillic, Greek, Arabic, Armenian, Hebrew, and some other modern scripts. The characters from this range are encoded using 2 bytes. 

The code points in the range U+0800–U+FFFF mainly represent Chinese, Japanese, and Korean characters. They are encoded using 3 bytes.  

The code points in the range U+10000–U+10FFFF represent very rare characters which do not belong to the Basic Multilingual Plane – they are encoded using 4 bytes.

### Encoding Structure

It’s easy to identify 1-, 2-, 3-, and 4-byte UTF-8 characters in a row of code units (“x” represents the other bits in the code unit):

* In 1-byte characters, the only byte always starts with 0: **0**xxxxxxx 
* In 2-byte characters, the first byte in the sequence starts with 110: **110**xxxxx
* In 3-byte characters the first byte starts with 1110: **1110**xxxx
* In 4-byte characters the first byte starts with 11110: **11110**xxx

Also, if a character uses more than one byte, each following byte in the sequence starts with 10:

* In 2-byte characters: 110xxxxx **10**xxxxxx
* In 3-byte characters: 1110xxxx **10**xxxxxx **10**xxxxxx
* In 4-byte characters: 11110xxx **10**xxxxxx **10**xxxxxx **10**xxxxxx

As you can see, each UTF-8 character indicates itself, which has its advantages:

* An incomplete byte sequence will never be decoded, since the number of bytes in the sequence is always indicated by the prefix in the first byte.
* The first byte never starts with 10, so it can always be distinguished from the following bytes which always start with 10. If you run a search, it will never find the byte sequence for one character beginning in the middle of a byte sequence for another character.

### Advantages  

* Worldwide popularity: UTF-8 is the most used encoding nowadays
* Compatibility with ASCII: English text encoded in ASCII looks the same in UTF-8
* Saving space when working with Latin-based scripts: most common Latin characters require only a single byte     
* Easy decoding and search due to the smart encoding structure  

### Disadvantages

* Less space efficiency when working with Asian characters: in UTF-8, an Asian character requires 3 bytes, while in UTF&#8209;16 it requires only 2 bytes

## UTF-16

### Description

In UTF-16, a character is encoded using 2 or 4 bytes (code units consisting of 16 or 32 bits).

| Code points range            | Number of bytes |
| ---------------------------- | --------------- |
| U+0000–U+D7FF; U+E000–U+FFFF | 2 (16 bit)      |
| U+10000–U+10FFFF             | 4 (32 bit)      |

The ranges U+0000–U+D7FF and U+E000–U+FFFF represent all BMP characters, including Chinese, Japanese, and Korean characters – all of them are encoded using 2 bytes.    

The characters out of BMP (U+10000–U+10FFFF) are encoded using 4 bytes – two 16-bit code units called **surrogate pairs**. The first 16-bit code unit is called the **high surrogate**, and the second one is called the **low surrogate**. 

The code points in the range U+D800–U+DFFF are reserved for encoding surrogate pairs.

### Encoding Structure

Each character in the ranges U+0000–U+D7FF and U+E000–U+FFFF is encoded as one 16-bit code unit equal to the corresponding code point value. In the table below, you can see how Latin, Cyrillic, and Chinese characters are encoded in UTF-16 compared to the UTF-8 encoding (spaces between bytes were added for better readability):

<table class="tg">
  <tr>
    <th class="tg-cly1" colspan="2">g (U+0067)</th>
  </tr>
  <tr>
    <td class="tg-0lax">Number of bytes in UTF-16</td>
    <td class="tg-0lax">2</td>
  </tr>
  <tr>
    <td class="tg-0lax">Binary representation in UTF-16</td>
    <td class="tg-0lax">00000000 0<b>1100111</b></td>
  </tr>
  <tr>
    <td class="tg-0lax">Number of bytes in UTF-8</td>
    <td class="tg-0lax">1</td>
  </tr>
  <tr>
    <td class="tg-0lax">Binary representation in UTF-8</td>
    <td class="tg-0lax">01100111</td>
  </tr>
</table>

<table class="tg">
  <tr>
    <th class="tg-cly1" colspan="2">д (U+0434)</th>
  </tr>
  <tr>
    <td class="tg-0lax">Number of bytes in UTF-16</td>
    <td class="tg-0lax">2</td>
  </tr>
  <tr>
    <td class="tg-0lax">Binary representation in UTF-16</td>
    <td class="tg-0lax">00000<b>100 00110100</b></td>
  </tr>
  <tr>
    <td class="tg-0lax">Number of bytes in UTF-8</td>
    <td class="tg-0lax">2</td>
  </tr>
  <tr>
    <td class="tg-0lax">Binary representation in UTF-8</td>
    <td class="tg-0lax">11010000 10110100</td>
  </tr>
</table>

<table class="tg">
  <tr>
    <th class="tg-cly1" colspan="2">信 (U+4FE1)</th>
  </tr>
  <tr>
    <td class="tg-0lax">Number of bytes in UTF-16</td>
    <td class="tg-0lax">2</td>
  </tr>
  <tr>
    <td class="tg-0lax">Binary representation in UTF-16</td>
    <td class="tg-0lax">0<b>1001111 11100001</b></td>
  </tr>
  <tr>
    <td class="tg-0lax">Number of bytes in UTF-8</td>
    <td class="tg-0lax">3</td>
  </tr>
  <tr>
    <td class="tg-0lax">Binary representation in UTF-8</td>
    <td class="tg-0lax">11100100 10111111 10100001</td>
  </tr>
</table>

Encoding characters in the range U+10000–U+10FFFF is more complicated. To illustrate it, let’s encode the **emoji 😁 (U+1F601)**:

1. 10000<sub>16</sub> is subtracted from the code point, and the result is represented as a 20-bit number:

   > 1F601<sub>16</sub> - 10000<sub>16</sub> = F601<sub>16</sub> = 00001111011000000001<sub>2</sub>

   Now let’s split the resulting binary number into two parts:

   > The high 10 bits: **0000111101 (xxxxxxxxxx)**<br>
   > The low 10 bits: **1000000001 (yyyyyyyyyy)**

2. The high 10 bits are added to the binary value of U+D800 (the first code point from the reserved range) as follows:

   > 110110**xxxxxxxxxx** = 110110**0000111101**

   The resulting number is the first 16-bit unit (the high surrogate).
   
3. The low 10 bits are added to the binary value of U+DC00 (which is also the code point from the reserved range) as follows:
   
   > 110111**yyyyyyyyyy** = 110111**1000000001**

   Now we’ve got the second 16-bit unit (the low surrogate).
   
4. Together, the high and low surrogates represent the binary code for the character:

   > **1101100000111101 1101111000000001** (D83DDE01<sub>16</sub>)
   
For correct interpretation of the byte order, the **Byte Order Mark** (U+FEFF) is used at the beginning of a UTF-16 text. Alternatively, you can specify **UTF-16BE** (big-endian) or **UTF-16LE** (little-endian) as the encoding type – in this case, using the BOM is not required.

### Advantages

* Saving space when working with Asian characters: they require only 2 bytes in UTF-16 compared to 3 bytes in UTF-8

### Disadvantages

* Less space efficiency when working with Latin characters: they need 2 bytes in UTF-16 compared to 1 byte in UTF-8
* Incompatibility with ASCII


**You can learn more about the latest version of Unicode on the official website: <a href="http://www.unicode.org/versions/Unicode12.1.0/">http://www.unicode.org/versions/Unicode12.1.0/</a>.**




