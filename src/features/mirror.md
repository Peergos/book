# Mirroring your data to another server
This will keep an uptodate copy of all your data on another server.

## Using the web ui
1. Browse to a server like https://peergos.net
2. Click on the "Mirror" tab and pay or request enough storage.
3. Login and click on the User menu in the top right
4. Select Migrate
5. Click on "Mirror your data on this server"
6. Click on "Mirror your login data on this server"

This will provide a constantly updated copy of your data on this server.

## Using the CLI
On any instance, run:

> java -jar Peergos.jar mirror init -username $username -peergos-url https://YOUR_PEERGOS_SERVER_DOMAIN

It will ask for your password and print three parameters. Pass them to the daemon on the machine doing the mirroring:

> java -jar Peergos.jar daemon -mirror.username $username -mirror.bat $mirrorBat -login-keypair $loginKeypair

That instance will then continuously mirror the user's data.

## Mirroring an entire server
A server admin can keep an up to date copy of every user whose home is a given server (their data, login data, mirror BATs and secret link counts) on another server.

1. On any instance, generate a new instance BAT:

> java -jar Peergos.jar admin bat generate

This prints a line like `Generate new BAT and id: $instanceBat`.

2. Restart the source server's daemon with this BAT added to its existing arguments:

> java -jar Peergos.jar daemon ... -instance-bat $instanceBat

Keep the BAT secret: anyone who has it can request a snapshot of every user on the source server.

3. Get the node id of the source server:

> curl -X POST https://YOUR_SOURCE_SERVER_DOMAIN/api/v0/id

This returns JSON like `{"ID":"12D3KooW..."}`. The value of `ID` is `$sourceNodeId`.

4. Start the daemon on the mirror server with these additional arguments:

> java -jar Peergos.jar daemon ... -mirror.node.id $sourceNodeId -mirror-instance-bat $instanceBat

The mirror server will then copy all users of the source server, and repeat this once a day. Change the interval with `-server-mirror-period-seconds`. If a run has errors, it retries after 30 seconds.
