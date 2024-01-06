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


## Hash Tables
Hash tables are one of the most important data structures you can possibly learn. They are used to efficiently store key-value pairs and are designed for super-fast lookup, insertion, and removal. The key underlying principle of a hash table is a hashing algorithm which converts keys into an index in an array. We'll discuss this more in a moment. Hashing algorithms are also an essential feature of encryption, allowing us to ensure passwords and other sensitive data remain secure.

In this lesson, we'll learn how to build a basic hash table from scratch. To do so, we'll also create a very mediocre hashing algorithm. This lesson is focused more on understanding the basics of how a hash table works - not the intricacies of hashing algorithms. Our mediocre hashing algorithm is meant to serve as a teaching tool that simplifies how hash tables and hashing algorithms work. We will take a look at ways to write an efficient hashing algorithm in the next lesson.

### Hashes
A hash is a set of key-value pairs with extremely fast lookup (if the hash is well-implemented). We simply need to get a key from a hash to quickly retrieve the value. This should sound familiar because we have been working with hashes a lot.

In JavaScript, we use basic objects and maps as a hash. For instance, here's a basic object where we can retrieve values based on keys:

```
let hash = {
  "John": "Lead vocalist",
  "Paul": "Bass guitarist",
  "George": "Lead guitarist",
  "Ringo": "Drummer"
}
```
To get the value associated with a key, we simply need to do this:

```
> hash["john"]
"Lead guitarist"
```
We can easily set a value as well:

```
hash["ted"] = "Not a Beatle!"
```
While a basic object is the easiest implementation of a hash in JavaScript, maps are even better:

```
let map = new Map(["John", "Lead vocalist"], ["Paul", "Bass guitarist"], ["George", "Lead guitarist"], ["Ringo", "Drummer"])
```
We can get and set values from a map like this:

```
map.get("John");
map.set("Ted", "Not a Beatle!");
```

Ruby and C#  also work with hashes regularly. In the case of Ruby, the data structure is called a hash. In the case of C#, the data structure is called a dictionary. However, they are doing the same thing that a JavaScript map does.

Because we are working with high-level languages, it's easy to just take advantage of all the benefits of hash tables without really thinking about how they work. However, hashes aren't an innate feature of programming languages. They are actually built using arrays. Remember that looking up an index in an array has a Big O of O(1) constant time. That is very fast. For that reason, the goal of a hash table is to get as close to that speed as possible.

Let's look at the hash above again:

```
let hash = {
  "John": "Lead vocalist",
  "Paul": "Bass guitarist",
  "George": "Lead guitarist",
  "Ringo": "Drummer"
}
```

How can we turn that into an array with extremely fast lookup times? Well, the only way we can do that is to associate keys with specific indexes. To do that, we need to write a special hashing algorithm that translates a key to an array index and back. In other words, we need a function that determines what number the key `"John"` should equal.

We're going to use object-oriented programming and a test-driven approach to build a `HashTable` class. This class will include methods to get, set, and remove key-value pairs from a hash table. It will use an array under the hood and the methods will be backed by a hashing algorithm.

### Instantiating a Hash Table
As we just discussed, hashes aren't innate to programming languages. They use arrays under the hood. That means when we instantiate a new hash table, it will need to use an array. Let's start with a test:

```
import HashTable from '../src/hash-table.js';

describe('HashTable', () => {

  let hashTable = new HashTable();

  afterEach(() => {
    hashTable = new HashTable();
  });

  test('should instantiate a hash with an empty array', () => {
    expect(hashTable.array).toEqual([]);
  });
});
```

A few things about this code:
* We create an instance of the `HashTable` class outside our test and include an `afterEach()` block as well. This will keep our tests DRY as we go.

The test itself is simple. We should expect the `hashTable` variable (which stores an instance of HashTable) to start with an empty array.

Here's the code to get this passing:

```
export default class HashTable {
  constructor() {
    this.array = [];
  }
}
```

All of our key-value pairs will be stored in this array. But how will we store them so we can quickly insert and retrieve values?

If we just pushed key-value pairs to the array, that wouldn't be very efficient. Insertion at the end of an array would be super-fast (pushing to the end of an array is O(1)) but we'd need a linear search (O(N)) to find key-value pairs. In a very large data set, it would take quite some time to grab key-values near the end of the data set. Meanwhile, insertion anywhere else in an array is quite slow (O(N)) because, in addition to inserting the new element, the index of each element that comes after the inserted element must be shifted. While JavaScript takes care of this for us, it will still be time-consuming with large datasets.

