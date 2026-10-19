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