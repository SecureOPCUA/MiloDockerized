# Need maven, java and git to work, all of those are included here
FROM maven

# Set the working directory to /app
WORKDIR /app

# Clone the milo fork, which includes standalone examples and checkout the correct branch
RUN git clone https://github.com/mbruns42/milo.git && cd milo && git checkout origin/user-database-docker

# Use maven to build a server jar
RUN mvn package -P userdatabase -Dmaven.test.failure.ignore=true && cd /app

# Copy the directory contents of the milo's directory into the container at /app
RUN cp milo/milo-examples/server-examples/target/milo-opcua-server-jar-with-dependencies.jar /app

# Add seperate directory for secrets that should not be copied into image for reuseability
VOLUME /app/credentials

# Tell the server it is responsible for traffic adressed to "server", docker networks substitute the correct IP
ENV OPCUA_SERVER_IP "server"

# Here the certificates for the can be found, both the server's own and rejected or trusted client credentials
ENV OPCUA_CERT_DIR "/app/credentials"

EXPOSE 4840

# Run example client
CMD ["java", "-jar", "milo-opcua-server-jar-with-dependencies.jar"]