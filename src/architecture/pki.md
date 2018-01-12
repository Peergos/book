# Usernames

The public keys and usernames are stored in a global append only data structure, with names taken on a first come first served basis. This needs consensus to ensure uniqueness of usernames. This is also where the Tor address of the server responsible for synchronising the user's writes is. 