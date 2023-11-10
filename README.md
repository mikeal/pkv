# pkv - Public Key-Value Cryptography 

Public Key-Value Cryptography (PKV) is a collection of simple cryptographic processes that can be used to create permissionless data systems.

The use cases for these systems fall into two categories:
* Systems for storing new data and
* Systems describing already existent data

The later category is perhaps the most critical to focus on. Since PKV is a purely cryptographic process describing key-value pairs it functions as a fair-use common description of what already exists. As such, it is not copyrightable, because it's just a bunch of hashes and signatures 😅

This gives us, the **public**, the right to build whatever systems we want describing the digital world ***as it appears***. Anything already viewable, including everything on The Web, can be used as a data source without any permission granted from the owner, because all you're doing is cryptographically describing what is in common public view *as it appears in pubic view*.

# Key Cryptography

A PKV-KEY is a recursive cryptographic process applied to a sequence of values (KEY "parts").

No specific encoding of these values can be defined herein because differences in encoding introduce indeterminism into the process. Agreement about encoding is part of the consensus agreement parties need to reach in order to arrive at a consensus achieving result.

Examples herein will use strings, encoded into sequence as a JSON array.

```
[ "twitter.com", "@mikeal", "fullname" ]
```

The PKV-KEY can be determined by, 
1. Passing the first part of the key to a hash function, then hashing the result again (double-hash),
2. the binary result of the prior step, followed by the hash result of the subsequent part, is passed to another hash function
3. the binary hash result of which is used to repeat step 2 with every key part until all parts in the key are exhausted.

This final result is PKV-KEY.

Note: The double-hash of the first key part is necessary to avoid conflicts.

# Value Cryptography

A PKV-VALUE is guaranteed to be unique to each PKV-KEY, and also guaranteed to be uniquely a PKV-VALUE (cannot conflict with PKV-KEY).

The process is simple,
1. The value being described is passed to a hash function.
2. The binary result of the prior step, followed by the PKV-KEY, are passed to a hash function.

This result is PKV-VALUE.

# Key-Value Cryptography

A PKV-KV is a Key-Value pair. The process is simply,
1. the binary hash result of PKV-KEY, followed by PKV-VALUE.

This result is PKV-KV.

# Namespace Cryptography

There is no "PKV-NS" because a namespace in PKV is just a PKV-KEY. 

The PKV-KEY [ "twitter.com", "@mikeal", "fullname"] is securely in the namespace of [ "twitter.com", "@mikeal" ] and our PKV-KEY cryptography already produces a unique identifier for that as well. This is true for any position in the key sequence.

Note that, cryptographically speaking, this is a sequence and not a heirarchy. A heirarchy could be formed as a view of the sequence, but all manor of typed arrays and even sets can also be encoded as a sequence.

The imposition of heirarchy is only possible in the process of authentication which is not defined herein. Beware, it is easy to mistakenly believe that the characteristics associated with a heirarchy somehow exist intrinsically in the key rather than an authenticated view of it.

For instance, it is laughable that an anonymous actor could claim to be a source of authenication for the namespace [ "twitter.com" ] but once a developer gets used to an interface that handles this sort of authentication for them migrates to one that doesn't they may fail to realize the vulnerability inherent in this false trust.

# Verifiable Claim Cryptography

While agreement on an identifier for a Key-Value pair is rather simpl3, the method for ***authenticating*** that pair has unbounded complexity.

Using our previous example of [ "twitter.com", "mikeal" ], whatever we wish to claim the "true" value to mean should be authenticable by looking at something publicly available at https://twitter.com/@mikeal

