# Custom Apps

Peergos Apps are a way to extend the Peergos platform to add custom functionality

When an app is run, its HTML5 assets are rendered in a unique hostname (sha256(app path).$peergos-domain) of the peergos server, e.g. https://bciqjmdntozhuanb2c3ka5vtqpux75j5symbyomhkpnilndngl6iaspy.peergos.net. The app domain is isolated from the main peergos domain in a separate OS process, and from other apps. The app domain is also locked down with CSP http headers so it cannot make any external requests which could be used to exfilrate data [0]. Requests made by the app are intercepted in a service worker and translated to post messages which are sent to the main peergos tab. That is where the requests are checked for validity and permissions are enforced. By default, an app has no permissions and can only read its own assets. Running an app also doesn't reveal its assets to the server - they are served via a service worker and post messages to the main peergos tab, and thus benefit from all the existing privacy protections in Peergos.

[0] This is currently not true until browsers implement [webrtc CSP](https://github.com/w3c/webappsec-csp/issues/92) which blocks any webrtc connections. Issue for this are [firefox](https://bugzilla.mozilla.org/show_bug.cgi?id=1783489), [Chrome](https://bugs.chromium.org/p/chromium/issues/detail?id=1225968). So only install apps from authors you trust for now, unless they don't require any permissions which is safe.

## Use cases:
1. Media Player App. The App should appear as a context menu item when a media file is selected on the Drive screen.

1. Word Processor App. As well as having read access to a document file, the App should be able to overwrite the contents of the document file.

1. Image Gallery App. The App should be able to read image files from the selected Folder tree.

1. White Board App. App will appear on the Launcher page. App can create, retrieve, update, append and delete files within it’s own App space.
    
## Example apps
You can find some example apps here: [https://github.com/Peergos/example-apps](https://github.com/Peergos/example-apps)

## Anatomy of a Peergos App

An app consists of plain HTML/Javascript/CSS packaged in a folder

The App is described by a mandatory manifest file called peergos-app.json

### App Folder Structure

asssets					- Must contain index.html as an entry point

data					- Files under the control of the App

peergos-app.json		- manifest file 
    

### Peergos-app.json

This file describes the App. It also indicates the permissions required for the App to function

Fields:

schemaVersion	- Currently always set to 1

displayName		- Used for display.  Limited to 25 characters. (alphanumeric plus dash and underscore).

version			- Format of Major.Minor.Patch-Suffix. Example: 0.0.1-initial

description		- Text. Length must not exceed 100 characters

author			- Text. Length must not exceed 32 characters
    		
fileExtensions	- Array of target file extensions e.g. ["jpg","png","gif"]

mimeTypes		- Array of target mime types e.g. ["application/zip","application/vnd.peergos-todo","video/quicktime"]

fileTypes		- Another way to target files e.g. [“image”, “video”, “audio”, “text”]

launchable		- Indicates App can be opened on the Launcher page

folderAction	- Indicates App acts on folders

appIcon			- filename of image to use as icon on launcher page. Must be available in assets folder

permissions		- see below

Permissions:

STORE_APP_DATA	- Can store and read files in a folder private to the app

EDIT_CHOSEN_FILE – Can modify file chosen by user

READ_CHOSEN_FOLDER – Can read contents of folder chosen by user

EXCHANGE_MESSAGES_WITH_FRIENDS - Can exchange messages with friends

CSP_UNSAFE_EVAL - Allow app to modify its own code via calls to eval()


A minimal peergos-app.json file would look like:

```js
{
    "displayName": "App",
    "description": "does something",
    "launchable": true  	
}
```

These are already quite powerful, but we plan to add more permissions as we see more use cases.

## Peergos REST API

The following endpoints are available:

/peergos-api/v0/data/path.to.file – The data folder is where the App can store and retrieve files

/peergos-api/v0/form/path.to.file – An app can POST a HTML Form and have the results stored in a file of the same name in the data folder.

/peergos-api/v0/chat/ - An app can use the chat api for communication between friends.

If an App is launched from a file/folder context menu item, the path of the file/folder will be available via:

```js
let url = new URL(window.location.href);
let filePath = url.searchParams.get("path");
```

 Dark mode can be detected via the theme param
 
 ```js
 let theme = url.searchParams.get("theme");// curent values: ['dark-mode', '']
 ```
 
### Drive - The following HTTP actions are supported:

Note: /peergos-api/v0/data/ is only relevant for the App's data folder. It is not necessary when referencing a file in the App's assets folder or the folder/file selected by the user. 

GET – Retrieve a resource. Can be a file or folder

Response code: 200 – success. 

404, 400 – request failed

Notes:

1. If resource is a folder the response will look like: {files:[“file1.txt”, “file2.txt”], subFolders:[“folder”]}

1. If the file is a media file with a thumbnail, provide ?preview=true to the request to have the thumbnail returned in the Response as a Base64 string.

POST – Create a resource

Response code: 201 – create success. See Response header field: location

200 for POST using form/

400 – request failed

PUT – update a resource

Response code: 201 – create success. See Response header field: location

200 – for update success

400 – request failed

DELETE – delete a file

204 – delete success

400 – request failed

PATCH – append to a file only supported

204 – Append success. See Response header field: Content-Location

400 – request failed


PUT|POST - save a file (launches a dialog)

/peergos-api/v0/save/filename.txt

Request body set to contents of file to save

Response code: 200 or 201 – success.


GET - launch folder picker

/peergos-api/v0/folders

Response code: 200 and an array of the selected paths.



### Chat - The following HTTP actions are supported (see chat-api in example-apps):

GET – Retrieve a list of all chats created by this App

/peergos-api/v0/chat/

Response code: 200 – success. 

Response:

{chatId: chatId, title: title}



GET – Retrieve chat messages

/peergos-api/v0/chat/:chatId

Url Parameters:

from - paging from index

to - paging to index

Response code: 200 – success.

Response:

{messages:[], count: messagesRead}

Contents of messages array:

{type: 'Application', id: messageHash, text: text, author: author, timestamp: timestamp}

{type: 'Join', username: username, timestamp: timestamp}


POST - create chat

/peergos-api/v0/chat/

Request FormData

parameters:

maxInvites - Numeric

Response code: 201 – success.

Response header:

Location - chatId



PUT - send message

/peergos-api/v0/chat/:chatId

Request FormData

parameters:

text - Contents of message

Response code: 201 – success.



## Developing a Peergos App

Select the peergos-app.json file and choose ‘Run App’ to launch the app from the current directory. 
This is only available for launchable apps.

Tip: During development, set the launchable property to get the fast dev cycle feedback. 

The install process will detect if an existing App has the same name. 
It will display the version of the already installed App. 

During install the App's files are copied to an internal folder. Any existing contents in the assets folder will be replaced. 
The contents of the data folder will be added to.  
The previously installed peergos-app.json file is copied to the App’s data directory as ‘peergos-app-previous.json’. 


