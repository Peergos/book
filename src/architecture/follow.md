# Follow requests

A user's storage server stores their pending follow requests until they are retrieved and deleted. These are not actually stored in ipfs itself, and reading them is guarded by a challenge protocol to mitigate against someone logging them all now and decrypting them with a large quantum computer when one is built. 

Follow requests contain no unencrypted data visible to the network, or server, apart from the target user. Only the target user can decrypt the follow request to see the sender.

The interface for sending, receiving and removing follow requests is called SocialNetwork and has the following methods:

```java
/** Send a follow request to the target public key
 *
 * @param target The public identity key hash of the target user
 * @param encryptedPermission The encrypted follow request
 * @return True if successful
 */
CompletableFuture<Boolean> sendFollowRequest(PublicKeyHash target, byte[] encryptedPermission);

/**
 *
 * @param owner The public identity key hash of user who's pending follow requests are being retrieved
 * @param signedTime The current time signed by the owner
 * @return all the pending follow requests for the given user
 */
CompletableFuture<byte[]> getFollowRequests(PublicKeyHash owner, byte[] signedTime);

/** Delete a follow request for a given public key
 *
 * @param owner The public identity key hash of user who's follow request is being deleted
 * @param data The original follow request data to delete, signed by the owner
 * @return True if successful
 */
CompletableFuture<Boolean> removeFollowRequest(PublicKeyHash owner, byte[] data);
```