# How to generate a Secure OPCUA Client using Eclipse Milo

## Client authenticated via Username and Password

1. Build the docker image for the client accord to README.md in the `client` directory
2. Build the docker image for the server accord to README.md in the `server` directory
3. Run `docker-compose up` to test it. 
The server does not stop automatically so stop it with "Ctrl + C".
You will know the test succeeded when you read the log message "Succeeded in making a connection on a secure channel.".
4. Client and server can now be used separately. 
The server image can be reused, as the user database for the server is
both located outside the images and can be changed without rebuilding the image.