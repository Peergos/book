# Mutable

Mutable pointers in Peergos are just a mapping from a public key to a root hash. Clearly, being mutable, they need some kind of synchronization or concurrent data structure. Each user lists a Tor hidden service which is responsible for synchronising their writes and publishing the latest root hashes. This means the global filesystem is sharded by username and each user can use a Tor hidden service with sufficient capability for their bandwidth requirements.

Initially each user's file system is under a single public key. Additional keys are generated when granting write access. 