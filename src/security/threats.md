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