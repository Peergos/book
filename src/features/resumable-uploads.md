# Resumable Uploads

It can be a pain if you're uploading a huge file to some service and then your internet cuts out half way and you need to start again. Peergos avoids this by implementing resumable uploads. If you try and upload a file that failed halfway for whatever reason, then it will ask you if you want to continue the upload. This even works if you're uploading from a different device!

We achieve this by storing an upload transaction file with details of the upload in your peergos space during the upload. 