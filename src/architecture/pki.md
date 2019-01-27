# Usernames

The public keys and usernames are stored in a global append only data structure, with names taken on a first come first served basis. This needs consensus to ensure uniqueness of usernames. This is also where the ipfs node id of the server(s) responsible for synchronising the user's writes is stored. The public key infrastructure (pki) server is called the Corenode, and its interface is the following.

```java
/**
 *
 * @param username
 * @return the key chain proving the claim of the requested username and the ipfs node id of their storage
 */
CompletableFuture<List<UserPublicKeyLink>> getChain(String username);

/** Claim a username, or change the public key owning a username
 *
 * @param username
 * @param chain The changed links of the chain
 * @return True if successfully updated
 */
CompletableFuture<Boolean> updateChain(String username, List<UserPublicKeyLink> chain);

/**
 *
 * @param key the hash of the public identity key of a user
 * @return the username claimed by a given public key
 */
CompletableFuture<String> getUsername(PublicKeyHash key);

/**
 *
 * @param prefix
 * @return All usernames starting with prefix
 */
CompletableFuture<List<String>> getUsernames(String prefix);
```