# How to generate a Secure OPCUA Server using Eclipse Milo

## Copy trusted Client Certificate into Folder of trusted Certificates

1. Chose the right file. You do not want certificates including the private key 
   here (like .p12), you want the public certficate (like .crt).

2. Copy it into `certificates/security/trusted`
    
## How to build and start the OPCUA Server

1. Make sure docker is installed and running

2. Build the docker image for the opcua server
    `docker build -t opcua-server .`
    
   Important: If you build the server again, docker may use intermediate images and not check out the most recent version of the source code. 
   In this case use `docker build --no-cache -t opcua-server .`

3. Run the opcua server. Map the volume you use for certificates to /app/certificates.
    `docker run -v $(pwd)/certificates:/app/certificates opcua-server`
    
4. For debugging purposes it may be helpful to run it with a shell. 
    This allows you to check whether enviroment variables are set correctly, other containers can be reached or explore the filesystem.
    `docker run -it -v $(pwd)/certificates:/app/certificates  opcua-server /bin/bash`
