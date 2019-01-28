# Sending a follow request

Sending a follow request proceeds in the following steps:

1) Look up the target friend's public following key

2) Create a directory /our-name/shared/friend-name

3) Encrypt a read capability for that directory using a random key pair to the target's following key. Using a random keypair ensures that noone but the target friend can see who sent the request. 

4) Send the follow request to the storage server of the target friend

The target can then either allow and reciprocate (full bi-directional friendship), allow (you are following them), reciprocate (they are following you) or deny. If they have reciprocated then you can grant read or write access to any file or folder by adding a read or write capability in their directory in your space.

When you receive a follow request and either allow or reciprocate it then you add the capability in the request to your static data on your identity WriterData, so you can find it again later, before deleting the follow request from your server. 