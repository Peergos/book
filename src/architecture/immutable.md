# Immutable data

The immutable data store is provided by IPFS and allows anyone authorised to retrieve cipher text from its hash through any Peergos node. Note that IPFS is used in a fully trustless manner. Every hash and signature is checked client side during reads and writes. The underlying storage can be provided by the local harddisk or any S3 compatible object storage without loss of privacy. Access to raw blocks is controlled using S3 V4 signatures from an allowed [Block Access Token](/security/bats.html) (BAT). Each block specifies which BATs are allowed to retrieve it. Any node that retrieves such a block enforces the same auth on it. 

The interface for this storage is ContentAddressedStorage, with the following methods:

```java

     /**
     *
     * @return The identity (hash of the public key) of the storage node we are talking to
     */
    CompletableFuture<Cid> id();

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
     * @param owner
     * @param tid
     * @return
     */
    CompletableFuture<Boolean> closeTransaction(PublicKeyHash owner, TransactionId tid);

    /**
     *
     * @param owner The owner of these blocks of data
     * @param writer The public signing key authorizing these writes, which must be owned by the owner key
     * @param signedHashes The signatures of the sha256 of each block being written (by the writer)
     * @param blocks The blocks to write
     * @param tid The transaction to group these writes under
     * @return
     */
    CompletableFuture<List<Cid>> put(PublicKeyHash owner,
                                     PublicKeyHash writer,
                                     List<byte[]> signedHashes,
                                     List<byte[]> blocks,
                                     TransactionId tid);


    /**
     *
     * @param hash
     * @return The data with the requested hash, deserialized into cbor, or Optional.empty() if no object can be found
     */
    CompletableFuture<Optional<CborObject>> get(Cid hash, Optional<BatWithId> bat);

    /**
     * Write a block of data that is just raw bytes, not ipld structured cbor
     * @param owner
     * @param writer
     * @param signedHashes
     * @param blocks
     * @param tid
     * @param progressCounter
     * @return
     */
    CompletableFuture<List<Cid>> putRaw(PublicKeyHash owner,
                                        PublicKeyHash writer,
                                        List<byte[]> signedHashes,
                                        List<byte[]> blocks,
                                        TransactionId tid,
                                        ProgressConsumer<Long> progressCounter);

    /**
     * Get a block of data that is not in ipld cbor format, just raw bytes
     * @param hash
     * @return
     */
    CompletableFuture<Optional<byte[]>> getRaw(Cid hash, Optional<BatWithId> bat);

    CompletableFuture<List<byte[]>> getChampLookup(PublicKeyHash owner, Cid root, byte[] champKey, Optional<BatWithId> bat);
 ```