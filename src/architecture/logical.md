# Logical

The logical architecture of Peergos consists of the following:

1) Content addressed storage: a data store with a mapping from the hash of a block of data to the data itself
2) Mutable pointers: a mapping from a public key to a hash
3) PKI: a global append only log for the username <==> {identity public key, storage public key} mappings
4) Social: each user designates a server for sending follow requests for users to (the server can't see the source user). This is the same as the storage server for that user and is identified and contacted via its public key. 

<img alt="Logical Architecture" src="/img/logical-arch.svg" class="center" style="width: 100%;" />