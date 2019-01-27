# Mutable

Mutable pointers in Peergos are just a mapping from a public key to a root hash. Clearly, being mutable, they need some kind of synchronization or concurrent data structure. Each user lists an ipfs node id (the hash of its public key) which is responsible for synchronising their writes and publishing the latest root hashes. This means the global filesystem is sharded by username and each user can use an ipfs instance (or cluster) with sufficient capability for their bandwidth requirements.

Initially each user's file system is under a single public key. Additional keys are generated when granting write access.

The interface for MutablePointers has the following methods:

```java
/** Update the hash that a public key maps to (doing a cas with the existing value)
 *
 * @param owner The owner of this signing key
 * @param writer The public signing key
 * @param writerSignedBtreeRootHash the signed serialization of the HashCasPair
 * @return True when sucessfully completed
 */
CompletableFuture<Boolean> setPointer(PublicKeyHash owner, PublicKeyHash writer, byte[] writerSignedBtreeRootHash);

/** Get the current hash a public key maps to
 *
 * @param writer The public signing key
 * @return The signed cas of the pointer from its previous value to its current value
 */
CompletableFuture<Optional<byte[]>> getPointer(PublicKeyHash owner, PublicKeyHash writer);
```