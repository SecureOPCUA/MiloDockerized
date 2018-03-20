# Need maven, java and git to work, all of those are included here
FROM maven

# Set the working directory to /app
WORKDIR /app

# Clone the milo fork and checkout the correct branch, build the jar using maven and copy the jar into the container at /app, then remove the unnecessary code to reduce image size
RUN git clone https://github.com/mbruns42/milo.git \
    && cd milo \
    && git checkout origin/user-database-docker \
    && mvn clean package -P userdatabase -DskipTests \
    && cp /app/milo/milo-examples/standalone-examples/target/milo-opcua-client-jar-with-dependencies.jar /app \
    && rm -r /app/milo
    
# Add a script that makes the client wait for the server to accept TCP connections
ADD wait-for-it.sh /app

# Define endpoint of server - the name refers to the other container in the docker compose
ENV ENDPOINT_URL opc.tcp://server:4840/example

# Get client credentials from credentials file
ADD credentials /app
RUN eval $(cat credentials) \
    && rm credentials

# Run example client
CMD ["./wait-for-it.sh", "server:4840", "--","java", "-jar", "milo-opcua-client-jar-with-dependencies.jar"]
