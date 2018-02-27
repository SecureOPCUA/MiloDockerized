# How to generate a Secure OPCUA Client using Eclipse Milo

## Client authenticated via Certificates

1. Go into the client example and generate a certificate following the steps in the README.md there
2. Build the docker image for the client 
3. Build the docker image for the server 
4. Place client's public certificate into "trusted" folder of server
5. Adapt the docker-compose file so that the client's "secrets" volume and the server's "certificates" volume are mapped to the correct folder on your system.
6. Run "docker-compose up" to test it. The server does not stop automatically so stop it with "Ctrl + C"