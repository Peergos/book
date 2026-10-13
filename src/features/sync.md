# File Sync

Peergos can do bi-directional syncing between a local folder and a peergos folder. It keeps both folders in sync including deletes (optional) and empty folders. If a large file changes it only copies the changed chunks of the file, rather than the entire file.

## Web ui instructions
1. Download the desktop or mobile app from https://peergos.org/download
2. Run the app and it should open a login page in your browser to http://localhost:7777
   On some versions of Debian it won't be able to open the browser, in that case just open the URL above yourself.
3. Login or sign up to your account
4. Create a folder in peergos which wil be the sync target
5. Click on the sync icon in the left sidebar
<img alt="The sync page" src="/img/sync.jpg" class="center" style="width: 100%;" />

6. Click on "Add folder"
7. Choose a local folder and a Peergos folder, and whether to sync deletes.

8. You will start by choosing the local folder to sync. Click the arrow next to a folder to see its subfolders.
   <img alt="Choosing a folder on this device" src="/img/sync-local.jpg" class="center" style="width: 80%;" />

9. To choose a different drive, click the drive box at the top and pick the one you want from the list.

<img alt="Choosing a drive" src="/img/sync-drive.jpg" class="center" style="width: 50%;" />

10. Find the folder you want and click the circle next to it to select it.

<img alt="A local folder selected" src="/img/sync-select.jpg" class="center" style="width: 80%;" />

11. Click the Select button to go to the next stage, which lets you choose the Peergos folder in the same way. Below the folders you can choose the behaviour for deletes. Syncing local deletes means if you delete a local file in the synced folder then it will be deleted in the Peergos folder. Syncing remote deletes means if you delete a file in the Peergos folder it will be deleted in the local folder.

<img alt="Choosing a folder in your Drive" src="/img/sync-remote.jpg" class="center" style="width: 80%;" />

12. Click the Select button. Peergos will then create the sync pair and start displaying the sync status.

<img alt="A folder being synced" src="/img/sync-status.jpg" class="center" style="width: 100%;" />
You can close your browser now. The app does the syncs in the background. 

To stop syncing a folder, click on the "Stop syncing" button. 


## CLI instructions

To get started with sync, first create a folder in Peergos. Then run the following (replace peergos with "java -jar Peergos.jar" if you are using the jar instead of the desktop app):

> peergos sync init -peergos-url https://peergos.net

Follow the prompts to enter your username, password and the peergos dir you want to sync with. This will generate a writable secret link to the folder which will be used by the sync process. This will output something like:

> Run the sync dir command with the following args: -links secret/z59vuwzfFDomTEuyeEw7rkofcd2vt5EnVffmAy5fnQe9V9MG36ZiBVY/3615659421#QUq6mf4gz8uk -local-dirs $LOCAL_DIR

Then to run the sync client with:
> peergos sync dir -peergos-url https://peergos.net -links secret/z59vuwzfFDomTEuyeEw7rkofcd2vt5EnVffmAy5fnQe9V9MG36ZiBVY/3615659421#QUq6mf4gz8uk -local-dirs /path/to/local/dir -sync-local-deletes true -sync-remote-deletes true

You can use the same link to sync on multiple different devices at the same time. You can also sync multiple pairs of directories, just use a comma separated list for the links, local-dirs, sync-local-deletes and sync-remote-deletes arguments. This will sync the dirs and check for changes every 30s.

For more customisation you can use the following args:

Only do one sync run
> -run-once true

Set the minimum local free space allowed as a percentage of the total
> -min-free-space-percent 5


To stop a CLI sync just kill the process in the usual way for your OS. 