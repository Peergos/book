# Specification

The wire protocol is standard libp2p. This is specified [here](https://github.com/libp2p/specs).

The serialization format for blocks is [dag-cbor](https://ipld.io/specs/codecs/dag-cbor/spec/), or raw (unformatted).

Merkle links are encoded as Cids, whose specification is [here](https://github.com/multiformats/cid).

Public signing keys are encoded in cbor, with format specified [here](https://github.com/Peergos/Peergos/blob/master/src/peergos/shared/crypto/asymmetric/curve25519/Ed25519PublicKey.java#L44).

Public encryption keys are encoded in cbor, with format specified [here](https://github.com/Peergos/Peergos/blob/master/src/peergos/shared/crypto/asymmetric/curve25519/Ed25519PublicKey.java#L44).

The cryptree+ structure is specified [here](https://github.com/Peergos/Peergos/blob/master/src/peergos/shared/user/fs/cryptree/CryptreeNode.java).

The server API is specified [here](https://github.com/Peergos/Peergos/tree/master/src/peergos/server/net)

The Compressed Hash-Array Mapped Prefix-Tree (CHAMP) structure is specified and implemented [here](https://github.com/Peergos/Peergos/blob/master/src/peergos/shared/hamt/Champ.java).

The local (service worker based) REST API for applications is described [here](/features/apps.html).