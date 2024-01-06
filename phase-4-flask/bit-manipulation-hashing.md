# Bit Manipulation and Hashing
## ASCII
There are some situations where we might need to convert a letter or a string into a number. For example, when we work with binary numbers, we might need to turn a letter to its binary representation. Some algorithms also require converting letters to numbers. A key example is a hashing algorithm, which requires the ability to convert a string into an index in an array.

Fortunately, all characters - including letters, numbers and punctuation - can be translated into an ASCII character code, which is the standard for encoding characters on a keyboard as a number. You can see a complete ASCII chart [here](https://www.ascii-code.com/).

ASCII specifications serve a very specific purpose. ASCII is a universal language that computers can use to communicate and share files. Without standards like ASCII, one machine might send a message that reads `"hello"` - but a second machine wouldn't be able to translate it for a user because there'd be no standard by which to decode the message from the first machine.

Generally, we will not need to worry about how various machines communicate with each other - that's something that other developers have taken care of for us. However, as mentioned before, there are situations where we might need to turn letters into numbers. Instead of just creating our own converter, we'd be better off using a universal standard that all machines understand.

### Javascript Methods for ASCII Encoding
There are two important methods related to ASCII encoding. One is to turn a letter into its ASCII equivalent. The other is to turn an ASCII code back into its letter equivalent.

#### Turning a Letter Into ASCII Code
To turn a letter into its equivalent ASCII code, we can use the `String.prototype.charCodeAt()` method. This method takes a position in a string as an argument.

If we want to encode the first character in a string, we'd do the following:

```
> "cat".charCodeAt(0);
99
```

If we try to get the character code of a position that doesn't exist, the method will return `NaN`:

```
> "cat".charCodeAt(3);
NaN
```

If we want to return the codes for each character, we need to loop through the string. For example, we could do something like this:

```
function asciiConverter(string) {
  return string.split("").map(function(letter) {
    return letter.charCodeAt(0);
  });
}
```

This splits the string and then uses `Array.prototype.map()` to return the ASCII code for each letter. As we can see from the example above, we use `String.prototype.charCodeAt(0)` because we are looking at one letter at a time. It's common to just use `String.prototype.charCodeAt(0)` in this kind of loop because we are evaluating each letter individually - and the first position of a single letter is always going to be itself.

#### Turning an ASCII Code Into a Character
To go in the other direction, we can turn an ASCII code into a character with `String.fromCharCode()`. Note that this is a static method, not a prototype method.

We can call it like this:

```
> String.fromCharCode(97);
"a"
```

Or we can pass in multiple arguments:

```
> String.fromCharCode(99, 97, 116);
"cat"
```

This method won't just translate ASCII characters, by the way - it's for translating all [UTF-16](https://en.wikipedia.org/wiki/UTF-16/) characters. That means [Unicode](https://www.unicode.org/standard/WhatIsUnicode.html/) - which has a lot more characters than ASCII. For instance:

```
> String.fromCharCode(6543210);
"흪"
```

#### Notes
* The ASCII chart isn't just for numbers and letters. It's for encoding all characters on a keyboard. The first 32 characters in the ASCII chart are characters that can't be printed. This includes everything from the backspace to the Esc key.
* Lower-cased letters are encoded differently than upper-case letters. For instance, the ASCII code for A is 65 while the character code for a is 97. The letters are sequential so B is 66, C is 67, and so on. Because lower-case letters are also sequential, the lower-cased version of a letter will always have an ASCII code that's the sum of the ASCII code for the upper-cased version plus 32. Not that you need to know this fact - but it could help solve a problem some day if, for instance, you wanted to uppercase or lowercase letters based on their ASCII codes.
* There are also extended ASCII character codes for special characters such as characters used in other languages like `ë`.
* ASCII is based on a limited set of Latin characters. This means many characters from other languages (such as Arabic) are not included. Unicode is much more extensive for character encoding.


## Bits, Bytes and Binary Numbers
Computers use the binary number system, but what exactly does that mean? At this point, we should all recognize a number that's using the binary number system - it's just a series of zeroes and ones. For example, an uppercase A translates to `01000001` in binary code.

Humans generally use the decimal number system, though. The decimal number system is base 10. We have ten total digits: 0 through 9. When we are counting up, we hit the end of these digits when we reach 9. To get to the next number, we add an extra digit to the beginning and start over. That gives us the number 10. The same pattern happens when we switch from 99 to 100, 999 to 1000, and so on.

The binary number system is base 2, which means it only uses two digits: 0 and 1. When we get to the number 1, we have to add a new digit and start over.

Let's take a look:

```
<!-- We start at 0. -->

0
1

<!-- We've reached the point where we need to roll over and add a digit. -->

10
11

<!-- Time to roll over again and add a digit. -->

100
101
110
111

<!-- As we can see, we are gradually increasing the amount of numbers we can count up to before rolling over. -->

1000
1001
1010
1011
1100

1101
1111

<!-- These are the first fifteen numbers in the binary system. See if you can do the next five. -->
```

Why does a computer use binary language? Well, at the very deepest level, a computer's processing power consists of billions of transistors that have only two states: on and off. The 1 represents the on state while the 0 represents the off state. 

Each 0 or 1 in a sequence of binary code is a **bit**. A bit by itself isn't very helpful. In general, for each bit we add to a sequence, we have double the number of total possible permutations:
* **1 bit**: 2 permutations
* **2 bit**: 4 permutations
* **3 bit**: 8 permutations
* **4 bit**: 16 permutations
* **5 bit**: 32 permutations
* **6 bit**: 64 permutations
* **7 bit**: 128 permutations
* **8 bit**: 256 permutations

A group of eight bits put together is known as a **byte**. A byte consists of 256 different combinations if you include the number 00000000 - all the binary numbers between 00000000 and 11111111.

A single byte is an excellent option for storing characters. It's no coincidence that there are 255 characters represented in the extended set of ASCII character codes. 

Of course, processing power is much more advanced now. We now tend to measure things in terms of megabytes (say, a music file) and gigabytes (the storage space in our hard drive). Sometimes we might even talk in terms of terabytes. Here's how they all correspond to each other:
* **Byte**: 8 bits
* **Kilobyte**: 1024 bits (approximately 1000 bytes)
* **Megabyte**: 1024 kilobytes (approximately one million bytes)
* **Gigabyte**: 1024 megabytes (approximately one billion bytes)
* **Terabyte**: 1024 gigabytes (approximately one trillion bytes)

Now that we know the basics of bits and bytes and have also covered the basics of the binary number system, let's look at some methods we can use with binary numbers. This will allow us to explore use cases for bitwise manipulation in the next lesson.

## Converting Decimal to Binary
We can also convert any number into its binary equivalent with the `Number.prototype.toString()` method. This method turns a number to its string equivalent - but we can also specify an optional base as an argument. Because the binary number system is base 2, we can convert a decimal number to binary like this:

```
> (34).toString(2);
"100010"
```

Note that we have to put parentheses around the number or the parser will get confused and throw the following error: `Uncaught SyntaxError: Invalid or unexpected token`.

In the next lesson, we'll learn more about bit manipulation, including JavaScript operators for manipulating bits. In the process, we'll learn about some use cases for bit manipulation in the real world.


## Manipulating Bits
In the last lesson, we learned about bits - the smallest units of binary code that our machines use to operate. In this lesson, we'll learn how to work with bits, including comparing and manipulating them.

Why learn about bit manipulation? While working with binary code might be necessary on the lowest level of the machine, why would we use bits in higher-level languages? Well, it turns out that bits are very useful for a number of different things:
* **Hashing algorithms**: These algorithms are used to convert a hash into an array and back. They are an essential part of hash tables, which we will cover in a future lesson, and hashing algorithms often use bitwise manipulation. Hashing algorithms are one-way programs, so the text can’t be unscrambled and decoded by anyone else. And that’s the point. Hashing protects data at rest, so even if someone gains access to your server, the items stored there remain unreadable. Eg MD5,SHA-256
* **Encryption**: Encryption processes use hashing algorithms as well. We can use bitwise manipulation to improve our encryption and make it exceptionally difficult to crack passwords.
* **Compression**: Bitwise manipulation is often used for compression algorithms. An example is the widely used [zlib](https://www.euccas.me/zlib/) library, which is implemented with Node, Ruby, C#, and many other languages.
* **Storing boolean values**: We can store multiple boolean values with a minimum amount of data. This is useful for determining read/write permissions and also has other potential applications which we will cover later.

Now that we can see some of the potential use cases for bit manipulation, let's take a look at the bitwise operators we can use in JavaScript. Note that almost all of these bitwise operators are exactly the same in Ruby and C# as well.

| Operator | Name        | Description                                                                                                 |
|-|-|-
| &       | AND         | Sets each bit to 1 if both bits are 1                                                                       |  
| \|      | OR          | Sets each bit to 1 if one of two bits is 1                                                                  |
| ^       | XOR         | Sets each bit to 1 if only one of two bits is 1                                                             |
| ~       | NOT         | Inverts all the bits                                                                                        |
| <<      | Zero fill left shift  | Shifts left by pushing zeros in from the right and let the leftmost bits fall off                            |
| >>      | Signed right shift | Shifts right by pushing copies of the leftmost bit in from the left, and let the rightmost bits fall off     | 
| >>>     | Zero fill right shift | Shifts right by pushing zeros in from the left, and let the rightmost bits fall off                          |

### Examples

-----------------------------------------------------------

**Operation**      | **Result** | **Same As** | **Result**
-|-|-|-
5 & 1             | 1 | 0101 & 0001     | 0001
5 \| 1            | 5 | 0101 \| 0001   | 0101  
~ 5                | 10 | ~ 0101       | 1010
5 << 1            | 10 | 0101 << 1   | 1010 
5 ^ 1            | 4 | 0101 ^ 0001    | 0100
5 >> 1            | 2 | 0101 >> 1   | 0010
5 >>> 1           | 2 | 0101 >>> 1  | 0010 

JavaScript stores numbers as 64 bits floating point numbers, but all bitwise operations are performed on 32 bits binary numbers. Before a bitwise operation is performed, JavaScript converts numbers to 32 bits signed integers. After the bitwise operation is performed, the result is converted back to 64 bits JavaScript numbers.

In the next lesson, we'll have an opportunity to practice solving some problems using bitwise manipulation.

### Practice
Now it's time to practice using bitwise manipulation. The problems below are sorted from easiest to hardest. See if you can try solving them with a whiteboard. If not, solving them in VS Code is fine, too.

1. #### Odd or Even Number
    Write a function that looks at a number's binary representation to determine if it is even or odd.

    **Hint**: Start by writing out 1 to 10 in binary (or even higher if you want to practice). Look for a pattern you can use to determine whether a number is even or odd.

2. #### True Love
    Paris, a nonbinary princex, is searching all the kingdoms of the land to find another nonbinary princex to marry. To find the right match, Paris's advisors have used the [Myers-Briggs test](https://www.psychologytoday.com/us/basics/myers-briggs) to try to find the best fit. 

    (**Note**: We are not advocating for the Myers-Briggs test in any way - it just lends itself nicely to being written as binary code.) This test looks at four qualities:
    * Extraversion versus Information
    * Sensing versus Intuition
    * Thinking versus Feeling
    * Judgment versus Perception

    Here's the princex list so far (including Paris):

    ```
    const princexList = {
        Paris: {
            eVsI: "E",
            sVsI: "S",
            tVsF: "F",
            jVsP: "P"
        },
        Pat: {
            eVsI: "I",
            sVsI: "S",
            tVsF: "T",
            jVsP: "P"
        },
        Pau: {
            eVsI: "E",
            sVsI: "S",
            tVsF: "T",
            jVsP: "J"
        },
        Pearl: {
            eVsI: "E",
            sVsI: "I",
            tVsF: "T",
            jVsP: "P"
        },
        Puck: {
            eVsI: "I",
            sVsI: "S",
            tVsF: "T",
            jVsP: "J"
        },
        Pluto: {
            eVsI: "E",
            sVsI: "S",
            tVsF: "T",
            jVsP: "P"
        },
        Parker: {
            eVsI: "I",
            sVsI: "S",
            tVsF: "T",
            jVsP: "J"
        }
    }
    ```
    That's a lot of data that could be stored much more efficiently - especially since Paris has recently decided to expand their search to all the people in the kingdom - not just people from the princex list above. Since we now have potentially millions of people to search through, our first task is to store all the information about each person in the kingdom into a single binary number instead of a basic object.

    Your first task is to write a function that converts the results of each person's Myers-Briggs test into a series of zeroes and ones.

    For instance, this:
    ```
    Paris: {
      eVsI: "E",
      sVsI: "S",
      tVsF: "F",
      jVsP: "P"
    }
    ```
    Could be converted into this:

    ```
    ["Paris", 1100]
    ```
    This assumes that the first option (extraversion, sensing, thinking, judgment) of each binary Myers-Brigg quality is translated to a 1 while the second option (information, intuition, feeling, perception) is translated to 0.

    Next, translate `princexList` list into an array of arrays called `princexArray`. Each nested array should contain a key (the name of the person) and a value (the results of their Myers-Briggs test in binary code).

    So how much less memory does this `princexArray` take than `princexList`? We can use an NPM library called [object-sizeof](https://www.npmjs.com/package/object-sizeof) to find out. Don't worry, you don't need to use this library in your own code unless you are curious to compare the size of various objects.

    We can use this library to calculate the following:
    * `princexList` (our basic object) uses 342 bytes.
    * `princexArray` (our array) uses 126 bytes.

    Note that these savings are mostly due to using an array instead of an object, not because we are using a binary number. Both 1100 and `"ESFP"` use 8 bytes in JavaScript. However, let's say we wanted to compare 16 values instead of 4. A 16-digit number that uses zeroes and ones still takes 8 bytes - while a 16-digit string takes 32 bytes. The savings become more apparent the more boolean values there are to compare. This may not seem like much difference but in very large datasets, it becomes more significant.

    Next, write an algorithm that uses bitwise operators to determine whether a person is a good match for Paris. If three or more values on the Myers-Briggs test are the same, they should be a good match. If all four values are the same, they should be a great match. How you sort this information is up to you.

3. #### Encrypter
    Next, write a function that encrypts a five-letter string using bitwise manipulation. You will need to use methods that convert letters to numbers and then back to do so. See the [ASCII lesson](https://github.com/kev065/Moringa-CS-Notes/blob/main/phase-4-flask/bit-manipulation-hashing.md#ascii) for a refresher. You may come up with your own implementation, or you may try the one below.

    The encrypter should do the following:
    * It should do a binary left shift of two for each letter.
    * Next, it should switch every bit in each binary number to its opposite.
    * Next, it should do another left shift for each letter, this one of three.
    * At this point, there should be encrypted binary representations of all five letters. Merge these into one long binary string and then convert it to the decimal number system. The method should return this number.

    Next, try writing a method to decrypt the number back to its original string.