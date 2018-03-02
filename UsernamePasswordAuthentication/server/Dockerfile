# Need maven, java and git to work, all of those are included here
FROM maven

# Set the working directory to /app
WORKDIR /app

# Clone the milo fork, which includes standalone examples and checkout the correct branch
RUN git clone https://github.com/mbruns42/milo.git && cd milo && git checkout origin/user-database-docker

# Use maven to build a server jar
RUN cd milo && mvn package -P userdatabase -Dmaven.test.failure.ignore=true && cd /app

# Copy the directory contents of the milo's directory into the container at /app
RUN cp /app/milo/milo-examples/standalone-examples/target/milo-opcua-server-jar-with-dependencies.jar /app

# Add seperate directory for secrets that should not be copied into image for reuseability
VOLUME /app/credentials

# Tell the server it is responsible for traffic adressed to "server", docker networks substitute the correct IP
ENV OPCUA_SERVER_IP server

# Directory in which the user database is located
ENV SECURE_FOLDER /app/credentials

EXPOSE 4840

# Run example client
CMD ["java", "-jar", "milo-opcua-server-jar-with-dependencies.jar"]