[Spring](https://spring.io)  
[Spring Security](https://spring.io/projects/spring-security)  
[Spring Security SAML](https://projects.spring.io/spring-security-saml/)  
[Spring Security SAML Reference](https://docs.spring.io/spring-security-saml/docs/current/reference/htmlsingle/)  
[Spring Security SAML API](https://docs.spring.io/spring-security-saml/docs/current/api/)  
[Spring Security SAML GitHub](https://github.com/spring-projects/spring-security-saml)  
[Spring Security Extension RELEASES : Spring Security SAML](https://repo.spring.io/list/release/org/springframework/security/extensions/spring-security-saml/)

```console
# Main GitHub Project
# Doesn't work as of 9/12/2019
git clone https://github.com/spring-projects/spring-security-saml.git

#
```

#### Prerequisites

1. Java 1.6+ SDK
2. Apache Maven

#### Samples

1. [Sources](https://github.com/spring-projects/spring-security-saml)
   - Main project from Spring Security
   - Currently being re-written (as of 9/12/2019) and is very buggy
   - Only allows Gradle at the moment as a build management tool
2. [Releases](https://repo.spring.io/list/release/org/springframework/security/extensions/spring-security-saml/)
   - Works as of 9/12/2019
   - Need to be mindful of dependencies
   - e.g. The compiler marked in **pom.xml** file was too old, which produced the following error when I tried to build with Maven :

```console
[ERROR] COMPILATION ERROR :
[INFO] -------------------------------------------------------------
[ERROR] Source option 6 is no longer supported. Use 7 or later.
[ERROR] Target option 6 is no longer supported. Use 7 or later.
...
```

#### Proceeding with Building from a Release (Version 1.0.9 as of 9/12/2019)

```console
spring-security-saml-1.0.9.RELEASE\
   |
   |_ docs\
   | |_ api\
   | |_ reference\
   |
   |_ libs\
   | |_ spring-security-saml2-core-1.0.9.RELEASE.jar
   | |_ spring-security-saml2-core-1.0.9.RELEASE-javadoc.jar
   | |_ spring-security-saml2-core-1.0.9.RELEASE-sources.jar
   |
   |_ sample\
   | |_ src\
   | | |_ main\
   | |   |_ java\
   | |   | |_ org\
   | |   |   |_ springframework\
   | |   |     |_ security\
   | |   |       |_ saml\
   | |   |         |_ web\
   | |   |           |_ MetadataController.java
   | |   |           |_ MetadataForm.java
   | |   |           |_ MetadataValidator.java
   | |   |
   | |   |_ resources\
   | |   | |_ metadata\
   | |   | | |_ idp.xml
   | |   | |
   | |   | |_ security\
   | |   | | |_ samlKeystore.jks
   | |   | |_ log4j.properties
   | |   |
   | |   |_ webapp\
   | |     |_ css\
   | |     | |_ style.css
   | |     |
   | |     |_ images\
   | |     | |_ favicon.png
   | |     | |_ logo.png
   | |     | |_ main-two-columns.gif
   | |     | |_ main-two-columns-left.gif
   | |     |
   | |     |_ WEB-INF\
   | |     | |_ security\
   | |     | | |_ adminLogin.jsp
   | |     | | |_ idpSelection.jsp
   | |     | | |_ metadataGenerator.jsp
   | |     | | |_ metadataList.jsp
   | |     | | |_ metadataView.jsp
   | |     | | |_ providerView.jsp
   | |     | |
   | |     | |_ templates\
   | |     | | |_ footer.jsp
   | |     | | |_ head.jsp
   | |     | | |_ navigation.jsp
   | |     | | |_ sidebar.jsp
   | |     | |_ saml-servlet.xml
   | |     | |_ securityContext.xml
   | |     | |_ web.xml
   | |     |_ error.jsp
   | |     |_ index.jsp
   | |     |_ logout.jsp
   | |
   | |_ target\
   | |_ pom.xml
   |_ license.txt
```

#### Configuration of IDP metadata

**Modify file securityContext.xml and replace the metadata bean**
_The settings tell system to download IDP metadata from the given URL with timeout of 5 seconds. In
production system metadata should be either stored as a local file or be downloaded from a source
using SSL/TLS with configured trust or which provides digitally signed metadata._

```console
<bean id="metadata" class="org.springframework.security.saml.metadata.CachingMetadataManager">
   <constructor-arg>
      <list>
         <bean class="org.opensaml.saml2.metadata.provider.HTTPMetadataProvider">
            <constructor-arg>
               <value type="java.lang.String">http://idp.ssocircle.com/idp-meta.xml</value>
            </constructor-arg>
            <constructor-arg>
               <value type="int">5000</value>
            </constructor-arg>
            <property name="parserPool" ref="parserPool"/>
         </bean>
      </list>
   </constructor-arg>
</bean>
```

#### Generation of SP metadata

**Modify securityContext.xml again, replacing metadataGeneratorFile bean**
_Make sure to replace the entityId value with a string which
is unique within the SSO Circle service (e.g. urn:test:yourname:yourcity):_

```console
<bean id="metadataGeneratorFilter" class="org.springframework.security.saml.metadata.MetadataGeneratorFilter">
   <constructor-arg>
      <bean class="org.springframework.security.saml.metadata.MetadataGenerator">
         <property name="entityId" value="replaceWithUniqueIdentifier"/>
         <property name="extendedMetadata">
            <bean class="org.springframework.security.saml.metadata.ExtendedMetadata">
               <property name="signMetadata" value="false"/>
               <property name="idpDiscoveryEnabled" value="true"/>
            </bean>
         </property>
      </bean>
   </constructor-arg>
</bean>
```

#### Compilation

```console
# Gradle (doesn't work as of 9/12/2019)
# If building from the source
# Can find spring-security-saml2-sample.war in directory sample/build/libs
gradlew build install

# Maven (requires check for
# If building from the release zip
mvn package
```

#### As of 9/12/2019, There will be a compilation error when trying to compile

#### This means that you need to update the references to a certain compiler

```console
[ERROR] COMPILATION ERROR :
[INFO] -------------------------------------------------------------
[ERROR] Source option 6 is no longer supported. Use 7 or later.
[ERROR] Target option 6 is no longer supported. Use 7 or later.
...
```

#### Search within build and update 1.6 --> 1.7

```console
<plugin>
   <groupId>org.apache.maven.plugins</groupId>
   <artifactId>maven-compiler-plugin</artifactId>
   <configuration>
      <source>1.7</source>
      <target>1.7</target>
   </configuration>
</plugin>
```

#### Compile & Deploy

```console
mvn package
mvn tomcat7:run

# If you want to see more logging results from compilation, run this :
mvn -X package
```

**Tomcat7 is referenced in the build portion of the pom.xml**

```
<plugin>
   <groupId>org.apache.tomcat.maven</groupId>
   <artifactId>tomcat7-maven-plugin</artifactId>
   <version>2.2</version>
   <configuration>
       <path>/spring-security-saml2-sample</path>
   </configuration>
</plugin>
```

After startup the Spring SAML sample application will be available at  
http://localhost:8080/springsecurity-saml2-sample

[Spring Boot Tutorials : Mkyong](https://www.mkyong.com/tutorials/spring-boot-tutorials/)
[Intro to Spring Boot Web App : Lecture 10](https://bah.udemy.com/course/spring-framework-5-beginner-to-guru/learn/lecture/11162448#overview)
[Spring Framework Petclinic](https://github.com/spring-petclinic/spring-framework-petclinic)
[Spring Petclinic GraphQL](https://github.com/spring-petclinic/spring-petclinic-graphql)

https://spring.io/projects
https://spring.io/projects/spring-security#learn
https://spring.io/guides/gs/securing-web/
https://spring.io/guides/gs/spring-boot/
https://spring.io/projects/spring-boot#learn
https://spring.io/projects/spring-boot#samples
https://spring.io/guides/topicals/spring-security-architecture

https://docs.spring.io/spring-boot/docs/current/
https://docs.spring.io/spring-boot/docs/current/api/
https://docs.spring.io/spring-boot/docs/current/maven-plugin/
https://docs.spring.io/spring-boot/docs/current/actuator-api/html/
https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/

## Spring Best Practices

[Article](https://www.e4developer.com/2018/08/06/spring-boot-best-practices/)

1. Use auto-configuration
2. Use Spring Initializr for starting new Spring Boot projects
3. Consider creating your own auto-configuration for common organizational concerns
4. Structure your code correctly
5. Keep your @Controller's clean and focused
6. Build your @Service's around business capabilities
7. Make your database a detail - abstract it from the core logic
8. Keep your business logic free of Spring Boot code
9. Favour Constructor Injection
10. Be familiar with the concurrency model
11. Externalise and mature your configuration management
12. Provide global exception handling
13. Use a logging framework
14. Test your code
15. Use testing slices to make your testing easier and more focused

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
    - Add dependency to pom.xml
    - Access the H2 Console spring.h2.console.enabled=true
    - Navigate to http://localhost:8080/h2-console to view
    - The credentials can be specified in the application.properties file
    - [StackOverflow](https://stackoverflow.com/questions/55349373/database-not-found-and-ifexists-true-so-we-cant-auto-create-it)
      - 2019 Versions of H2 will not auto-create an in-memory database.
        - Use a pre-2019 version, defined in the pom.xml file
        - OR start the server, then change the field "JDBC URL:" from "jdbc:h2:~/test" to "jdbc:h2:mem:testdb"
        - [Reference](https://github.com/h2database/h2database/commit/8b53f3999c6c5c3d5ca29020e2657968f4f59ec4)
      - [H2 DB Example](https://howtodoinjava.com/spring-boot2/h2-database-example/)
      - [H2 DB Install & Tutorial](https://www.tutorialspoint.com/h2_database/h2_database_installation.htm)
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

- Group : com.example --> guru.springframework
- Artifact : demo --> spring5webapp
- Name : demo --> Same as Artifact
- Package Name : com.example.demo --> Group + Artifact (Name)
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
   | |   |_ application.properties <EMPTY>
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
  - application.properties should be in the classpath. Using Maven or Gradle, it should be under /src/main/resources
  - There we can add server.port = xxxx to change the default port from 8080
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

Common to see people create a new package for repositories.  
Spring Data JPA is a huge time saver compared to JDBC.  
Sets up interfaces with CRUD operations for the classes we want to store very quickly.

Make sure JDBC URL is : jdbc:h2:mem:testdb

## Spring MVC == Model View Controller

- MVC is a common design pattern for GUI and Web apps.
- Does a great job of separating concerns.

```
                           --> Model
                         /
Clients <--> Controller <
                         \
                           --> View
```

**Client** (mobile phone, browser, etc) makes requests to **Controller** (traffic cop for data)  
Controller goes out to get the **Model** (e.g. POJO),  
and returns the Model to the **View** (e.g. Thymeleaf templating engine).  
The View Component renders the view for the Client.

#### Spring MVC

```
                      / <--> Handler Mapping
                     /          --> Controller <--> Service <--> Data
                    /          /        |
Clients <--> Dispatch Servlet < <--Model|
                    \          \
                     \           --> View (JSP, Thymeleaf)
                      \                ^
                       \               |
                        \ ----> Model  |
```

**Web Request** comes in, which goes to a **Dispatch Servlet**.  
First goes to a **Handler Mapping** which determines which **Controller** to go to.  
The Controller typically has a Service Layer and Data wired up to it.  
The Controller gives the **Model**, which then gets rendered for the client in the **View**.

#### Thymeleaf template engine

Java has many template engines to choose from.  
Templates are a way to generate dynamic HTML.  
It is a "natural" template engine, meaning you can see it naturally in your browser.

## Software Layers

1. Views :  
   1a) JSP w/ custom tags
   - Boostrap (CSS)
     1b) webjars
   - custom LESS
2. Controller :
   - Spring @MVC annotations
     - Bean Validation
3. Service :
   - @Cacheable
     - @Transactional
4. Repository (Spring profiles) :
   - Spring Data JPA
   - JPA (default)
   - JDBC

[Spring Pet Clinic](https://github.com/spring-projects/spring-petclinic)

```
cd spring-petclinic
mvn spring-boot:run
```

Spring Boot brings a number of opinionated configurations, which makes it opinionated.  
Opinionated just means there is one way of doing things.  
Un-opinionated means then that you can do something however you choose.

Don't change the source code without an issue.  
Keep the visibility and traceability.

Dependency Injection == where Spring shines.  
Version control and dependency injection is the cornerstone of Spring.

#### S.O.L.I.D. OOP Principles

- From Robert Martin 1995
- 5 principles focus on dependency management
- avoid god classes

1. **Single Responsibility**
   - Every class should have 1 responsibility
   - Never should be more than 1 reason for a class to change
   - Classes should be small -- No God Classes
2. **Open-Closed**
   - Classes should be open for extension
   - BUT closed for modification
   - Use private variables w/ getters and setters - ONLY when you need them
   - Use abstract base classes
3. **Liskov Substitution**
   - Objects in a program would be replaceable w/ instances of their subtypes WITHOUT altering the correctness of the program
   - Violations will often fail the classification test
     - A square is a rectangle
     - But a rectangle is not a square : FAIL
4. **Interface Segregation**
   - Make fine-grained interfaces that are client specific
   - Many client-specific interfaces are better than one "general purpose" interface
   - Keep your components focused and minimize dependencies between them
5. **Dependency Inversion**
   - Would you solder a lamp directly to the electrical wiring in a wall rather than using a plug?
   - Abstractions should not upon the details
   - Details should not depend upon abstractions
   - Important that higher level and lower level objects depend on the same abstract interaction
   - This is NOT dependency injection

## Dependency Injection, DI

State your need, then the framework will make sure that you have it.  
This avoids issues caused by the user trying to get things himself.

- Dependency Injection is where a needed dependency is injected by another object.
- The class being injected has no responsibility in instantiating the object being injected.
- Some say you avoid declaring objects using 'new' but this isn't 100% correct...
- DI can be done with Concrete Classes or with Interfaces
  - Interfaces are preferred
    - Allows runtime to decide implementation to inject
    - Follows Interface Segregation Principle of S.O.L.I.D.
    - Makes the code more testable
  - Concrete Classes should be avoided

#### Three types of DI dependencies

1. By Property
2. By Setter
3. By Constructor
   - This requires the dependency to be injected when the class is instantiated.
   - Very bad practice.

- Is it good practice to use concrete classes for dependency injection?
  - You should use interfaces, which will allow the runtime environment to determine the implementation to inject.

## Inversion of Control, IoC

- Technique to allow dependencies to be injected at runtime
- Dependencies are not predetermined
- Gives frameworks the power to serve as extensible skeletons in applications
- The runtime environment (or framework) which injects dependencies

## IoC vs DI

- DI refers to the composition of you classes, i.e. you compose your classes with DI in mind
- IoC is the runtime environment of your code, i.e. Spring Framework's IoC container

## Spring Qualifier Annotations

## Primary Annotation for Spring Beans

## Getters and Setters

## Controllers

## Services

## Repositories

## Resources

## Beans

## Templates

## Models / POJOs

## Spring Profiles

## Spring Bean Life Cycle

1. Instantiate
2. Populate Properties
3. Call setBeanName of BeanNameAware
4. Call setBeanFactory of BeanFactoryAware
5. Call setApplicationContext of ApplicationContextAware
6. Preinitialization (Bean PostProcessors)
7. afterPropertiesSet of Initializing Beans
8. Custom Init Method
9. Post Initialization (BeanPostProcessors)
10. Bean is ready to use!

#### ~~~~

11. Container Shutdown
12. Disposable Bean's destroy() method
13. Call Custom Destroy Method
14. TERMINATED

## Callback Interfaces

- Spring has 2 interfaces you can implement for call back events
- InitializingBean.afterPropertiesSet()
  - called after properties are set
- DisposableBean.destroy()
  - Called during bean destruction in shutdown

## Bean Post Processors

- Gives you a means to tap into the Spring context life cycle and interact w/ beans as they are processed
- Implement interface BeanPostProcessor
  - postProcessBeforeInitialization : Called before bean initialization method
  - postProcessAfterInitialization : Called after bean initialization

## 'Aware' Interfaces

- Spring has over 14 'Aware' interfaces
- These are used to access the Spring Framework infrastructure
- These are largely used within the framework
- Rarely used by Spring developers
- ApplicationContextAware : Interface to be implemented by any object that wishes to be notified of the ApplicationContext that it runs in.
- ApplicationEventPublisherAware : Set the ApplicationEventPublisher that this object runs in.
- BeanClassLoaderAware : Callback that supplies the bean class loader to a bean instance.
- BeanFactoryAware : Callback that supplies the owning factory to a bean instance.
- BeanNameAware : Set the name of the bean in the bean factory that created this bean.
- BootstrapContextAware : Set the BootstrapContext that this object runs in.
- LoadTimeWeaverAware : Set the LoadTimeWeaver of this object's containing ApplicationContext.
- MessageSourceAware : Set the MessageSource that this object run in.
- NotificationPublisherAware : Set the NotificationPublisher instance for the current managed resource instance.
- PortletConfigAware : Set the PortletConfig this object runs in.
- PortletContextAware : Set the PortletContext that this object runs in.
- ResourceLoaderAware : Set the ResourceLoader that this object runs in.
- ServletConfigAware : Set the ServletConfig that this object runs in.
- ServletContextAware : Set the ServletContext that this object runs in.

* What is the annotation used in Spring to indicat you want a dependency injected?
  - @Autowired
* If you have 2 beans of the same type, how do you specify a preference for 1 over the other?
  - @Primary
* What are the 2 callback interfaces you can implement to tap into the bean lifecycle?
  1.  InitializingBean
  2.  DisposableBean
* What 2 annotations can be used to access the Spring Bean lifecycle?
  1.  @PostConstruct
  2.  @PreDestroy
* How do you specify a bean name you want injected?
  - Use the @Qualifier annotation
