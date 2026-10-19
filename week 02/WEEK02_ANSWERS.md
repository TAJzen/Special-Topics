# SCSC3203 Week 02 Answers

## Discussion: Maven dependencies and target/

The dependencies in `pom.xml` tell Maven which libraries the project needs. Maven also downloads the libraries that those dependencies depend on. These are called transitive dependencies. This is useful because I do not have to download every JAR file manually or find compatible versions myself.

The `target/` folder contains generated files such as compiled classes, test reports and the packaged JAR. It can be generated again by Maven, so it should not be uploaded as source code. It is also ignored in `.gitignore`.

## Reflection and exit ticket

### 28. How do JDK, Maven, Spring Boot, embedded Tomcat and Thymeleaf work together?

The JDK provides the Java compiler and the Java Virtual Machine. Maven Wrapper builds the project and downloads the libraries listed in `pom.xml`. Spring Boot starts the application and configures the web application. Embedded Tomcat acts as the web server, so I do not need to install Tomcat separately. Thymeleaf takes the HTML templates and renders them when a controller returns a view name.

### 29. Why does `return "index"` render `index.html` instead of sending the text `index`?

The method is inside a class marked with `@Controller`, so Spring treats the returned value as a view name. Thymeleaf looks for a template called `index` in the templates folder and renders `index.html`. It would be different with `@ResponseBody`, because then the returned value would be sent as response text.

### 30. What is the difference between a Git commit and a Git push?

A commit saves a version of my changes in the local Git repository. A push sends those local commits to a remote repository such as GitHub. I can commit without internet access, but I need a configured remote and authentication to push.

### 31. Which checkpoint was most difficult and what evidence helped you solve it?

The most difficult checkpoint was opening the application after adding the profile page. The browser showed a 404 because an older Java process was still running on port 8080. I checked the process using the port, stopped it, and started the application again from the project `pom.xml`. The useful evidence was the Spring Boot startup message, the controller mapping, and the successful HTTP response from `/profile`.

## Evidence checklist

- Java 21 and `javac` 21 were installed and verified.
- Java Extension Pack and Spring Boot Extension Pack were installed in VS Code.
- Maven Wrapper completed successfully.
- The Spring Boot application started on port 8080.
- The Home page rendered successfully.
- The About page and navigation worked.
- The Profile page rendered with fictional student details.
- `./mvnw test` passed.
- The application was packaged as `target/week02-demo-0.0.1-SNAPSHOT.jar`.
- Git was initialized and `target/` was confirmed ignored.

## Short reflection

At first, I got a 404 when I opened the profile page. I checked the controller and found that the `/profile` mapping and `profile.html` were already present. The problem was that an old Java process was still using port 8080, so the browser was not using the latest compiled code. I checked which process was listening on port 8080, stopped it, and started the application again from the project folder. After restarting, `/profile` returned the student profile and the links to Home and About worked. This showed me that a 404 can be caused by an incorrect route, but it can also happen when the browser is connected to an old version of the application. Checking the server process and startup output helped me find the actual cause.

## Week 02 Continuation

### E2. Maven and project verification

The project uses group ID `my.utm.scsc3203`, artifact ID `week02-demo`, and version `0.0.1-SNAPSHOT`. The packaging is JAR because packaging is omitted from the POM and Maven uses JAR by default. The project contains the Maven Wrapper, `.mvn`, Java source files, resource files, the MVC controller and Thymeleaf templates. The Maven Wrapper command completed successfully.

### G2. Port configuration

The normal port is 8080. To test another port, I can temporarily add `server.port=8081` to `application.properties`, restart the application, and open `http://localhost:8081/`. After testing, I remove that line so the application returns to port 8080.

### H1. Docker fundamentals

A Dockerfile is a text file containing instructions for building an image. An image is a packaged application and runtime stored in layers. A container is a running instance of an image. Port mapping connects a port on the host computer to a port inside the container.

The workflow is Java source, Maven package, executable JAR, Docker image, and then a running container. Docker is a container platform, not automatically a CI/CD pipeline.

### H2. Docker verification

Docker was not installed on my current Mac, so the Docker Engine and `hello-world` container could not be verified locally. The correct verification commands are `docker --version`, `docker info`, and `docker run --rm hello-world`. The required success message would be `Hello from Docker!`.

### H3-H5. Dockerfile and container commands

The Dockerfile uses the Java 21 runtime image, copies the executable JAR into `/app/app.jar`, exposes port 8080, and starts the JAR with `java -jar`. After installing Docker Desktop, the image can be built and run with:

```text
./mvnw clean package
docker build -t scsc3203-week02 .
docker run -d --name week02-web -p 8080:8080 scsc3203-week02
```

The website can then be tested at `http://localhost:8080/`, `http://localhost:8080/about`, and `http://localhost:8080/profile`. The container can be inspected with `docker ps` and `docker logs week02-web`.

### H6-H7. Container cleanup and port mapping

The container can be stopped and removed with `docker stop week02-web` and `docker rm week02-web`. To test a different host port, I can run `docker run -d --name week02-web-alt -p 8082:8080 scsc3203-week02` and open `http://localhost:8082/`. The first number, 8082, is the host port. The second number, 8080, is the port used by Spring Boot inside the container. Docker forwards traffic between them.

### Continuation exit ticket

**Why does `return "index"` render `index.html` instead of sending the word `index`?**

The class is a Spring MVC `@Controller`, so the returned string is treated as a logical view name. Thymeleaf looks for the matching `index.html` file in `src/main/resources/templates/` and renders it.

**How do JDK, Maven, Spring Boot, embedded Tomcat and Thymeleaf work together?**

The JDK compiles and runs the Java application. Maven Wrapper downloads dependencies and builds the project. Spring Boot starts and configures the application. Embedded Tomcat receives browser requests, while Thymeleaf renders the HTML templates returned by the controller.

**How is a Docker image different from an executable JAR and a running container?**

An executable JAR is the packaged Spring Boot application. A Docker image contains that JAR together with the Java runtime and startup instructions. A container is the running process created from the image.

**Why can host port 8082 display an application listening on port 8080 inside Docker?**

The mapping `-p 8082:8080` connects host port 8082 to container port 8080. The browser connects to the host port, and Docker forwards the request to Spring Boot inside the container.

**Why does manually running `docker build` not constitute a CI/CD pipeline?**

Running `docker build` manually is only one build action started by a person. A CI/CD pipeline is an automated workflow that usually checks out code, runs tests, builds an image or application, and may deploy it when changes are pushed.
