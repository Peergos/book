# Uploading a file

A file upload proceeds in the following steps

1) Check filename is valid and free

2) Create a transaction file with a plan for the upload

3) Generate a stream secret for the file (32 random bytes) which is stored in the encrypted file metadata

4) For every section of the file which is up to 5 MiB:
   - Pad plaintext to a multiple of 4096 bytes.
   - Encrypt the padded 5 MiB file section with a random symmetric key
   - Split the cipher text into 1 MiB fragments
   - Create a FileAccess cryptree node with merkle links to all the resulting fragments
   - Add the FileAccess to the champ of the writing key pair (under a random 32 byte label for initial chunks, or sha256(stream secret + previous label))

5) Add a cryptree link from the parent directory to the file

6) Delete the transaction file

A modification, such as uploading a file, can be done through any Peergos server as the writes are [proxied](/dev/proxy.html) through an ipfs p2p stream to the owner's storage ipfs node. 