What if we used a binary search tree to place key-value pairs? We could assign each key a value and then sort our array that way. This would have a faster search time of O(log(N)), or logarithmic time than just using an array. However, insertion would potentially be slower than our previous option - generally O(log(N)), and logarithmic time, while faster than linear time, is a lot slower than the O(1) constant time of just pushing to an array. Also, the worst-case scenario is O(N) for insertion.

What we need is a hashing algorithm that will use an array while also utilizing the strengths of other data structures. What is the biggest strength of an array? Well, finding a value in an array is relatively slow (O(N)), but finding an element by its index is super fast (O(1)). That means we want to associate each key in our hash with the index of an array. That way, we can do a super-fast lookup. We'll also be able to take advantage of super-fast key-value insertion and removal using some other tricks as well. We'll learn about those later in the lesson.

First, we'll need to write an algorithm that will associate the value of a key with an array's index.

### A Very Basic Hashing Algorithm
To start, our application is going to use a very mediocre algorithm so we can focus primarily on how hash tables work. Super-fast hashing algorithms are complex and beyond the scope of this lesson - but we will make some basic improvements to our algorithm over the course of this lesson to deepen our understanding of hash tables.

As we now know, we need a way to turn a key into the index of an array. That means our algorithm needs to somehow turn a key that is a string into a number.

So if the first letter of a string key is `"a"`, it should be converted to an index of 0.If the first letter of a string key is `"b"`, it should be converted to an index of 1. This will apply to every letter up to `"z"`, which will be converted to an index of 25.

That means the key `"John"` would be converted to an index of 9 in our hash table's array (since J is the 10th letter of the alphabet).

This is similar to what a worker would do with an old-fashioned paper filing system. If the worker gets a file that begins with the letter `"j"`, they will put the file in the cabinet that corresponds with the letter. Likewise, when a worker needs to find a file that begins with that letter, they know exactly which filing cabinet to check.

This is exactly how a hashing algorithm works, except instead of calling it a cabinet, we call each index in the array a **bucket**. The hash we are creating will have 25 buckets, one for each letter of the alphabet. And just as file cabinets make it easier for a worker to narrow down a search for a particular file - or to know exactly where to put it - the buckets in a hash table work the same way. A real-world hash table with a lot of data will have a lot more than 25 buckets but we'll cover that later.

Let's write a test for our hashing algorithm:

```
...
  test('should return a number representation of a letter', () => {
    expect(hashTable.hash("Alaric")).toEqual(0);
    expect(hashTable.hash("zygorth")).toEqual(25);
  });
...
```

Our test will check to see if two keys can correctly be converted. Note that one starts with a capital letter and the other doesn't. It shouldn't matter what the case is - they should be translated to the same index regardless. We'd normally break this up into several tests but in the interest of brevity, we are testing multiple behaviors here.

So how can we convert strings into numbers? For our very simple algorithm, we need to grab the first letter of the string, make sure it is lower-cased, and then convert it to its corresponding ASCII code.

