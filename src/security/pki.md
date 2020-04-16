# Public Key Infrastructure

All users have a public identity key, and these are stored in a merkle-champ. This structure is mirrored by all nodes (and will eventually be sharded). This includes the user's claim to a username along with an expiry, and their current storage server. The effect is similar to certificate transparency logs. 

This allows users to do public key lookups without leaking to the network who they are looking up. Users also store the keys of their friends in their own filesystem in a TOFU setup. This means that ordinary usage doesn't involve looking up keys from the public blockchain. 