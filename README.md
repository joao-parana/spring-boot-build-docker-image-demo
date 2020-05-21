# Spring Boot bootBuildImage example app

How to build an [OCI compliant](https://www.opencontainers.org/) docker image with Spring Boot 2.3.0

## Requirements

Installed on your machine:
- docker
- java 11+

## Running with LiveReload

```bash
mvn clean spring-boot:run
curl -i http://localhost/messorconf-v2/go/10
```

## Building Docker image

```bash
mvn clean ; mvn compile ; ./gradlew bootBuildImage
docker run --rm -p 80:8080 --name soma3 -it spring-boot-build-docker-image-demo
curl -i http://localhost/messorconf-v2/go/10
```

## Creating a Fat Jar and running it

```bash
mvn clean compile package assembly:single | grep -v " already added, skipping" ; java -jar target/SBBDID.jar
curl -i http://localhost/messorconf-v2/go/10
```

## Play

- clone this repository
- run `docker images` and see the list of images you have locally
- run `./gradlew bootBuildImage` from this repo directory to generate an [OCI image](https://www.opencontainers.org/) for this application
- run `docker images` and see the new images created
- you should see also `spring-boot-build-docker-image-demo` in the list now!
- run it with `docker run spring-boot-build-docker-image-demo`
- you can see from the output that it's running with java 11
- uncomment the `bootBuildImage` section in `build.gradle`
- run `./gradlew bootBuildImage` again to update the image
- see how _only_ the `jre` layer is updated
- not even the app layer is updated (even though there's a change in `build.gradle`!)
- the app layer is not updated even if you add a new line anywhere on the source code!
- run your new image always with `docker run spring-boot-build-docker-image-demo`
- your app is now
  - running with openjdk 14
  - with just one line change on your gradle build file
  - and only one layer (jdk) of the original image has been updated
  
# Notes

- Spring Boot [gradle docs](https://docs.spring.io/spring-boot/docs/current/gradle-plugin/reference/html/#build-image)
- Works also with [maven and the `build-image` goal](https://docs.spring.io/spring-boot/docs/current/maven-plugin/reference/html/#build-image)
- [What's new in Spring Boot 2.3.0](https://spring.io/blog/2020/05/15/spring-boot-2-3-0-available-now)
- [Spring Boot and buildpacks](https://spring.io/blog/2020/01/27/creating-docker-images-with-spring-boot-2-3-0-m1)
- [buildpacks](https://buildpacks.io/)
- [Open Container Initiative](https://www.opencontainers.org/)