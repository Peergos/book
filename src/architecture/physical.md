# Physical

Each user must have at least one Peergos server (which includes an instance of IPFS). This server stores their data, their mutable pointers and any pending follow requests for them. There is also the global append only log for the PKI which is mirrored on every node. Communication between IPFS instances is done over encrypted TLS 1.3 streams. 

<img alt="The physical architecture" src="/img/physical-arch.svg" class="center" style="width: 100%;" />