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
