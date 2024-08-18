# Threat models

Peergos supports several threat models depending on the user and their situation.

## Casual user:
* Trusts the SSL certificate hierarchy and the domain name system
* Is happy to run Javascript in their browser
* Trusts TLS and their browser (and OS and CPU ;-) )

Such a user can interact with peergos purely through a public web server that they trust over TLS.

## Slightly paranoid user:
* Doesn't trust DNS or SSL certificates
* Is happy to run Javascript served from localhost in their browser


This class of user can download and run the Peergos application and access the web interface through their browser over localhost.

## More paranoid user:
* Doesn't not trust the SSL certificate system
* Doesn't trust DNS
* Doesn't trust javascript

This class of user can download the Peergos application (or otherwise obtain a signed copy), or build it from source. They can then run Peergos locally and use the native user interface, either the comand line or a FUSE mount. Once they have obtained or built a copy they trust, then they need trust only the integrity of TweetNacl cryptography (or our post-quantum upgrade) and the Tor architecture. 

# General Threat model
## Actors and capabilities
1. Device with logged out client
Can See:
* any cached ciphertext blocks and mutable pointers

Cannot see:
* any plaintext of any files or directories or their metadata. 

2. Home server
Can see:
* total space used (rounded up due to padding before encryption). 
* total number of secret links and any expiry, use limit and use count

Can't see:
* file/directory names
* file/directory sizes
* file thumbnails
* whether a chunk is part of a directory or file
* how many files or folders there are
* who has access to a file or folder
* what file/dir or chunk a secret link points to
* your friend or followers list

3. Mirror servers
Can see the same things as your home server

4. Friends or followers
* Can retrieve ciphertext for things they have been granted access to. 
* Can read or modify files or directories they have been granted access to.

5. Global active network adversary
* Cannot MITM connetions
* Cannot MITM friends
