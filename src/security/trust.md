# Trust free levels

Peergos is designed at every level to minimise or remove entirely any need for trust.

The first trust barrier is between Peergos and IPFS. A Peergos server verifies the hash of everything written to or read from IPFS. This removes the possibility of tampering at the ipfs level.

The second trust free barrier is between a Peergos client and a Peergos server. A Peergos client verifies the hash of every block read from or written to a Peergos server. Peergos clients also verify the signature of every signed piece of data received from Peergos. This means that once you have obtained a trustworthy copy of a Peergos client you do not need to trust a server to interact with it. 