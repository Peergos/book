# Self hosting

Peergos is fully self hostable. You can run peergos from your own home or server to obtain as much storage and bandwidth as you need, whilst still transparently interacting with anyone using any other server. Because the server only ever sees encrypted data you can also tell it to directly store your data in a standard cloud storage provider like Backblaze or Amazon without any loss of privacy.

To self host, we recommend at least 2 CPU cores and 2 GiB of RAM. You can use a local file backed blockstore, or an S3 compatible service. You can use local sqlite db, or postgres compatible. We recommend sticking with sqlite, even if you plan on having 1000s of users. The database is very small (~10MB per 1000 users). 

Follow the instructions [here](https://github.com/peergos/peergos?tab=readme-ov-file#usage---self-hosting).

<img alt="Self host at home or on your own server" src="/img/self-host.svg" class="center" style="width: 100%;" />