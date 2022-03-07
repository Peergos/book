# Block Access Control

When an app on IPFS wants a block of data, it asks IPFS for the data corresponding to its content identifier, or [CID](https://docs.ipfs.io/concepts/content-addressing/#identifier-formats) (basically, a hash of the data). IPFS will then search the global IPFS network for nodes that have this CID. At the same time, it will ask any nodes it's already in contact with: "Do you have this CID?". Any contacted node that has the block can respond with the data. A nice property of this is that any node that has the content can serve it up, which means that it autoscales to demand. 

<center>
![authed](/img/bitswap-authed.png)
<br/>
Authed bitswap retrieving a block.
</center>

We have extended this protocol to have an optional auth string paired with every CID. In Peergos, this auth string is an [S3 V4 signature](https://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-header-based-auth.html), which is time-limited, includes the CID, and is tied to the requesting node's public key (to prevent replay attacks). A replay attack would be if it were possible for someone without the block, who we had sent a valid auth token, to retrieve the block themselves directly using the token. As an anology, consider a ticketed event. If someone buys a ticket, and then a friend of theirs copies the ticket and uses that to gain entry, that is a replay attack. If, however, the tickets included the buyer's name on the ticket (they were non-transferable) and the event verified the holder's name on entry then the friend couldn't get in, even with the original ticket. 

We do a similar thing to avoid this by using the source node's public key as the domain in the S3 request. This way we can broadcast a cid and auth string to the network and no one but us can use that auth string. The S3 V4 signature scheme is essentially repeated [hmac-sha256](https://en.wikipedia.org/wiki/HMAC) and needs a secret key to function. Such a secret key would grant the holder access to the block, so we call it a Block Access Token or BAT for short, and each is 32 bytes long. Since it only depends on hmac-sha256, which itself only depends on sha256, it is post-quantum - a large quantum computer does not break it. 

The primary BAT used for this authentication is derived from the block itself. This means any instance that retrieves such a block (after being authorised) can continue to serve it up and enforce the same access control, thus maintaining the autoscaling properties in a privacy-preserving way.

There are two formats of blocks in Peergos, cbor and raw. Raw blocks are the most sensitive (they hold users' encrypted data) and are just fragments of ciphertext with no additional structure. Cbor blocks are valid [dag-cbor](https://ipld.io/docs/codecs/known/dag-cbor/) structured IPLD objects which can reference other blocks. How could we put a BAT in these blocks? In a cbor block, it is easy to choose a canonical place to put a list of BATs. If the cbor is a map object, we put a list of BATs at the top level under the key "bats".

<center>
![cbor-block-auth](/img/cbor-bats.png)
<br/>
Structure for storing BATs in cbor blocks
</center>

For raw objects, it is a little more difficult, as we also need to support raw blocks that do not have a BAT (either legacy blocks or ones specifically made public). Our design uses a detectable prefix of 8 FIXED bytes followed by a cbor list of BATs before the actual ciphertext of the block.

<center>
![raw-bock-auth](/img/raw-bats.png)
<br/>
Structure for storing BATs in the prefix of a raw block
</center>

We normally have two bats per block. One is inline - and specific to that block only. The other is a user wide "mirror" BAT - and referenced in the block by its hash. The mirror BAT is for when a user wants to mirror all their data on another instance, or migrate to another instance. 

| | Chunk 1                |  Chunk 2     | Chunk 3 |
|-|-------------------------|-----------------|---------|
| BAT stream secret | Sb (encrypted in base data)  |                    |    |
| BAT[] (unencrypted in root cbor object under "bats") | B1=randomBytes(32)                | B2=hash(Sb + B1) | B3=hash(Sb + B2) |

<center>
BAT derivation for subsequent chunks of a file
</center>

Each 5 MiB chunk of a file or directory has its own unique BAT, so the server still cannot link the different blocks of a file to deduce the padded size of the file. Subsequent chunk BATs within a file are derived in the same way as we do the CHAMP labels, by hashing the current chunk BAT with a stream-secret, stored encrypted in the first chunk. This maintains our ability to seek within arbitrarily large files without any IO operations (just local hashing and then a final lookup of the requested chunk). When someone's access to a file or directory is revoked, the BATs are also changed, making it impossible to retrieve the new ciphertext even with previous access.