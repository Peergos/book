# Secret links

A secret link can be generated to point to any file or folder. Anyone with a (Javascript enabled) web browser can view such a link. Secret links can be read-only, writable, time limited, password-protected or n-use. Secret links can be revoked. A secret link doesn't expose the file to the network, or indeed to anyone who doesn't have the link itself (and any password) because the key material isn't sent to the server. A secret link works through any server, including localhost, unless that server blocks external secret links. 

An example of a secret link to a folder is:

[https://peergos.net/secret/z59vuwzfFDovgun2sU9YF8LqJRVXaoVR39XAkvRR6sNp7CJnseecHhV/3647803968#oqmlU2vyq0Fe](https://peergos.net/secret/z59vuwzfFDovgun2sU9YF8LqJRVXaoVR39XAkvRR6sNp7CJnseecHhV/3647803968#oqmlU2vyq0Fe)

The structure of the link is

https://server.com/secret / OWNER_PUBLIC_KEY / LINK_LABEL # LINK_DECRYPTION_PASSWORD

The owner public key is used to lookup the current host of the file owner. We ask that host to look up the link label and the host applies any expiry or n-use restrictions before returning the encrypted link. The password (the fragment of the URL) is then used to decrypt the link to get the capability to the file or folder. The password based encryption on each link is designed to take 100 years to brute force if you have 1,000,000 GPUs, each of which can do 1M scrypt hashes per second.

If you migrate to another server, your links come with you and all pre-existing secret links will continue to work. There is no limit to the number of links you can create, because they are just normal blocks that contribute to your space usage.

## Hiding where items are

By default a secret link hides where its items are. Whoever opens it sees each item under its own name, and can open everything inside it, but sees neither your username nor the folders above it, either in the page or in the link itself. This uses [restricted read keys](/security/cryptree.html#full-and-restricted-keys), which can read down the tree but not follow it back up. You can turn this off when creating a link, in which case the link shows the full path of each item, as a direct share with a friend does.

A link that hides where its items are can still be writable. Anything uploaded or created through it is sealed to the folder it is in, so you can read it as normal, and is marked pending until you share it directly or add it to a link that shows where it is, at which point it gets new keys and its place in the tree. Moving an item through such a link is done as a copy followed by a delete, because a restricted key can't re-point the original.