If you're not familiar with ASCII, it's the standard for encoding any character on a keyboard as a number. Remember that at the lowest level of our machines, characters on a keyboard don't exist - everything is binary (zeroes and ones). While we can convert characters to their binary representation, there's also a decimal representation as well. You can see an ASCII chart [here](https://www.ascii-code.com/) to see how the numbers translate.

Using this chart, a lowercase a corresponds to the number 97. The numbers go up sequentially to a lowercase z, which translates to the number 122 (decimal character code). In other words, we just need to lowercase the first letter of a string, convert it to its ASCII code and then subtract 97 from that value. That would give us a 0 for the letter a, a 1 for the letter b, and so on.

Here's the code to get this test passing:

```
...
  hash(key) {
    return key.charAt(0).toLowerCase().charCodeAt(0) - 97;
  }
...
```

We've added a method called `HashTable.prototype.hash()` which is our hashing algorithm. It takes a key as an argument.

The algorithm takes the first letter (`key.charAt(0)`) and then lower-cases it (`toLowerCase()`). Next, we use the JavaScript method `String.prototype.charCodeAt(0)`, which translates the first character into an ASCII number. Finally, we subtract 97 from the number. After all, we don't want an array with 96 empty elements before the element containing keys starting with the letter a.

If we run our test, it will pass.

At this point, we have a very basic hashing algorithm. We can use this algorithm both to store an element in a hash table and to find it - much like a worker can use a filing system to store and find files alphabetically.

The code snippet below demonstrates that we'll use the hashing algorithm in both our `HashTable.prototype.set()` and `HashTable.prototype.get()` methods.

```
function set(key, value) {
  // Hashing function converts the key into an index and then stores the value at that index in the array.
}
```

```
function get(key) {
  // The same hashing function once again converts the key into an index and then grabs that index from the array.
}
```

If we pass in the key `"John"`, our `Hash.prototype.set()` function will use the hashing algorithm to determine that the value corresponding to John should be placed at the 9th position of the hashing array.

Similarly, if we try to look up the key `"John"`, our `Hash.prototype.get()` function will use the same hashing algorithm to determine that if the key exists, it will be at the 9th position of the array (which we can also call a bucket).

### Adding a Key-Value Pair to A Hash Table
Next, we need to actually add a key-value pair to our hash table. Let's start with a test.

```
...
  test('should correctly set a key-value pair in a hash table', () => {
    hashTable.set("John", "Lead Singer");
    expect(hashTable.array[9]).toEqual([["John", "Lead Singer"]]);
  });
...
```

In this test, we call `HashTable.prototype.set()` with two arguments: the first is the key while the second is the value.

In our expectation, we check that `hashTable.array[9]` is equal to the following: `[["John", "Lead Singer"]]`. Note that we have to hard-code the index of the array because we haven't written a `HashTable.prototype.get()` method yet.

The value of the bucket at the ninth position should be the following: `[["John", "Lead Singer"]]`. Why is this a nested array and why are we storing both the key and the value? Well, our array only has 25 buckets, one for each letter of the alphabet. What will happen when we want to add Jane to this hash table like this?

```
hashTable.set("Jane", "Fan of The Beatles")
```

We can't just store values in the bucket corresponding to the letter J because the bucket would look like this after adding both John and Jane:

```
["Lead Singer", "Fan of The Beatles"]
```

How would our application know which value corresponds to John and which to Jane? In order for our application to know the difference, we need to store the keys as well:

```
[["John", "Lead Singer"], ["Jane", "Fan of The Beatles"]]
```

When an element is added to a hash table bucket that already contains an element, it's known as a **collision**. So what is the point of a collision? Why would a hash table account for one?

Well, our hash table implementation has only 26 buckets. That's not nearly enough for a good hash table. There are going to be lots of collisions - and if we were storing millions of names, it wouldn't be very efficient.

From that perspective, wouldn't it make sense to just avoid collisions altogether? Why not assign a unique index for every possible key? That wouldn't be a good idea. Here's why.

Let's say we want to create a hashing algorithm that can assign a different index in an array for each unique string. And let's say we limit the length of a string key to 6 characters. There are more than 300 million permutations of 6 characters just for lower-case letters. So let's say you add your first element to a hashing algorithm and it assigns a value at the 100 millionth position for a unique string. Congratulations! Your hash now uses an array that has information about one key-value pair but has 100 million elements. That's terrible in terms of space and memory considerations.

To deal with this issue, we need to be a little less picky. Instead of giving every single permutation of characters a unique spot in an array, we use buckets instead - just like we are doing so far. We account for buckets having some collisions - but not very many. For instance, with our mediocre algorithm, if our hash table needed to store millions of names, the bucket corresponding to the letter J might hold a huge number of values. In order to find the key-value pair we want, we'd need to do an inefficient search.

Ideally, however, the number of values in a hash table's array should not exceed the number of buckets. So if you have one thousand values, you should have at least one thousand buckets as well. However, just because the number of buckets roughly corresponds to the number of values being held, it doesn't mean you won't have a lot of collisions. If you have a good algorithm, you shouldn't have too many - but you'll always have to account for at least some. We'll discuss this further in the next lesson.

Let's get our test passing now. Once again, we'll group a few behaviors together to keep things moving quickly.

```
...
set(key, value) {
  const index = this.hash(key);
  if (this.array[index] === undefined) {
    this.array[index] = []
  }
  this.array[index].push([key, value]);
}
...
```

Our `HashTable.prototype.set()` method takes two arguments: a key and a value. For instance, in our test, we do the following: `hashTable.set("John", "Lead Singer");`.

Next, we do the following:

```
const index = this.hash(key);
```

This is our hashing algorithm at work! All we do is pass in a key and our `HashTable.prototype.hash()` method will determine what index the key should correspond to.

Next, we check the following:

```
if (this.array[index] === undefined) {
  this.array[index] = []
}
```

If there is no value at the specified index of the hash table's array, it will be `undefined`. That means we need to place an empty array there. After all, we can't push anything to `undefined`.

Finally, we do the following:

```
this.array[index].push([key, value]);
```

We push the specified key-value pair `("John", "Lead Singer")` to the array at the specified index.

To sum up, our algorithm determines where the key of `"John"` should go. Based on our algorithm, the index should be 9. Then our method checks to see if any key-value pairs are stored in the array at the index of 9. This is the bucket where all J values should be stored. If there aren't any values there yet, an array is initialized. If there are values, we know an array already exists there and we move onto the next step. We push the new key-value pair into its correct bucket.

By the way, we are using arrays in our buckets to keep things simple. In a hash table with few collisions, using arrays for buckets works just fine. However, there's always the possibility of a lot of collisions, and for that reason real-world hash tables generally use linked lists for their buckets. We will learn about linked lists in another lesson. However, it isn't strictly necessary to know about linked lists to understand the basics of how hash tables work. And as we mentioned before, each individual bucket shouldn't have too many collisions - but we still have to account for the possibility.

In general, this `HashTable.prototype.set()` method encapsulates what this method should do in just about any hash table implementation. The difference would be the hashing algorithm itself - as well as some differences in the code depending on how to best add the key-value pair to the data structure being used for the bucket.

### Retrieving a Value From a Hash Table
Next, we need to be able to retrieve a value from our hash table. We'll use the same hashing algorithm to help us retrieve values. But first we need to write a test.

```
...
test('should correctly get a key-value pair from a hash table', () => {
  hashTable.set("John", "Lead Singer");
  hashTable.set("Jane", "Fan of The Beatles");
  expect(hashTable.get("John")).toEqual("Lead Singer");
});
```

Our test will add two key-value pairs that will go into the same bucket. Then it will expect our new `HashTable.prototype.get()` method to grab the correct value for the key `"John"`.

Here's the code to get this test passing:

```
get(key) {
  const index = this.hash(key);
  const bucket = this.array[index];
  for (let i = 0; i < bucket.length; i++) {
    if (bucket[i][0] === key) {
      return bucket[i][1];
    }
  }
}
```

Look at the first line inside our method:

```
const index = this.hash(key);
```

Hashing algorithms are very fast - they don't use loops so they are always O(1) constant time. And because they are combined with putting an element in an indexed array, another O(1) operation, the process of finding buckets for either retrieving or setting key-value pairs is very fast.

Next, we determine which bucket we need to look in:

```
const bucket = this.array[index];
```

We actually don't need this line of code but saving this value in a bucket variable makes it a little clearer what's happening here.

Finally, we need to loop through key-value pairs until we find the correct key. At that point, we can finally return the corresponding value.

```
for (let i = 0; i < bucket.length; i++) {
  if (bucket[i][0] === key) {
    return bucket[i][1];
  }
}
```

Remember that each bucket is an array of arrays. The nested arrays are key-value pairs. The value at position 0 of each nested array is the key while the value at position 1 of each nested array is the value.

If we find a key in one of the nested arrays (`bucket[i][0]`) that matches the key we passed into our method, we return the value that corresponds with it (`bucket[i][1]`). If you're feeling confused about the bracket notation there, remember that the `[i]` represents iterating through the bucket while the `[0]` or `[1]` represents the key or value of each nested array.

There are a few other use cases we need to consider. What if the key doesn't exist in the bucket? Also, what if no values exist in the bucket at all?

Here's a test for when a bucket doesn't have any values:

```
...
test('should return null if the bucket has no values', () => {
  expect(hashTable.get("John")).toEqual(null);
});
...
```

And here's our updated method:

```
get(key) {
  const element = this.hash(key);
  const bucket = this.array[element];
  if (bucket != undefined) {
    for (let i = 0; i < bucket.length; i++) {
      if (bucket[i][0] === key) {
        return bucket[i][1];
      }
    }
  }
  return null;
}
```

First, if our bucket is `undefined`, it won't run through the loop. We also add `return null`; at the end of our method. This means our method will return null if the bucket is `undefined` (our test condition) or if the bucket is defined but doesn't include a value. We should still confirm that it correctly returns null if the bucket contains key-value pairs but not the key we are looking for. Here's the test:

```
...
test('should return null if the bucket does not contain the key we are looking for', () => {
  hashTable.set("John", "Lead Singer");
  hashTable.set("Jane", "Fan of The Beatles");
  expect(hashTable.get("Jim")).toEqual(null);
});
...
```

If we run it, we'll see that the test is passing.

In this lesson, we've written a basic hash table implementation with three methods. We didn't include a `HashTable.prototype.remove()` method because the implementation is basically the same as it would be with `HashTable.prototype.get()`. The only difference is that instead of just returning a value once it finds a key, it actually removes that key-value pair from the hash table.

At this point, you know the basics of how a hash table works - and you know the underlying concepts and mechanisms of how they're implemented, whether that's in JavaScript, C#, Ruby, or another language.

In the next lesson, we'll cover ways we could make our hashing algorithm much faster.