# Writing subspaces

Each user has a randomly generated writing key pair which controls writes to their filesystem. They can create new writing key pairs for any subtree, for example when granting write access to a file or folder. If desired, a given writing key can be quota controlled, to prevent users to which you've granted write access to a file from filling your data store.

Everying signing keypair, including your identity keypair your root writing keypair map to a data structure called WriterData. A WriterData can contain merkle links to roots of merkle champs and various public keys. The full data structure is listed below. If any of the properties are empty they do not contribute to the size of the serialized WriterData. 

```java
// the public signing key controlling this subspace
PublicKeyHash controller;

// publicly readable and present on owner keys
Optional<SecretGenerationAlgorithm> generationAlgorithm;

// This is the root of a champ containing publicly shared files and folders (a lookup from path to capability)
Optional<Multihash> publicData;

// The public boxing key to encrypt follow requests to
Optional<PublicKeyHash> followRequestReceiver;

// Any keys directly owned by the controller, that aren't named
Set<PublicKeyHash> ownedKeys;

// Any keys directly owned by the controller that have specific labels
Map<String, PublicKeyHash> namedOwnedKeys;

// Encrypted entry points to our and our friends file systems (present on owner keys)
Optional<UserStaticData> staticData;

// This is the root of a champ containing the controller's filesystem (present on writer keys)
Optional<Multihash> tree;
```