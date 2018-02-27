# How to generate a Secure OPCUA Client using Eclipse Milo

## Certificate Requirements:

For OPCUA Version 1.04, check in OPCUA Specification Part 4, Section 6.1.2 and Table 106 and Part 6, Section 6.2 and Table 36.
Every OPCUA Application requires an Application Instance Certificate with the following requirements:

* The certificate must be a valid X.509v3 certificate
* Certificates used by OPC UA applications shall also conform to RFC 3280 which defines a profile for X.509 v3 Certificates when they are used as part of an Internet based application.
* An Application Instance Certificate is a ByteString containing the DER encoded form (see X690) of an X.509 v3 Certificate. 
* If the server does not trust self-signed certificates, the issuer of the certificate must be known and must be traced back to trusted certificates of the server
* The certificate must contain the network name or address of the computer where the application runs
* The certificate must contain the name of the organisation that administers or owns the application
* The certificate must contain the name of the application
* The certificate must contain the URI of the application instance
* The Certificate signature shall comply with the CertificateSignatureAlgorithm, MinAsymmetricKeyLength and MaxAsymmetricKeyLength requirements for the used SecurityPolicy
* The certificate must be valid at the time of the use, unless the server surpresses errors about this
* For servers, the hostname in the URL used to connect to it, must be one of the hostnames specified in the certificate
* The certificate may have a revocation list. Revoked certificates may not be used.
* The keyUsage shall include digitalSignature, nonRepudiation, keyEncipherment and dataEncipherment. Other key uses are allowed.
* The alternate names (subjectAltName) for the application Instance shall include a URI which is equal to the applicationUri. The URI shall be a valid URL (see RFC 1738) or a valid URN (see RFC 2141).


## How to generate a compliant certificate:

1. Generate the certificate using the openssl command and the configuration file:
    `openssl req -x509 -sha256 -newkey rsa:2048 -keyout privateKey.key -out certificate.crt -extensions v3_self_signed -config=openssl.cnf`
    
3.  Convert the key and the certificate into one file
    `openssl pkcs12 -export -name opcua -in certificate.crt -inkey privateKey.key > opcua.p12`

4. Import the certificate into the keystore
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