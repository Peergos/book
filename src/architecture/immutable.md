# Immutable data

The immutable data store is provided by IPFS and allows anyone to retrieve any cipher text from its hash through any Peergos node. Note that IPFS is used in a fully trustless manner. Every single hash and signature is checked client side during reads and writes. The underlying storage can be provided by the local harddisk or any cloud provider without loss of privacy.

The interface for this storage is call ContentAddressedStorage, with the following methods:

```java
/**
 *
 * @return The identity (hash of the public key) of the storage node we are talking to
 */
CompletableFuture<Multihash> id();

/**
 *
 * @param owner
 * @return A new transaction id that can be used to group writes together and protect them from being garbage
 * collected before they have been pinned.
 */
CompletableFuture<TransactionId> startTransaction(PublicKeyHash owner);

/**
 * Release all associated objects from this transaction to allow them to be garbage collected if they haven't been
 * pinned.
 * owner
 * @param tid
 * @return
 */
CompletableFuture<Boolean> closeTransaction(PublicKeyHash owner, TransactionId tid);

/**
 *
 * @param owner The owner of these blocks of data
 * @param writer The public signing key authorizing these writes, which must be owned by the owner key
 * @param signatures The signatures of each block being written (by the writer)
 * @param blocks The blocks to write
 * @param tid The transaction to group these writes under
 * @return
 */
CompletableFuture<List<Multihash>> put(PublicKeyHash owner, PublicKeyHash writer, List<byte[]> signatures, List<byte[]> blocks, TransactionId tid);

/**
 *
 * @param hash
 * @return The data with the requested hash, deserialized into cbor, or Optional.empty() if no object can be found
 */
CompletableFuture<Optional<CborObject>> get(Multihash hash);

/**
 * Write a block of data that is just raw bytes, not ipld structured cbor
 * @param owner
 * @param writer
 * @param signatures
 * @param blocks
 * @param tid
 * @return
 */
CompletableFuture<List<Multihash>> putRaw(PublicKeyHash owner, PublicKeyHash writer, List<byte[]> signatures, List<byte[]> blocks, TransactionId tid);

/**
 * Get a block of data that is not in ipld cbor format, just raw bytes
 * @param hash
 * @return
 */
CompletableFuture<Optional<byte[]>> getRaw(Multihash hash);

/**
 * Update an existing pin with a new root. This is useful when modifying a tree of ipld objects where only a small
 * number of components are changed
 * @param owner The owner of the data
 * @param existing The present root hash
 * @param updated The new root hash
 * @return
 */
CompletableFuture<List<MultiAddress>> pinUpdate(PublicKeyHash owner, Multihash existing, Multihash updated);

/**
 * Recursively pin all the objects referenced via ipld merkle links from a root object
 * @param owner The owner of the data
 * @param hash The root hash of the merkle-tree
 * @return A list of the multihashes pinned
 */
CompletableFuture<List<Multihash>> recursivePin(PublicKeyHash owner, Multihash hash);

/**
 * Recursively unpin a merkle tree of objects. This releases the objects to be collected by garbage collection
 * @param owner The owner of the data
 * @param hash The root hash of the merkle-tree
 * @return
 */
CompletableFuture<List<Multihash>> recursiveUnpin(PublicKeyHash owner, Multihash hash);

/**
 * Get all the merkle-links referenced directly from this object
 * @param root The hash of the object whose links we want
 * @return A list of the multihashes referenced with ipld links in this object
 */
CompletableFuture<List<Multihash>> getLinks(Multihash root);

/**
 * Get the size in bytes of the object with the requested hash
 * @param block The hash of the object
 * @return The size in bytes, or Optional.empty() if it cannot be found.
 */
CompletableFuture<Optional<Integer>> getSize(Multihash block);
 ```