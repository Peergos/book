# Proxying requests

Any modifying request needs to be proxied to the correct destination server. This could be signing up, uploading a file, or sending a follow request. This is achieved using an ipfs p2p stream. In particular, because all these requests are http requests, we use the http p2p proxy exposed locally on the ipfs gateway. It means we can send any request to

http://localhost:8080/p2p/$target_node_id/http/$path

and it will go through an end to end encrypted stream through the ipfs network to the destination node, which then sends it to the local Peergos server at:

http://localhost:8000/$path

This is illustrated below:
<img alt="Proxying a request through ipfs" src="/img/proxy.svg" class="center" style="width: 100%;" />