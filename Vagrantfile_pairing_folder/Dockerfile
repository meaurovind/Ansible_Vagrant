# https://docs.docker.com/develop/develop-images/multistage-build/

FROM maven as BUILDER
COPY my-app /usr/src/maven
WORKDIR /usr/src/maven
RUN mvn clean package

FROM tomcat
COPY --from=BUILDER /usr/src/maven/target/my-app /usr/local/tomcat/webapps/my-app
