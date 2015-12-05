# Artifactory

Run [Artifactory](http://www.jfrog.com/home/v_artifactory_opensource_overview) inside a Docker container.
This image is based on the following image:
Link: [mattgruter/artifactory](https://registry.hub.docker.com/u/mattgruter/artifactory/)

(this image is using the open source version of artifactory and storers data in the embedded derby database)

## Volumes
Artifactories `data`, `logs` and `backup` directories are exported as volumes:

    <USER_HOME>/docker/data/artifactory/data
    <USER_HOME>/docker/data/artifactory/logs
    <USER_HOME>/docker/data/artifactory/backup

## Ports
The service is exposed through port `8081`.

## Example
To run artifactory do:

    docker run -p 18081:8081 amesken/artifactory

Now point your browser to http://192.168.99.100:18081/artifactory

Administration is done by user `admin` with password `password`.

## URLs
The artifactory servlet is available at the `artifactory/` path. However a filter redirects all paths outside of `artifactory/` to the artifactory servlet. Thus instead of linking to the URL http://192.168.99.100:18081/artifactory/libs-release-local you can just link to http://192.168.99.100:18081/libs-release-local (i.e. omitting the subpath `artifactory/`).

## Runtime options
Inject the environment variable `RUNTIME_OPTS` when starting a container to set Tomcat's runtime options (i.e. `CATALANA_OPTS`). The most common use case is to set the heap size:

    docker run -e RUNTIME_OPTS="-Xms256m -Xmx512m" -P amesken/artifactory


# Advanced Artifactory

## Switching to Artifactory Pro
If you are using Artifactory Pro, the artifactory war archive has to be replaced. The image tagged `-onbuild` is built with an `ONBUILD` trigger for this purpose. Unpack the Artifactory Pro distribution ZIP file and place the file `artifactory.war` (located in the archive) in the same directory as a simple Dockerfile that extends the `onbuild` image:

    # Dockerfile for Artifactory Pro
    FROM mattgruter/artifactory:latest-onbuild

Now build your child docker image:

    docker build -t yourname/myartifactory .

The `ONBUILD` trigger ensures your `artifactory.war` is picked up and applied to the image upon build.

    docker run -P yourname/myartifactory
    
## Context of this image
This artifactory image is originally created to work with a complete Continuous Delivery tool stack which can be obtained from [docker](https://hub.docker.com/r/amesken/cd-tool-stack/) or [github](https://github.com/amesken/cd-tool-stack).
The image is based on [this blog article](https://blog.codecentric.de/en/2015/10/continuous-integration-platform-using-docker-container-jenkins-sonarqube-nexus-gitlab) by Marcel Birkner.