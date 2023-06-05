# Login

To login your password + username + public salt are hashed using [scrypt](https://en.wikipedia.org/wiki/Scrypt) hashing function (with parameters 17, 8, 1, 64). The output of this is your login keypair and a symmetric key. The login keypair is used to auth with your home server and retrieve your encrypted login data, which is decrypted using the aforementioned symmetric key. Alternatively client can cache the encrypted login data to allow offline login. The login data contains your identity keypair, social keypair, and the root capability to your filesystem. The public salt and encrypted login data are stored on your home server, and any mirror your have authorised. 

<img alt="Login key derivation" src="/img/login.png" class="center" style="width: 70%;" />