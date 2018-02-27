# Need maven, java and git to work, all of those are included here
FROM maven

# Set the working directory to /app
WORKDIR /app

# Clone the milo fork, which includes standalone examples and checkout the correct branch
RUN git clone https://github.com/mbruns42/milo.git && cd milo && git checkout origin/user-database-docker

# Use maven to build a client jar
RUN mvn package -P userdatabase -Dmaven.test.failure.ignore=true && cd /app

# Copy the compiled jar into the container at /app as well as a script that makes the client wait for the server to accept TCP connections
RUN cp milo/milo-examples/client-examples/target/milo-opcua-client-jar-with-dependencies.jar /app
ADD wait-for-it.sh /app

# Define endpoint of server - the name refers to the other container in the docker compose
ENV ENDPOINT_URL opc.tcp://server:4840

# Put passwords here
ENV USERNAME User
ENV PASSWORD userpassword

# Run example client
CMD ["./wait-for-it.sh", "server:4840", "--","java", "-jar", "milo-opcua-client-jar-with-dependencies.jar"]
