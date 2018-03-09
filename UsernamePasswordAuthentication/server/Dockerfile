# Need maven, java and git to work, all of those are included here
FROM maven

# Set the working directory to /app
WORKDIR /app

# Clone the milo fork and checkout the correct branch, build the jar using maven and copy the jar into the container at /app, then remove the unnecessary code to reduce image size
RUN git clone https://github.com/mbruns42/milo.git \
    && cd milo \
    && git checkout origin/user-database-docker \
    && mvn clean package -P userdatabase -DskipTests \
    && cp /app/milo/milo-examples/standalone-examples/target/milo-opcua-server-jar-with-dependencies.jar /app \
    && rm -r /app/milo

# Add seperate directory for secrets that should not be copied into image for reuseability
VOLUME /app/security

# Tell the server it is responsible for traffic adressed to "server", docker networks substitute the correct IP
ENV OPCUA_SERVER_IP server

# Directory in which the security related information such as certificates and user databases is located
ENV SECURE_FOLDER /app/security

EXPOSE 4840

# Run example client
CMD ["java", "-jar", "milo-opcua-server-jar-with-dependencies.jar"]