# Metadata

All of the metadata for a given file is also encrypted, with a different symmetric key. This includes the name for directories and also the filesize, modification time, any thumbnail and mime type for files. The size if files is further hidden by splitting files into 5MiB chunks and storing each chunk under a random label (along with those for all other files owned by the same user).

The metadata around access patterns is hidden by hosting files behind a tor hidden service. This ensures that when one user reads a file shared with them by a friend this access does not leak to the network the fact that they are friends. 