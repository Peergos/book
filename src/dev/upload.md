# Uploading a file

A file upload proceeds in the following steps

1) Check filename is valid and free

2) Create a transaction file with a plan for the upload

3) For every section of the file which is up to 5 MiB:
   - Encrypt the 5 MiB file section with a random symmetric key
   - Split the cipher text into 128 KiB fragments
   - Create a FileAccess cryptree node with merkle links to all the resulting fragments and an encrypted link to the next section (even if there isn't a next section)
   - Add the FileAccess to the champ of the writing key pair under a random 32 byte label

4) Add a cryptree link from the parent directory to the file

5) Delete the transaction file

A modification, such as uploading a file, can be done through any Peergos server as the writes are [proxied](/dev/proxy.html) through an ipfs p2p stream to the owner's storage ipfs node. 
