# Capabilities

There are four kinds of capabilities in Peergos: Mirror, Restricted read, Read and Write.

<img alt="Capability types" src="/img/cap-types.svg" class="center" style="width: 70%;" />

The owner is a public key, which is used to lookup the host public key from the PKI. The writer is a public key for the mutable pointer. The map key is 32 bytes which form the lookup key in the champ, the BAT is 32 bytes which are used to control access to the ciphertext by the server. The read and write keys are 256 bit symmetric keys.

With a mirror capability you are able to retrieve the raw ciphertext of a chunk. The read capability allows you to decrypt and read the chunk, and the write key allows you to modify the file/directory.

A restricted read capability holds the restricted key derived from the read key, rather than the read key itself. It reads the file or directory and everything below it just as a read capability does, but can't follow parent links, so it doesn't reveal where the item is (see [access control](/security/cryptree.html)). Since the restricted key is derived from the read key, a read capability can always be turned into a restricted one, but never the reverse. A write key can be paired with either, which is how a writable secret link can still hide where its items are.
