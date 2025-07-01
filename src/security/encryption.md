# Encryption

All your files are encrypted symmetrically with a random 256-bit key using salsa20+poly1305 (from TweetNaCl). These keys are not derived from the contents of the file (as some services do) because this leaks to the network which files you are storing. Files are split into chunks of up to 5 MiB, padded to a multiple of 4 KiB, and each chunk is independently encrypted. Different chunks of a file are not linkable unless you have a read capability for that file.

Sharing files with other users is done through follow requests. Follow requests are asymmetrically encrypted with a hybrid of X25519 and MLKEM. This means all files are safe against exposure to a current or future large quantum computer, should one be built. 