# Week 02 Session 1: A-E1 Completion

## A. Preflight

I used the existing macOS VS Code setup and a writable project folder at `/Users/zuzi/SCSC3203/`. Internet access was available for downloading Java dependencies. The project root is the folder that contains `pom.xml`.

## B. Java 21

Java 21 was installed with Homebrew and selected for this project. The verified JDK is Java `21.0.12.1`, and both `java` and `javac` are available. The project uses:

```text
JAVA_HOME=/opt/homebrew/opt/openjdk@21
```

The JDK is needed because it includes the compiler and runtime. `javac` compiles Java source code, and `java` runs the compiled application.

## C. VS Code Java environment

The Extension Pack for Java and Spring Boot Extension Pack were installed in VS Code. The workspace also contains Java 21 settings in `.vscode/settings.json`, so the Java language tools use the intended JDK.

## D. Maven Wrapper and embedded Tomcat

Maven Wrapper is included in the project as `mvnw` and `mvnw.cmd`. It downloads and uses the project Maven version, so a separate global Maven installation is not required. Spring Boot includes an embedded Tomcat server through the Web MVC dependency, so a separate Tomcat installation is not needed.

Spring Boot and Thymeleaf are downloaded by Maven from the dependencies in `pom.xml`. The dependencies also bring their required transitive libraries.

## E1. Spring Initializr project

The project was generated with Maven, Java, JAR packaging, group ID `my.utm.scsc3203`, artifact ID `week02-demo`, package `my.utm.scsc3203.week02`, Java 21, Spring Web MVC and Thymeleaf.

The available Spring Initializr service provided Spring Boot `4.1.0` at setup time, so that is the version in this project instead of the worksheet example `4.1.1`. The project root contains `pom.xml`, `mvnw`, `mvnw.cmd`, `.mvn/`, `src/main/java/` and `src/main/resources/`.

The Maven Wrapper verification completed successfully with Java 21. The POM contains `spring-boot-starter-webmvc` and `spring-boot-starter-thymeleaf`. No JAR files were downloaded manually because Maven resolves the declared dependencies and their transitive dependencies.

## Status

A-E1 completed. The project was then continued with the MVC controller, Home/About/Profile templates, executable JAR packaging, Dockerfile, and continuation answers.
