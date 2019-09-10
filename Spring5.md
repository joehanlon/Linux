[Intro to Spring Boot Web App : Lecture 10](https://bah.udemy.com/course/spring-framework-5-beginner-to-guru/learn/lecture/11162448#overview)

## Spring Initializr  
https://start.spring.io/  

Defaults (as of 9/2019):  
   - Project : Maven  
   - Language : Java  
   - Spring Boot : 2.1.8  
   - Project Metadata :
      - Group : com.example   
      - Artifact : demo  
      - Name : demo 
      - Package Name : com.example.demo  
      - Packaging : Jar    
      - Java : 8 
   - Developer Tools
      - Spring Boot DevTools  
      - Lombok
      - Spring Configuration Processor
   - Web
      - Spring Web
      - Spring Reactive Web
      - Rest Repositories
      - Spring Session
      - Spring Web Services
      - Etc.
   - Template Engines
      - Thymeleaf
      - Apache Freemarker
      - Mustache
      - Groovy Templates
   - Security
      - Spring Security
      - OAuth2 Client
      - OAuth2 Resource Server
      - Spring LDAP
      - Okta
   - SQL
      - Spring Data JPA
      - Spring Data JDBC
      - MySQL Driver
      - H2 Database
      - JDBC API
      - MyBatis Framework
      - PostgreSQL Driver
      - Apache Derby Database
      - JOOQ Access Layer
      - Etc.
   - NoSQL
      - Spring Data Redis (Access + Driver)
      - Spring Data Reactive Redis
      - Spring Data MongoDB
      - Spring Data Reactive MongoDB
      - Spring Data Neo4j
      - Etc.
   - Messaging
      - Spring Integration
      - Spring for Apache Kafka
      - Spring for Apache Kafka Streams
      - Etc.
   - I/O
      - Spring Batch
      - Etc.
   - Ops
      - Spring Boot Actuator
      - Spring Boot Admin (Client)
      - Spring Boot Admin (Server)
   - Etc. Etc. Etc. 
      
## First Project
#### Make the following changes / dependency selections
- Group : com.example  --> guru.springframework  
- Artifact : demo  --> spring5webapp  
- Name : demo --> Same as Artifact
- Package Name : com.example.demo  --> Group + Artifact (Name)
- Java : 8 --> 12  
- Spring Web
- Thymeleaf
- Spring Data JPA
- H2 Database
- Spring Boot Actuator
#### Download the project, unzip it in a preferred directory
```
# Default Maven Project Structure, based on what we selected in Spring Initializr
./spring5webapp/
   |_ .mvn/
   | |_ wrapper/
   |   |_ maven-wrapper.jar
   |   |_ maven-wrapper.properties
   |   |_ MavenWrapperDownloader.java
   |_ src/
   | |_ main/
   | | |_ java/
   | | | |_ guru/
   | | |   |_ springframework/
   | | |     |_ spring5webapp/
   | | |       |_ Spring5webappApplication.java
   | | |_ resources/
   | |   |_ static/
   | |   | |_ <EMPTY>
   | |   |_ templates/
   | |   | |_ <EMPTY>
   | |   |_ application.properties
   | |_ test/
   |   |_ java/ 
   |     |_ guru/
   |       |_ springframework/
   |         |_ spring5webapp/
   |           |_ Spring5webappApplicationTests.java
   |_ .gitignore
   |_ HELP.md
   |_ mvnw 
   |_ mvnw.cmd
   |_ pom.xml 
```
#### Below are details for the files created :
- mvnw : Maven wrapper. This allows us to run Maven commands w/o having Maven installed. (You should still install Maven though).
- Spring5webappApplication.java : This is the one application file that we have right now.
- application.properties : This is where we can define overrides for certain app settings.
Spring Boot has parent-child relationships. <parent> is tagged in the pom.xml file, and the project inherits curated dependencies / information from other releases when they occur. 
   
 #### To Run the App, Using the Maven Wrapper
 ```console
 # in Windows CMD
 cd spring5wepapp
 mvnw spring-boot:run
 # Tomcat will start on port : 8080
 ```

## JPA
**_Java Persistence API_**  
JPA is the official API for working with relational data in Java.  
JPA is only a specification, NOT a concrete implementation.  
It is an ORM : Object Relational Mapper.  
This means that it serves as a bridge between Java objects and data stored in relational databases.  
JPA offers Java developers database intependence since 1 API will support many relational databases.  

## Best Practices w/ Hibernate
Should have unique keys for your data.  
equals() and hashCode()  
 
