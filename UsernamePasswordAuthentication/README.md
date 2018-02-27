# How to generate a Secure OPCUA Client using Eclipse Milo

## Client authenticated via Username and Password

1. Go into the client example and generate a certificate following the steps in the README.md there
2. Download the docker image for the client from TODO: Upload docker image
    Alternatively: If you want to use a newer Milo version, replace the milo jar in the client and build the client image yourself
    Warning: It does not include a standalone version of clients and servers by default
3. Download the docker image for the server from TODO: Upload docker image
    Alternatively: If you want to use a newer Milo version, replace the milo jar in the server and build the server image yourself
    Warning: It does not include a standalone version of clients and servers by default
4. Place client's public certificate into "trusted" folder of server
5. Adapt the docker-compose file so that the client's "secrets" volume and the server's "certificates" volume are mapped to the correct folder on your system.
6. Run "docker-compose up" to test it. The server does not stop automatically so stop it with "Ctrl + C"