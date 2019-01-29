# Metadata

All of the metadata for a given file is encrypted, with a different symmetric key from the file itself. This includes the name for directories and also the filesize, modification time, any thumbnail and mime type for files. The size of files is further hidden by splitting files into 5MiB chunks and storing each chunk under a random label (along with those for all other files owned by the same user and controlled by the same writing key pair).

The metadata around access patterns will be hidden by hosting files behind a tor hidden service once Tor is integrated. This will ensure that when one user reads a file shared with them by a friend this access does not leak to the network the fact that they are friends. 