### This folder contains resources developed during a pair programming exercise at an Equal Experts' office

1. As a developer, I would like to develop a hello-world webapp and deploy it in tomcat. It should then be possible to make changes to the source code which should then be reflected in the webapp running in tompcat
    - `alias mvn='docker run -it --rm --name maven --network host -v $HOME/.m2:/root/.m2 -v $PWD:/usr/src/maven -w /usr/src/maven maven mvn'` - Use this docker image if maven is not already installed in the VM
    - `mvn archetype:generate -DarchetypeGroupId=org.apache.maven.archetypes -DarchetypeArtifactId=maven-archetype-webapp -DarchetypeVersion=1.4` - [maven-archetype-webapp](https://maven.apache.org/archetypes/maven-archetype-webapp/) is an archetype which generates a sample Maven webapp project. Provide groupId _(com.ee.app)_ and artifactId _(my-app)_ when prompted. Accept the default value for version
    - `sudo chown -R $USER:$USER my-app`
    - `cd my-app`
    - `mvn clean package` - This command creates a WAR package which can then be deployed in Tomcat
    - `mvn org.apache.tomcat.maven:tomcat7-maven-plugin:run` - Using this [Apache Tomcat Maven Plugin](http://tomcat.apache.org/maven-plugin-trunk/), start an instance of Tomcat and deploy the webapp
    - `http://techtest:8080/my-app/` - This URL in your web browser shoud show the content of "_src/main/webapp/index.jsp_"
      - Refreshing the URL after making a change in the index.jsp should reflect on the web page

1. As a member of an automation team, I would like take the source code delviered by the dev team and bundle the package _(WAR)_ in Tomcat docker container. In this part of the pairing exercise, a multi-stage Dockerfile is to be developed. The first stage will create the WAR file and the second stage will deploy the WAR in Tomcat docker image
    - `cd ..`
    - `docker image build --no-cache --tag my-app .` - This build a customised tomcat docker image
    - `docker image ls` - comfirm the image has indeed been built
    - `docker container run --rm --name my-app -d -p 8080:8080 my-app` - Launch Tomcat container
    - `docker container ls` - Confirm the container is up and running
    - `http://techtest:8080/my-app/` - This URL in your web browser shoud show the content of "_src/main/webapp/index.jsp_"

1. As a member of an automation team, I would like to take the Tomcat docker image developed in the previous section and orchestrate it as a swarm service in a docker swarm cluster. A docker compose file will be developed and run in this section
    - `docker stack deploy --compose-file docker-compose.yml webapp`
    - `docker stack ps webapp` - This should show the webapp container being up and running
    - `http://techtest:8080/my-app/` - This URL in your web browser shoud show the content of "_src/main/webapp/index.jsp_"

### Improvements
If we had more time, I would have liked to develop a [declarative pipeline](https://jenkins.io/doc/book/blueocean/getting-started/) in Jenkins' Blue Ocean. The first stage in the pipieline would build the docker image as in the section 2 above. The second stage in the pipeline would role out the change as in section 3
