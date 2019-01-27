# Merkle-CHAMP

The main network visible data structure in Peergos is a merkle compressed hash array mapped trie, or merkle-champ. All the data under a given writing keypair has its own merkle-champ. This is just a mapping from random 32 byte labels to cipher-text blobs. These blobs are cryptree nodes containing the cryptree data structure, and, in the case of a file section, merkle links to encrypted file fragments. Each 5 MiB section of a file is stored under a different random label in the btree, and similarly with large directories.

<img alt="the network visible merkle-champ" src="img/champ.svg" class="center" style="width: 80%;" />