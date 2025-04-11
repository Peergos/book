# File Sync

Peergos has the ability to do standard directory syncing and transparently mount a folder to your host operating system. This is achieved with a FUSE binding (or equivalent for Windows and MacOS). Peergos can do bi-direcitonal syncing between a local folder and a peergos folder. It keep sboth folders in sync including deletes and empty folders. If a large file charges it only copies the changed chunks of the file, rather than the entire file.

To get started with sync, fir create a folder in Peergos. Then run the following (replace peergos with "java -jar Peergos.jar" if you are using the jar instead of the desktop app, or with "peergos-cli" if you are on Windows):

> peergos sync init -peergos-url https://peergos.net

Follow the prompts to enter your username, password and the peergos dir you want to sync with. This will generate a writable secret link to the folder which will be used by the sync process. This will output something like:

> Run the sync dir command with the following args: -links secret/z59vuwzfFDomTEuyeEw7rkofcd2vt5EnVffmAy5fnQe9V9MG36ZiBVY/3615659421#QUq6mf4gz8uk -local-dirs $LOCAL_DIR

Then to run the sync client with:
> peergos sync dir -peergos-url https://peergos.net -links secret/z59vuwzfFDomTEuyeEw7rkofcd2vt5EnVffmAy5fnQe9V9MG36ZiBVY/3615659421#QUq6mf4gz8uk -local-dirs /path/to/local/dir

You can use the same link to sync on multiple different devices at the same time. You can also sync multiple pairs of directories, just use a comma separated list for the links and local-dirs arguments. This will sync the dirs and check for changes every 30s.

For more customisation you can use the following args:

Only do one sync run
> -run-once true

Set the minium local free space allowed as a percentage of the total
> -min-free-space-percent 5
