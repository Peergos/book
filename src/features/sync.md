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

6. Click on "Add folder to sync"
7. Follow the instructions to select a pre-existing peergos folder and a local folder and whether to sync deletes.

8. You will start by choosing the local folder to sync. You will see a drive selection box and a folder selection box. You will see a [+] sign next to a local folder that has subdirectories. 
   <img alt="The sync page" src="/img/sync-local.jpg" class="center" style="width: 80%;" />

9. To choose a different drive, click the drive selection box and you will get a list of available drives. Click the one you want.

<img alt="The sync page" src="/img/sync-drive.jpg" class="center" style="width: 100px;" />

10. Scroll the directory list until you find the one you want. select the checkbox for that folder to choose it for sync.

<img alt="The sync page" src="/img/sync-select.jpg" class="center" style="width: 50%;" />

11. Click the Done button to go to the next stage which will allow you to select the desired remote Peergos folder using the same process.

<img alt="The sync page" src="/img/sync-remote.jpg" class="center" style="width: 80%;" />

12. Click the done button to go to the next stagw which will allow you to selection the behaviour for deletions.

13. Select the desired behaviour for deletes and click confirm. Peergos will then create the sync pair and start displaying the sync status. Syncing local deletes means if you delete a local file in the synced directory then it will be deleted in the peergos directory. Syncing remote deletes means if you delete a file in the peergos folder it will be deleted in the local folder.

<img alt="The sync page" src="/img/sync-status.jpg" class="center" style="width: 80%;" />
You can close your browser now. The app does the syncs in the background. 

To stop syncing a folder, click on the "Stop syncing folder" button. 


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