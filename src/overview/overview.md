# Peergos - your private online space

Peergos is building the next web - the private web, where end users are in control. Imagine web apps being secure by default and unable to track you. Imagine being able to control exactly what personal data each web app can see. Imagine never having to log in to an app ever again. You own your data and decide where it is stored and who can see it. At Peergos, we believe that privacy is a fundamental human right and we want to make it easy for everyone to interact online in ways that respect this right. 

Peergos is a peer-to-peer encrypted global filesystem with fine-grained access control designed to be resistant to surveillance of data content or friendship graphs. It will have a secure messenger, with optional interoperability with email. There will also be a totally private and secure social network, where users are in control of who sees what (executed cryptographically).

The name Peergos comes from the Greek word Πύργος (Pyrgos), which means stronghold or tower, but phonetically spelt with the nice connection to being peer-to-peer. It is pronounced peer-goss, as in gossip. 

**WARNING:** Peergos has had an audit by Cure53, but is still in active development. Some of the features in this documentation are yet to be implemented, in particular, Tor is not used yet.

## Aims

 - **Securely** and **privately** store files in a peer to peer network which has no central node and is generally difficult to disrupt or surveil
 - **Secure sharing** of such files with other users of the network without visible meta-data (who shares with who)
 - **Trust free** servers and storage. Clients do not need to trust their server or storage. Data, metadata, and contact lists are never exposed to your server. 
 - **Beautiful user interface** that any computer or mobile user can understand
 - **Secure messaging**, with optional interop with actual email
 - **Independent** of the central SSL CA trust architecture, and the domain name system
 - **Self hostable** - A user should be able to easily run Peergos on a machine in their home and get their own Peergos storage space, and social communication platform from it 
 - Secure *web interface* as well as desktop clients, native folder sync, and a command line interface
 - Do not rely on any cryptocurrency
 - Enable users to *collaborate*, editing a document in place concurrently
 - Secure real time chat, and *video conferencing* able to handle 100s of participants, fully end-to-end encrypted
 - Social *account recovery* - designate N of M friends who can collaborate to recover your account if you lose your password
 - Optional use of U2F for securing login