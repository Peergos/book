# Logical

The logical architecture of Peergos consists of the following:

1) a global append only log for the username <==> public key mappings
2) a server for sending follow requests for users to (the server can't see the source user)
2) a mapping from a public key to a hash (the mutable pointers)
3) a mapping from a hash to the (encrypted) data it is the hash of (the data store)