# MiloDockerized
Contains dockerized OPC UA examples on the basis of Eclipse Milo that use OPC UA security features
 - x509 Client Authentication: The client generates a certificate using OpenSSL that conforms to OPC UA und presents this certificate to the OPC UA server. 
 - Username & Password Client Authentication: The client presents username and password
 to the OPC UA server, which the server
 verifies against a database of trusted Users. Rejected users are put into a separate database.