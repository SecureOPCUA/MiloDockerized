# How to generate a Secure OPCUA Client using Eclipse Milo

## Client authenticated via Certificates

1. Go into the client example and generate a certificate following the steps in the README.md there
2. Build the docker image for the client accord to README.md in the `client` directory
3. Build the docker image for the server accord to README.md in the `server` directory
4. Place client's public certificate into "trusted" folder of server
5. Run "docker-compose up" to test it. The server does not stop automatically so stop it with "Ctrl + C"