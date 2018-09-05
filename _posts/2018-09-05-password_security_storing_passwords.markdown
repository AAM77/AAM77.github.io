---
layout: post
title:      "Password Security: Storing Passwords"
date:       2018-09-05 17:04:06 -0400
permalink:  password_security_storing_passwords
---


## Summary:

Web developers have used (and still use) various methods to store user passwords. Some of these methods—such as storing passwords in plain text—are bad practice, but people still use them. One of the best ways to protect passwords from being stolen is to use salted password hashing. This post describes the basics of how these methods work and how they differ from one another.<br>
<br>
<br>
**You should not reinvent the wheel when it comes to password security algorithms. Use existing implementations and approved peer reviewed algorithms.**<br>
<br>
It is important to learn about password protection and how to secure passwords properly, but it is best to let users sign in using something like their Facebook, Google+, Twitter, or GitHub account. This way, you have to worry less about issues related to storing and protecting user passwords for your website or app. Ruby, for example, has a number of gems in the OmniAuth family to provide this functionality. If you do store passwords, do not write your own algorithm for hashing a password. *It is too easy to get it wrong.* More knowledgeable and experienced coders have already done this for us.

**The bottom line:**<br> 
<br>
(1) Learn about the different forms of password protection.<br>
(2) Salted hashes are the best method for protecting and storing passwords.<br>
(3) Do not reinvent the wheel. Others have already solved this problem for you. Use existing libraries that have already undergone extensive peer review (such as the various implementations of [Bcrypt](https://en.wikipedia.org/wiki/Bcrypt)).



### End of Summary
<br>
<br>

## Password Protection

Let's face it: there is a good chance that people tend to use the same password for two or more of their accounts. Regardless of what the user does, it falls on web developers and companies to store user passwords as securely as possible.<br>
<br>
One thing I want to point out before continuing is that there is no way to prevent hackers from using brute force to uncover passwords. As computer processors become faster, it becomes easier for hackers to use such methods. Fortunately, we can discourage them from doing so by making the process difficult or time consuming.<br>
<br>

### { Methods for Storing Passwords }

Four methods used to store passwords include:<br>
<br>
(1) Plain text<br>
(2) Encryption<br>
(3) Password Hashing<br>
(4) Salted Password Hashing<br>
(5) Slow Hashes [*Briefly mentioned at the end as a "Final Note"* ]
<br>

### { Plain Text }

Storing passwords in plain text is the least secure. Unfortunately, some well known companies still use this method. If a website lets users retrieve their passwords via email, there is a good chance it is being stored in plain text. What this means is that the password is stored the same way it is types in a users tables table on the database. It might look something like:

```
user id |   username    | password
---------------------------------------
001     | apple_minion  | peeler23
002     | cinna_man89   | yumyum12
003     | super_doctor  | lifesaver51
        |               |
```

As you can see, each user's password and username are clearly visible in the database. A hacker who manages to gain access to the database would have no trouble accessing any of the accounts. All of the necessary log in information is in plain sight and the hacker does not have to exert anymore energy.

**Authenticating a User at Login**<br>
<br>
Authentication is simple with this method of password storage. A user enters the username and password for an account at the login screen. The system finds the account associated with the username. It then compares the stored password for the account with the input password. If it matches,the user gets logged in. Otherwise, not.<br>

**Downsides**

This part should be pretty obvious. There is no extra work involved for the hacker. All anyone has to do is gain access to the database. The worst part is that a hacker or anyone else who obtains the password will probably try the the same username and password combinations on other websites. Since users tend to re-use the same emails, usernames, and passwords,   such individuals could porentially access other accounts with the same information.<br>
<br>

**Is it a good way to store passwords?**<br>
<br>
*Not good.* Plain text is the absolute worst way to store passwords on a database. *Just don't do it.*<br>
<br>

### { Encryption }

Encryption is slightly more secure than plain text. Basic encryption involves using a special key to convert the password into a random string before storing it. However, the process is two way. The system can use the same key to decrypt the password and return it to its original form. This is problematic because even though the passwords are stored in their encrypted form, a hacker needs only the special key to decrypt them. Often times, the key is stored on the same server as the passwords, so this it not too difficult.<br>
<br>

After getting encrypted, the passwords in a users table might look something like this:

```
user id |   username    | password hash
-------------------------------------------------------------------------
001     | apple_minion  | gwi1afCi3Bl6uR1q13xK+vDDy7KtC6EE93vto2Lx//A=
002     | cinna_man89   | CFT4aJQxBIkMIOWptld/qO7vbpazq7rQt/vvGzq+BH8=
003     | super_doctor  | VMT8R5Oqs1pcVDGOsfsShGC55RRwPEZbsVhQrOG2ULY=
        |               |
```

This is also what a hacker might see after first gaining access to the database.

**Authenticating a User at Login**

When a users want to login into an account, they enter the username and password associated with the account. The system finds the account by the username, if there is a match. Afterwards, it uses the master encryption key to decrypt the matching username's password (i.e. temporarily converts it back to the original password). It then compares this to the input password. If it matches, the system grants the user access to the account. Otherwise not.

**Downsides**

(1) If two or more users use the same password for each of their accounts, the encrypted string will look exactly the same for each of them. This is bad because anyone with access to the database might be able to use other clues stored in the database (like password hints) to figure out what the password is.<br>
<br>
(2) Let's say the hacker finds the encryption key used to generate the database.
```
encryption_key | 34ebgdbf1thf0b5k5f12a0o5a9sba
```

Once the hacker uses it to decrypt each password, each user's password will be in plain view:
```
user id |   username    | password
--------------------------------------
001     | apple_minion  | peeler23
002     | cinna_man89   | yumyum12
003     | super_doctor  | lifesaver51
        |               |
```

The hacker needs to run the decryption only once, so even if it takes a long time, it is worth it.<br>
Once the encrypted passwords get converted to their original (decrypted) form, we run into the same problem that we faced with passwords stored as plain text.<br>


**Is it a good way to store passwords?:**<br>
<br>
*Not good.* Encryption is *not* a good way to store passwords.<br>
<br>

### { Hashing }

Similar to encryption, the basics of password hashing involves converting a password into a long string of seemingly random characters. This means that a string like ```peeler23``` might end up looking something like ```fk1Abjhg2kj4kooaSd112pfittynnl8329ayq4``` after it gets hashed.  So, when a user creates a new password, it gets hashed using this algorithm and key. This hash then gets stored in the database.<br>
<br>
Unlike encryption, however, hashing is a one-way street. Once we turn a password into a hash, we cannot change the hash back into the original password. There is no master key present that lets us do this. Side note: in more complex forms of hashing, the key that gets used for generating a hash might be a file filled with a large amount of randomly generated text.<br>
<br>
A table of usernames and hashed passwords might look something like:
```
user id |   username    | password hash
-------------------------------------------------------------
001     | apple_minion  | 5072293ebdbf1f0550804868eaf5355f
002     | cinna_man89   | c3e35d624ca04ddd716d852490718bcc
003     | super_doctor  | e3ec512a05ba8d861127a858299647ee
        |               |
```

This looks similar to what we did using encryption. The difference, as I mentioned, is that there is no way to find the original password.<br>
<br>

**Authenticating a User at Login**<br>
<br>
When an existing user enters a username and password at log in, the system pulls up the account with the matching username. Since the system cannot convert the stored hash to its original form, the input password get converted to a hash, instead, using the same algorithm as before. This hash is compared to the one stored in the database. If it matches, the user gets successfully logged in. Otherwise, the user is denied access.<br>

**Downsides**<br>
<br>
(1) If two or more users have the same password, the hash that gets generated will also be the same. A hacker or anyone else with access to the database can use other information in the table (like password hints) to find the password. <br>
<br>
(2) Even though a hacker would not be able to convert the hash back to the original password, they can brute force it by converting many different passwords into a hash until one matches the one stored in the database. This might have been a problem decades ago with slower processors, but not any longer. Today, computers can do this very quickly.<br>
<br>
(3) Rainbow tables exist for each hash generating algorithm: since the hash generated by the algorithm will always be the same for identical passwords across databases, hackers need to run their brute force tactic only once. Hackers have taken advantage of this weakness and created something called rainbow tables for each of the algorithms that exist for generating hashes. Each of these tables contains a collection of trillions of passwords with columns for a password and its respective hash. All any hacker needs to do is search for the hashes they are interested in to find out the password it represents. This is much quicker for them than having to do the work themselves. Google, has become a great resource for finding such rainbow tables. Some sites offer searches for multiple rainbow tables all-in-one: [CrackStation](https://crackstation.net/).

**Is it a good way to store passwords?**<br>
<br>
*No, not really.* New algorithms keep emerging to "stay ahead of the game," but as computer systems become faster, it will take increasingly less time for hackers to crack them. They also aren't good because people can create rainbow tables for them. Once rainbow tables get created for an algorithm, it seems to become almost useless. That is my current understanding, at any rate.


### { Salted Password Hashing }

Generating a salted hash is similar to generating a regular hash, with one key difference: it gets salted. The process of adding salting a hash adds a new step before the password gets hashed:<br>
<br>
(1) First, the system generates a long, complex, unique, randomized hash. This is the *salt*.<br>
(2) Next, it adds this unique hash (*salt*) to the front or end of the password.<br>
(3) Then, it hashes this combination of *salt* and password.<br>
<br>
Note: Good algorithms create a *different* salt for each password. This way, even if multiple users have identical passwords, each one will have a unique salt, and thus a unique hash. This method becomes less effective if the same salt gets used.<br>
<br>

Let's see this in action with some examples using the table from before with three additional users.<br>

```
user id |   username    | password
--------------------------------------
001     | apple_minion  | peeler23
002     | cinna_man89   | yumyum12
003     | super_doctor  | lifesaver51
004     | string_artist | peeler23
005     | high_achiever | peeler23
        |               |
```

Here, we have three accounts with the same password:<br>
```
user id |   username    | password
--------------------------------------
001     | apple_minion  | peeler23
004     | string_artist | peeler23
005     | high_achiever | peeler23
        |               |
```

Focusing on only these accounts, let's hash them using salts:<br>

```
user id |   username    |        salt        |       password + salt      |            hash value
-------------------------------------------------------------------------------------------------------------
001     | apple_minion  | 67848cafe220fd91e7 | peeler2367848cafe220fd91e7 | 449cd9911418a668dfcf0dfee352f8f2
004     | string_artist | 9632f83c78aa9b2582 | peeler239632f83c78aa9b2582 | 4daba3babea133f130da41bbda1b5876
005     | high_achiever | 77ab219a5b0b602ceq | peeler2377ab219a5b0b602ceq | bdde4f2757dd0f5eae0ae71270bbf5e6
        |               |                    |                            |
```
<br>
***Note:*** The *hash value* may also be referred to as the *password digest*.
This is only an example because the password never gets stored in the table. That would defeat the purpose of a salted hash. Only the *user id*, *username*, *salt*,  and *hash value* get stored:<br>

```
user id |   username    |        salt        |            hash value
--------------------------------------------------------------------------------
001     | apple_minion  | 67848cafe220fd91e7 | 449cd9911418a668dfcf0dfee352f8f2
004     | string_artist | 9632f83c78aa9b2582 | 4daba3babea133f130da41bbda1b5876
005     | high_achiever | 77ab219a5b0b602ceq | bdde4f2757dd0f5eae0ae71270bbf5e6
        |               |                    |
```

**Authenticating a User at Login**<br>
<br>
When a user enters a username and password to log into an account, the system looks for an account with a username matching the one provided at the login screen. Once it finds a match, it takes the salt associated with that username and adds it to the password in the way the algorithm tells it to. It then converts the combination (input password + stored salt) to a hash using the original algorithm. If the resulting hash matches the stored hash value, it lets the user in.<br>
<br>

**Why Salts are Useful**<br>
<br>
(1) Proper salting prevents pre-computation attacks (such as the creation of rainbow tables by hackers). This is because salting greatly increases the storage requirement, to the point that it becomes impractical. For more information, these stackoverflow answers might help:[Stack Overflow: salt generation](https://stackoverflow.com/questions/1645161/salt-generation-and-open-source-software/1645190#1645190)  and [Stack Exchange: how to store a salt](https://security.stackexchange.com/questions/17421/how-to-store-salt/17435#17435)<br>
<br>
(2) So, yes, a hacker *can* still use brute force to uncover the password. There is a key difference, however, is that they need more processing power to do so and, once they uncover a password, they uncover only one password for one account. The database might contain other accounts with the same password, but the unique salts prevent the hacker from knowing which ones those are. This makes life much more difficult for the hacker.

**Is it a good way to store passwords?**<br>
<br>
*Yes (at least until someone comes up with something better*. The main strength of this lies in the difficulty of creating rainbow tables. *Note: *Slow hashes (see below) might work better).

### Final Note: { Slow Salted Hashes }

Okay, so there is one more type of hashing I learned about after I finished writing this blog: *Slow Hashes*.

Slow hashes differ from the hashes above in that their algorithms take longer to compute a hash for a particular password. This includes the password a user (or hacker) enters during login. This slows down each brute force attack, which decreases the number of attempts a hacker can make per hour, for example. [Bcrypt](https://en.wikipedia.org/wiki/Bcrypt)'s hashing algortihm does something similar to this. Ruby (with the Sinatra and Rails frameworks) has its own [bcrypt gem](https://github.com/codahale/bcrypt-ruby).


## Sources:
1. [Caleb Woods: OWASP Password Hashing in Ruby](https://www.calebwoods.com/2014/08/26/owasp-password-hashing-ruby/)
2. [Computerphile: How NOT to Store Passwords!](https://www.youtube.com/watch?v=8ZtInClXe1Q)
3. [CrackStation: Salted Password Hashing - Doing it Right](https://crackstation.net/hashing-security.htm#normalhashing)
4. [Safely Storing User Passwords: Hashing vs. Encrypting](https://www.darkreading.com/safely-storing-user-passwords-hashing-vs-encrypting/a/d-id/1269374)
5. [The Guardian: Passwords and hacking: the jargon of hashing, salting and SHA-2 explained](https://www.theguardian.com/technology/2016/dec/15/passwords-hacking-hashing-salting-sha-2)
6. <a href="https://en.wikipedia.org/wiki/Salt_(cryptography)" target="_blank">Wikipedia: Salt (cryptography)</a>
7. [Wired: Hacker Lexicon: What is Password Hashing?](https://www.wired.com/2016/06/hacker-lexicon-password-hashing/)
8. [Hashing in Actions: Understanding Bcrypt](https://auth0.com/blog/hashing-in-action-understanding-bcrypt/)


