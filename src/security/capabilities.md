# Capabilities

There are three kinds of capabilities in Peergos: Mirror, Read and Write.

<img alt="Capability types" src="/img/cap-types.png" class="center" style="width: 70%;" />

The owner is a public key, which is used to lookup the host public key from the PKI. The writer is a public key for the mutable pointer. The map key is 32 bytes which form the lookup key in the champ, the BAT is 32 bytes which are used to control access to the ciphertext by the server. The read and write keys are 256 bit symmetric keys.

With a mirror capability you are able to retrieve the raw ciphertext of a chunk. The read capability allows you to decrypt and read the chunk, and the write key allows you to modify the file/directory. 