# Signing up

The steps involved in signing up are:

1) Register the username
   - Hash the password and username through scrypt to get the auth key pair, and symmetric root key.
   - Generate a random identity keypair
   - Generate a signed username claim including an expiry, and the ipfs node id of the storage server (the server we are signing up through) This is just identity.sign(username, expiry, [storage id])
   - Send this claim to the pki node for confirmation

2) Set up your identity
   - Write the public identity key to ipfs
   - Write the public following key to ipfs
   - Create a [WriterData](/architecture/writer.html) for the identity key pair with the two resulting public key hashes
   - Generate a random key pair to control writes to the users filesystem. Add this key pair as an owned key to the identity WriterData.
   - Commit the identity WriterData (write it to ipfs and set the mutable pointer for the identity key pair to the resulting hash). 

3) Set up your filesystem
   - Create a DirAccess [cryptree](/security/cryptree.html) node for the user's root directory, and add this to the champ of the filesystem key pair.
   - Add a write capability to your root dir to your login data (encrypted with the symmetric root key, and only retrievable with the auth key pair)
   - Create the /username/shared directory which is used when sending follow requests