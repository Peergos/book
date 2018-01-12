# Quantum resistance

Peergos aims to be a long term secure file storage system, and hence we have architected it with an awareness of quantum computer based attacks (many of us are ex physicists).

Files that you store but don't share with anyone are already resistant to quantum computer based attacks. This is because the process from logging in to decrypting them only involves hashing and symmetric encryption, neither of which are significantly weakened by a quantum computer.

Files that have been shared are currently vulnerable to a quantum computer attack because they use asymmetric elliptic curve cryptography (Curve25519) to share the decryption capability. However, we plan to upgade to a suitable post-quantum algorithm soon. 