We may also wish to trust other parties, like the Internet Archive, to authenticate. We may know how to authenticate the value using the Twitter API (if there's no concern about being blocked or violating a usage agreement).

Given the unbounded complexity of this problem we should assume numerous systems for authentication will exist, with compatibility between them dependent upon negotiating a consensus on how to authenticate.

The following cryptography is offered to guide that process as it establishes the simplest and more easily agreement forms of signature.

## Time Agnostic Signature Cryptography

While not recommended (signatories should prefer time dependent signatures) we begin with the simplest claim signatures.

* PKV-KEY-SIG
* PKV-VALUE-SIG
* PKV-KV-SIG

which are simply a public key signature of the binary hash result of PKV-KEY, PKV-VALUE, and PKV-KV respectively.

## Time-Space Dependent Signature Cryptography

The recommended form of signature is one that depends upon some useful (presumably deterministic) encoding of time and/or/with space being used to differentiate it from all other signatures.

There are numerous ways of encoding serialized of time and space, none of which are specified herein as they are considered a part of the consensus process actors find agreement on in order to arrive at compatible systems.

The signatures for
* PKV-KEY-SIG
* PKV-VALUE-SIG
* PKV-KV-SIG

are arrived at by
1. passing the serialized time-space information to a hash function,
2. the binary hash result of step 1, followed by the binary result of either PKV-KEY, PKV-VALUE, and PKV-KV, are passed to a public key signing function.

the resulting signature is either PKV-KEY-TS-SIG, PKV-VALUE-TS-SIG or PKV-KV-TS-SIG respectively.

# Network Cryptography

PKV is "of The Web" in that it does not attempt to create an alternate web or network protocol and can be used in combination with existing protocols and networks.

Network cryptography herein describes a method of **linking** in PKV-KEY that can be used to form authenticable data networks in PKV.

In adding static and dynamic linking to PKV-KEY we find the means to build and manage sophisticated dependency structure that are not only secure but can be updated and patched securely as well.

Programming into a keyspace isn't something most programmers are used to but it's widely practiced in database engineering and has been applied cryptographically herein.

## Link Cryptography

We have already specified the following processes that arrive at unique hash results for:

* PKV-KEY
* PKV-VALUE
* PKV-KV
* PKV-KEY-SIG
* PKV-VALUE-SIG
* PKV-KV-SIG

Since these are all differentiated cryptographic processes, they all arrive at unique results and cannot conflict (assuming the security of the hash function is intact).

This means that these results can be used as parts of a key and will not naturally conflict with any other key. Even though we aren't specifying an encoding or differentiating these by type, we have cryptographic guarantees against natural colision. It does however mean that anyone can write into the key space may also be able to write a secure link of their choosing.

Parsing links from PKV-KEY requires knowledge of the value that arrives at a hash result that can be matched against a key part.

Therefore, all links in PKV-KEY must be written as a static link.

However, dynamic linking is available to **any** actor that has enough knowledge to understand the link. Therefor any trust dependent link can be treated as a dynamic link if the actor parsing the link wishes to update to a different dependency.

Essentially, all links are static, and if you have enough information you can authenticate a link you can treat it like a dynamic link and upgrade the static link upon the next transaction.

## Fork Cryptography

A fork could almost be thought of as a "consensus to salt" PKV-KEY.

The position of the salt, and the method with which one salts, are left to those seeking to achieve a consensus.

As an example, if I wanted to create a namespace for twitter user "inside" another twitter user (presumably authenticated by them to do so) that process could produce keys that looked something like:

[ "twitter.com", "@mikeal", PKV-KEY (of secondary user) ]

An interface could now be produced for reading and writing that prefixes that keyspace.

This this inserts a secure salt into every PKV-KEY it is applied to this is the perfect method for producing and maintaining forks.

Since this method to agreement can be implemented by any party who can manipulate the key space it is a highly accessible freedom to form any data described in PKV.

This freedom is beyond the bounds of property governed by copyright as it is **unownable** and its basis already agreed to.

# on Hash Functions

No specific hash function is specified because any is acceptable insofar as it has been agreed upon by the parties establishing a consensus.

As the cryptography is truly agnostic, any party able to compute a complete proof in one hash function could produce and authenticate a provable cryptographic equivalency with another.

# on Signing Methods

No specific signing method is specified because any is acceptable insofar as it has been agreed upon by the parties establishing a consensus.

As the cryptography is truly agnostic, any party able to compute a complete proof in one signing method could produce and authenticate a provable cryptographic equivalency with another.

# on Encodings

No specific encoding is specified because any is acceptable insofar as it has been agreed upon by the parties establishing a consensus.

As the cryptography is truly agnostic, any party able to compute a complete proof in one encoding could produce and authenticate a provable cryptographic equivalency with another.

