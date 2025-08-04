# ----- Build Stage -----
FROM openjdk:11 AS build_image

# Install Maven
RUN apt update && apt install -y maven

# Set working directory
WORKDIR /app

# Copy the entire application source code (from build context)
COPY . .

# Build the WAR file
RUN mvn clean install

# ----- Runtime Stage -----
FROM tomcat:9-jre11

# Clean default webapps
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy WAR file to Tomcat
COPY --from=build_image /app/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
CMD ["catalina.sh", "run"]
