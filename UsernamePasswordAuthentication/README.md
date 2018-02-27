# How to generate a Secure OPCUA Client using Eclipse Milo

## Client authenticated via Username and Password

1. Build the docker image for the client accord to README.md in the `client` directory
2. Build the docker image for the server accord to README.md in the `server` directory
3. Run "docker-compose up" to test it. The server does not stop automatically so stop it with "Ctrl + C"