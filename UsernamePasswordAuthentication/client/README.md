# How to generate a Secure OPCUA Client using Eclipse Milo

## How to build and start the opcua client

1. Make sure docker is installed and running

2. Build the docker image for the opcua client
    `docker build -t opcua-client .`
    
   Important: If you build the client again, docker may use intermediate images and not check out the most recent version of the source code. 
   In this case use `docker build --no-cache -t opcua-client .`

3. Run the opcua client
    `docker run -v $(pwd)/secrets:/app/secrets opcua-client`
    
4. For debugging purposes it may be helpful to run it with a shell. 
    This allows you to check whether enviroment variables are set correctly, other containers can be reached or explore the filesystem.
    `docker run -it -v $(pwd)/secrets:/app/secrets opcua-client /bin/bash`
    
## Without docker container and more elegant variant

Read password from console instead of using enviroment variables. This can also be done with a docker container, but you can no longer
detach from the container when you want to enter a password (or you need to re-attach later).

        Console console = System.console();
        if (console == null) {
            logger.error("Couldn't get Console instance");
            System.exit(0);
        }
        char[] keystorePasswordArray = console.readPassword("Enter your keystore password: ");