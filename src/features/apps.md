# Custom Apps

Peergos Apps are a way to extend the Peergos platform to add custom functionality

When an app is run, its HTML5 assets are rendered in a unique hostname (sha256(app path).$peergos-domain) of the peergos server, e.g. https://bciqjmdntozhuanb2c3ka5vtqpux75j5symbyomhkpnilndngl6iaspy.peergos.net. The app domain is isolated from the main peergos domain in a separate OS process, and from other apps. The app domain is also locked down with CSP http headers so it cannot make any external requests which could be used to exfilrate data [0]. Requests made by the app are intercepted in a service worker and translated to post messages which are sent to the main peergos tab. That is where the requests are checked for validity and permissions are enforced. By default, an app has no permissions and can only read its own assets. Running an app also doesn't reveal its assets to the server - they are served via a service worker and post messages to the main peergos tab, and thus benefit from all the existing privacy protections in Peergos.

<img alt="App sandbox" src="/img/sandbox.jpeg" class="center" style="width: 100%;" />

[0] This is currently not true until browsers implement [webrtc CSP](https://github.com/w3c/webappsec-csp/issues/92) which blocks any webrtc connections. Browser issues for this are [firefox](https://bugzilla.mozilla.org/show_bug.cgi?id=1783489), [Chrome](https://bugs.chromium.org/p/chromium/issues/detail?id=1225968). So only install apps from authors you trust for now, unless they don't require any permissions which is safe.

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

USE_MAILBOX - Can manage an email mailbox

ACCESS_PROFILE_PHOTO - Can retrieve profile photos shared with you

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


GET - launch file picker

/peergos-api/v0/file-picker
Optional url parameter ?extension="json" to filter files shown in picker.

Response code: 200 and an array containing the selected file path.


### Chat V0 - The following HTTP actions are supported (see chat-api in example-apps):

See Chat V1 below for a more comprehensive API to support more complex apps


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


### Chat V1 - The following HTTP actions are supported (see chat folder in example-apps):

GET – Retrieve chats for current App

/peergos-api/v1/chat/


Response code: 200 – success.

Response:

{chats: [], latestMessages: []}

Contents of chats array:

{chatId: string, title: string, members: [usernames], admins: [usernames] }

Contents of latestMessages array (array entries match corresponding chats array):

{message: string, creationTime: timestamp (localdatetime)}


GET – Retrieve chat messages

/peergos-api/v1/chat/:chatId

Url Parameters:

startIndex - message index

Response code: 200 – success.

Response:

{chatId: string, startIndex: url param from request, messages: array of message json,
  hasFriendsInChat: number of friends in current chat membership}

Where message is 

{ messageRef: string uuid, author: username, timestamp: localdatetime, 

type: can be one of RemoveMember|Invite|Join|GroupState|ReplyTo|Delete|Edit|Application ,

removeUsername: set if type is RemoveMember, inviteUsername: set if type is Invite, joinUsername: set if type is Join,

editPriorVersion: set if type is Edit, deleteTarget: set if type is Delete, replyToParent: set if type is ReplyTo,

text: set if type is Application|Edit|ReplyTo, envelope: base64 encoded opaque object,

groupState: set if type is GroupState, attachments : array of attachment json}

Where GroupState is

{ key: payload.key, value: payload.value}

Where attachment is

{fileRef: FileRef json, mimeType: mimeType of file, fileType: audio|video|image, thumbnail: base64 encoded thumbnail for file}

see FileRef description in API call for /attachment response)


DELETE - delete a chat

/peergos-api/v1/chat/:chatId

Response code: 204 – success.


DELETE - delete an attachment

/peergos-api/v1/chat/filePath

Response code: 204 – success.


POST - launch chat group membership modal in order to create a new chat

/peergos-api/v1/chat/

Response code: 201 – success. 400 - failure or modal closed

Response (location response header field):

{chatId: string, title: string, members: [username], admins: [username]};


POST - launch chat group membership modal in order to modify membership of existing chat

/peergos-api/v1/chat/:chatId

Response code: 200 – success. 400 - failure or modal closed


POST - launch gallery modal to display media attachment

/peergos-api/v1/chat/?view=true

Request body:

byte[] of FileRef json (see fileRef field in API call /attachment response)

Response code: 200


POST - download a media attachment

/peergos-api/v1/chat/?download=true

Request body:

byte[] of FileRef json (see fileRef field in API call /attachment response)

Response code: 200


POST - upload a media attachment

/peergos-api/v1/chat/attachment?filename=filename-of-file-to-upload

Request body:

byte[] of attachment's content

Response code: 201

Response (location response header field):

{fileRef: FileRef json, hasMediaFile: boolean, hasThumbnail: boolean, thumbnail: base64 string of thumbnail image,

fileType: file type string ie audio, image, video, mimeType: mimeType string, size: number }

