# Physical

The Physical architecture consists of a Tor hidden service for each user, behind which is a Peergos server which stores and publishes that user's mutable pointers, data store, and receives incoming follow requests. There is also the global append only log for the PKI. 