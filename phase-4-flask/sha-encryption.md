## SHA and Encryption
Hash algorithms are often used for passwords. You might've heard of SHA encryption before - for instance, SHA-1, SHA-256, and so on. In fact, SHA is short for *Secure Hashing Algorithm*. With a secure hashing algorithm, someone can create a password and then the secure hashing algorithm will scramble it and add salt to the password. Salting a password just means obscuring it further by adding additional characters to the password. 

Let's say that I have a password for an account that's `bestPasswordEver123!` It's a terrible password but the application where I have the password would hash this password and save it in a table. The hashing algorithm will add salt and turn it into a string that looks like gibberish. Let's say that the hashed password is `1bas23drdvzad34abcz9783924`. Now we have the password in encrypted form. We can type in `bestPasswordEver123!` when we log in, the password will be encrypted, and then it will be compared to its entry in the password table. If it matches, you'll be successfully logged in. However, if it doesn't match, the login attempt will fail.

However, if malicious users get the password table, they wouldn't be able to do anything with it. That is because secure hashing algorithms are one way operations - you can hash a password but you can't unhash it! We don't want to be able to decipher it - we just want to be able to input a password and see if it matches the hashed password - we don't ever want to unhash the hashed password - because only hackers would want to do that. So while we can correctly input `bestPasswordEver123!` and the hashing algorithm will match it to the correct value to allow us to log in, we can not get `bestPasswordEver123!` from `1bas23drdvzad34abcz9783924` - unless, of course, there's something wrong with the hashing algorithm.

In order for a secure hashing algorithm to be good, it has to keep collisions to a minimum and also use what is known as **avalanching**.

Avalanching means that even a very small change to a string leads to major changes to the encrypted password. The [Wikipedia entry on SHA-2](https://en.wikipedia.org/wiki/SHA-2) gives a great example of the avalanching effect at work. In the example below, SHA-2 is used to encrypt two strings that have only a single character difference (a period at the end of the sentence).

```
SHA224("The quick brown fox jumps over the lazy dog")
0x 730e109bd7a8a32b1cb9d9a09aa2325d2430587ddbc0c38bad911525
SHA224("The quick brown fox jumps over the lazy dog.")
0x 619cba8e8e05826e9b8c519c0a5c68f4fb653e8a3d8aa04bb2c8cd4c
```

This is sufficient to change 111 of the 224 bits in the hashed result!

The reason for avalanching is simple - the easier a code is to break, the worse it is. If you can make a code unbreakable - or nearly so - that means it's very well hashed. However, if only a very small change occurred when we added a character to the string we want to hash, it would be much easier to determine the pattern and reverse engineer the code.

In terms of collisions, it should also be clear why we want to avoid them with secure hashing algorithms. The more collisions, the easier it is to guess a password. For instance, if the corresponding key-value pair for `Password123!` in a hash table that has ten potential collisions, a malicious user is effectively checking ten passwords at once when they check `Password123!` Secure hashing algorithms can be vulnerable to collision attacks where malicious users attempt to break into a system by taking advantage of collisions. On the other hand, occasional collisions are possible - but the chances that a password collision will occur are extremely small.

Another interesting thing to note about secure hashing algorithms - they are designed to be very slow. Why is that? Well, think about the use case of a secure hashing algorithm - the algorithm should be able to check to see if a password matches a hash. If the algorithm were extremely fast (and it certainly could be if we didn't add bottlenecks), it would be easy to test tens of thousands of passwords very quickly. That's not something we want to do - but it would help malicious hackers crack passwords because they could test more combinations faster to see if any match a hashed password.

Finally, there is a big difference between hashing and encryption. Hashing is a one-way process - we turn a password into a hash but we don't ever turn the hash back into a password. Encryption, on the other hand, is a two-way process. If we encrypt a message (say, through an encrypted message service) and send it to someone else, we want that other person to be able to read the original message. That means it needs to be decrypted when it reaches the other end. We won't cover encryption in this lesson because it isn't related to hashes - however, it is important that you understand this basic distinction between secure hashing and encryption - the first is a one-way process while the second is a two-way process.

### Practice
Now it's time to apply our new knowledge about hash tables and algorithms.

1. #### Homemade Hash Table
    Create your own hash class and algorithm from scratch. You can use the one we created a few lessons ago as a template - but the next step is to improve it further. Use a TDD approach to build out your class and methods.

    The hash table should have the following methods:
    * `HashTable.prototype.set()`: Should add a key-value pair to the hash table.
    * `HashTable.prototype.get()`: Should get a key-value pair from the hash table. Try incorporating a linked list!
    * `HashTable.prototype.remove()`: Should remove a key-value pair from the hash table.
    * `HashTable.prototype.clear()`: Should clear all key-values from the hash table.
    * `HashTable.prototype.hash()`: The hash is the most important part!

    For your `HashTable.prototype.hash()` method, you can try the following:
    * Create a hash that will store at most 10000 key-value pairs.
    * Try out the DBJ2 hashing algorithm. Make your own modifications such as applying bitwise manipulation in a different way.
    * **More difficult**: Add a `HashTable.prototype.resize()` method that will double the size of the hash table when it's full or nearly full. You can make this a "manual" method that a developer would call if they see that the table is near its capacity. Note that it's the hashing method that really determines how big the table will be in JavaScript since arrays can be of any size. So that means that a `HashTable.prototype.resize()` method will need to create a new array and then use the updated hashing algorithm to recalculate where all the key-value pairs should go in the new array.

2. #### Secure Hashing Algorithm
    Try making your own (probably not so) secure hashing algorithm. Once again, use a TDD approach.
    * Incorporate bitwise manipulation in your algorithm.
    * Try adding salt to your algorithm. This could be a nonsense string appended to the end of a password - and this nonsense string should be computed based on the original password.
    * See how you can increase the avalanching effect of your algorithm. The easiest way to do this would be to compare what happens when `"cat"` and `"bat"` are passed into the algorithm. How different are the two hashes? You should also try to avoid symmetry - which means that `"bat"` and `"tab"` could end up with the same hashes because they have the same characters. So the order of characters in your hashing algorithm also matters!
    * Add functionality to artificially slow down the computing time of your algorithm so that hackers couldn't easily use it for a brute force attack.

