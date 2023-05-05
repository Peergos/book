# Access control

Read access to your files is controlled by a data structure called cryptree, which is essentially a tree of symmetric keys, where the holder of one key can decrypt all the descendant keys. The result is extremely fine grained access control. You can grant access to someone to a file and that user won't be able to see any of the sibling files in the same folder (or even their names - or even their labels in the champ). Granting read access to a folder implies granting read access to all the contents of the folder recursively.

<img alt="Read access capability tree" src="/img/cryptreeplus-read.jpeg" class="center" style="width: 100%;" />

Write access is independently controlled by a similar, but simpler cryptree. All updates to a given subtree are signed by a corresponding writing key pair. When you grant write access to a file or folder then that item is moved to a new writing key pair, to keep the fine grained access control applicable to write access too. This operates independently of the read access control cryptree.

<img alt="Write access capability tree" src="/img/cryptreeplus-write.jpeg" class="center" style="width: 100%;" />