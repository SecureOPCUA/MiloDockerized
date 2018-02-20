# How to generate a Secure OPCUA Client using Eclipse Milo

## Certificate Requirements:

Certificate Table in OPCUA Spec Part 4, Table 109. 
In OpenSecureChannel is specified "The Certificates used in the OpenSecureChannel service shall be the Application Instance Certificates."

* certificate must be PKIX compliant (at least for X.509)
* certificate must be valid at thre correct time
* the trust chain must check out (can be traced back to trusted certificates)
* the application uri must be correct (referred to as field 6 in milo, not sure why)
* subject alt name DNS and/or IP address must match the used one (referred to as field 2 and 7 in milo)
* the keyUsage must include: digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment



## How to generate a compliant certificate:



1. Personalize the openssl config file secrets/openssl.cnf 
    e.g.,
    -  `countryName_default	= DE`
    - `stateOrProvinceName_default	= Nordrhein-Westfalen`
    - `organizationName_default = Fraunhofer`
    - `organizationalUnitName_default	= Software Engineering`
    - `commonName_default  = John Doe`
    - `emailAdress_default     = john2@doe.com`

2. Generate openssl command using the config:
    `openssl req -x509 -sha256 -newkey rsa:2048 -keyout privateKey.key -out certificate.crt -extensions v3_self_signed -config=openssl.cnf`
    
3.  Convert into one file
    `openssl pkcs12 -export -name opcua -in certificate.crt -inkey privateKey.key > opcua.p12`

4. Import certificate into keystore
    `keytool -importkeystore -srckeystore opcua.p12 -destkeystore opcua.keystore -srcstoretype pkcs12 -alias opcua`

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

Read password for keystore and key from console instead of using enviroment 
variables. This can also be done with a docker container, but you can no longer
detach from the container when you want to enter a password (or you need to 
re-attach later).

        Console console = System.console();
        if (console == null) {
            logger.error("Couldn't get Console instance");
            System.exit(0);
        }
        char[] keystorePasswordArray = console.readPassword("Enter your keystore password: ");