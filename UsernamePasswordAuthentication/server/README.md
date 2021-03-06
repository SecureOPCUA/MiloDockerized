# How to generate a Secure OPCUA Server using Eclipse Milo
    
## How to build and start the OPCUA Server

1. Make sure docker is installed and running

2. Build the docker image for the opcua server
    `docker build -t opcua-username-server .`
    
   Important: If you build the server again, docker may use intermediate images and not check out the most recent version of the source code. 
   In this case use `docker build --no-cache -t opcua-username-server .`

3. Run the opcua server. Map the volume you use for credentials to /app/credentials.
    `docker run -v $(pwd)/credentials:/app/credentials opcua-username-server`
    
4. For debugging purposes it may be helpful to run it with a shell. 
    This allows you to check whether enviroment variables are set correctly, other containers can be reached or explore the filesystem.
    `docker run -it -v $(pwd)/credentials:/app/credentials  opcua-username-server /bin/bash`
