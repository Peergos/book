# Migration

Your identity in Peergos is not tied to any particular server. Compared to other federated social networks where moving server typically involves losing your social network and meta-data, if not data too, Peergos allows you to transparently migrate between servers and storage providers without any action required from your friends and without any data loss. We pioneered this account and data portability in 2019. 

This means, for example, you could start out by creating an account on your device which gives you limited storage and uptime, then effortlessly migrate to a paid server, or to your own server, when you realise how awesome Peergos is.

### Migrate using the web ui
1. Follow the instructions above to mirror your data
2. Once your Mirror status is 100%, click "Migrate to this server"

This server will then become your primry home server. Your previous server will continue to mirror your data until you cancel your storage there. 

### Migrate using the CLI

To move an account to a new server, ensure you have enough quota on the destination then run, on the destination server:

> java -jar Peergos.jar migrate

It will ask for your username and password, mirror all your data locally, and then update the PKI so this is your new home server. Your identity, links and social graph are all preserved - no one you've shared with needs to do anything. See also the [migration](./migration.md) page.
