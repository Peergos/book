# Fast seeking

Seeking forward or backward in huge files is both very fast (O(1) in I/O requests) and fully authenticated. When you start to retrieve a file from a capability you first retrieve the cryptree node (encrypted metadata) of the first chunk, which is located at the given champ key (32 random bytes).

In the FileProperties there is a stream secret (32 random bytes). To find the next chunk you calculate sha256(stream secret + current champ key). This means to skip forward X bytes you repeat this, doing X/5MiB sha256 hashes locally. Then finally do a champ lookup at the end to retrieve the chunk you asked for. To seek backward you just keep a reference to the start and seek forwards.

This seeking is both very fast, and doesn't leak to the server which chunks are part of the same file. 