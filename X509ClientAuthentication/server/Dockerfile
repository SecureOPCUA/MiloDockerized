# Need maven, java and git to work, all of those are included here
FROM maven

# Set the working directory to /app
WORKDIR /app

# Clone the milo fork, build the jar using maven and copy the jar into the container at /app, then remove the unnecessary code to reduce image size
RUN git clone https://github.com/mbruns42/milo.git \
    && cd milo \
    && mvn package -P standalone-server -DskipTests \
    && cp /app/milo/milo-examples/server-examples/target/milo-opcua-server-jar-with-dependencies.jar /app \
    && rm -r /app/milo

# Add seperate directory for secrets that should not be copied into image for reuseability
VOLUME /app/certificates

# Tell the server it is responsible for traffic adressed to "server", docker networks substitute the correct IP
ENV OPCUA_SERVER_IP "server"

# The certificates can be found here, both the server's own and rejected or trusted client certificates
ENV OPCUA_CERT_DIR "/app/certificates"

EXPOSE 4840

# Run example client
CMD ["java", "-jar", "milo-opcua-server-jar-with-dependencies.jar"]