where FileRef is

{path: absolute path to file, cap: opaque capability object, contentHash: hash of file contents}

PUT - send a message

/peergos-api/v1/chat/:chatId

Request body:

to create message 

{ createMessage : { text: string, attachments: array of FileRef json} }

to edit existing message

{ editMessage : { text: string, messageRef: uuid of message to edit} }

to reply to an existing message

{ replyMessage : { text: string, attachments: array of FileRef json, replyTo: envelope of message to reply to} }

to delete an existing message

{ deleteMessage : { messageRef: uuid of message to delete} }

Response code: 201
                    

### Profile:

GET – Launch the profile modal for the requested Peergos user (must be friend of current user)

/peergos-api/v0/profile/:username

Response code: 200 – success.  400 - failure.


GET – Retrieve the profile thumbnail image for the requested Peergos user (must be friend of current user + app has permission ACCESS_PROFILE_PHOTO)

/peergos-api/v0/profile/:username?thumbnail=true

Response code: 200 – success.  400 - failure.

Response:

{profileThumbnail: base64 data}


### Mailbox: (see email folder in example-apps):

GET – Get mailbox information

/peergos-api/v0/mailbox/

Response code: 200 – success.

{userFolders: array of type Folder, mailboxAddress: email address for user}

Where Folder is:
{name: display string, path: name of internal folder}


GET – Get contents of inbox

/peergos-api/v0/mailbox/inbox

Response code: 200 – success.

{data: array of type Email, folderName: 'inbox', filterStarredEmails: boolean}

Where Email is:
{ id: string, msgId: string, from: string, subject: string, timestamp: string of timestamp
, to: array of string, cc: array of string, bcc: array of string, content: string
, replyingToEmail: optional - of type Email, forwardingToEmail: optional - of type Email
, unread: boolean, star: boolean, attachments: array of type Attachment
, icalEvent: contents of an .ics file};

Where Attachment is:
{filename: string, size: integer, type: mime type string, uuid: string}


GET – Get contents of sent folder

/peergos-api/v0/mailbox/sent

Response code: 200 – success.

See /inbox for description of response


GET – Get contents of another folder [trash, archive, custom folder]

/peergos-api/v0/mailbox/:folderName

Response code: 200 – success.

See /inbox for description of response


DELETE - delete a custom folder

/peergos-api/v0/mailbox/:folderName

Response code: 204 – success.	400 - failure.


POST - move an email from one folder to another

/peergos-api/v0/mailbox/move/?from='srcFolderName'&to='destFolderName'

Response code: 200 – success.	400 - failure.


POST - move an email from one folder to another

/peergos-api/v0/mailbox/move/?from='srcFolderName'&to='destFolderName'

Request body:

either byte[] of singular Email json or Array of multiple Email json

Response code: 200 – success.	400 - failure.


POST - delete an email from a folder

/peergos-api/v0/mailbox/delete/?from='folderName'

Request body:

either byte[] of singular Email json or Array of multiple Email json

Response code: 200 – success.	400 - failure.


POST - download an attachment

/peergos-api/v0/mailbox/download

Request body:

byte[] of Attachment json

Response code: 200 – success.	400 - failure.


PUT - upload an attachment

/peergos-api/v0/mailbox/attachment

Request body:

byte[] of Attachment binary data

Response code: 201 – success.	400 - failure.

Response (location response header field): 

{uuid: string id for uploaded attachment}


PUT - send an Email

/peergos-api/v0/mailbox/post

Request body:

byte[] of Email json

Response code: 201 – success.	400 - failure.


PUT - import an ical event

/peergos-api/v0/mailbox/event-inline

Request body:

byte[] of the text of an ical file

Response code: 201 – success.	400 - failure.


PUT - import an ical attachment

/peergos-api/v0/mailbox/event

Request body:

byte[] of Attachment json where the existing attachment references a valid ical file

Response code: 201 – success.	400 - failure.


PUT - create a new user folder. API Call launches dialog to enter the folder name

/peergos-api/v0/mailbox/folder

Response code: 201 – success.	400 - failure.


PUT - Updates the boolean state values of an email (unread, star)

/peergos-api/v0/mailbox/inbox

byte[] of Email json

Response code: 201 – success.	400 - failure.


## Developing a Peergos App

Select the peergos-app.json file and choose ‘Run App’ to launch the app from the current directory. 
This is only available for launchable apps.

Tip: During development, set the launchable property to get the fast dev cycle feedback. 

The install process will detect if an existing App has the same name. 
It will display the version of the already installed App. 

During install the App's files are copied to an internal folder. Any existing contents in the assets folder will be replaced. 
The contents of the data folder will be added to.  
The previously installed peergos-app.json file is copied to the App’s data directory as ‘peergos-app-previous.json’. 


