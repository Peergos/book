# Custom Apps

Peergos Apps are a way to extend the Peergos platform to add custom functionality


## Use cases:
1. Media Player App. The App should appear as a context menu item when a media file is selected on the Drive screen.

1. Word Processor App. As well as having read access to a document file, the App should be able to overwrite the contents of the document file.

1. Image Gallery App. The App should be able to read image files from the selected Folder tree.

1. White Board App. App will appear on the Launcher page. App can create, retrieve, update, append and delete files within it’s own App space.
    

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


## Peergos REST API

The following endpoints are available:

/peergos-api/v0/data/path.to.file – The data folder is where the App can store and retrieve files

/peergos-api/v0/form/path.to.file – An app can POST a HTML Form and have the results stored in a file of the same name in the data folder.

If an App is launched from a file/folder context menu item, the path of the file/folder will be available via:

```js
let url = new URL(window.location.href);
let filePath = url.searchParams.get("path");
```

 Dark mode can be detected via the theme param
 
 ```js
 let theme = url.searchParams.get("theme");// curent values: ['dark-mode', '']
 ```
 
### The following HTTP actions are supported:

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

## Developing a Peergos App

Select the peergos-app.json file and choose ‘Run App’ to launch the app from the current directory. 

The install process will detect if an existing App has the same name. 
It will display the version of the already installed App. 

During install the App's files are copied to an internal folder. Any existing contents in the assets folder will be replaced. 
The contents of the data folder will be added to.  
The previously installed peergos-app.json file is copied to the App’s data directory as ‘peergos-app-previous.json’. 

