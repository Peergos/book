# Metadata

All of the metadata for a given file is encrypted, with a different symmetric key from the file itself. This includes the name for directories and also the filesize, modification time, any thumbnail and mime type for files. The size of files is further hidden by splitting files into 5MiB chunks, which are padded to a multiple of 4 KiB, and storing each chunk under a random label (along with those for all other files owned by the same user and controlled by the same writing key pair).

Where a file or folder is in your drive is metadata too. Someone you share an item with directly can see the names of the folders above it, but a secret link can hide that, by giving out [restricted keys](/security/cryptree.html#full-and-restricted-keys) which can only read down the tree.

The metadata around access patterns will be hidden by hosting files behind a tor hidden service once Tor is integrated. This will ensure that when one user reads a file shared with them by a friend this access does not leak to the network the fact that they are friends. 