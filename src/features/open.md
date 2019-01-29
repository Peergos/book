# Open source

Peergos is fully open-source, both clients, and server (incuding the web-interface). The main interface is a web-ui, but Peergos can also be accessed using a Java client on the command line, or with a FUSE mount of your Peergos filesystem. 

No part of our infrastructure, apart from TLS and Peergos private keys, are secret. We also have reproducible builds (we don't use npm or browserify etc.) We also vendor all dependencies so any historic git commit should be buildable without any external data.

Eventually we want to self host our git repos in Peergos itself. 