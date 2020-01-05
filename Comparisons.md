## Build Tools

#### Gradle
- Based on `Groovy`  
- Adopted by Google as the build system for Android projects.  
- Incremental compilations for Java classes
- Compile avoidance for Java
- The use of APIs for incremental subtasks
- A compiler daemon that also makes compiling a lot faster
Has JCenter  

#### Maven
XML based  
Has Maven Central.  

### [Gradle vs Maven](https://stackify.com/gradle-vs-maven/)
Gradle is based on a graph of task dependencies, where tasks are the things that do work.  
Maven is based on a fixed and linear model of phases.  

Both allow for multi-module builds to run in parallel.  
Gradle allows for incremental builds because it checks which tasks are updated or not.  
If it is, the task is not executed, giving you a much shorter build time.  
Both can handle dynamic and transitive dependencies.  

### [Maven chosen over Gradle](https://www.softwareyoga.com/10-reasons-why-we-chose-maven-over-gradle/) 
### [Convert Maven to Gradle](https://dzone.com/articles/how-to-convert-maven-to-gradle-and-vice-versa)
### [SO Convert Gradle to Maven](https://stackoverflow.com/questions/58188348/convert-gradle-to-maven)

## Configurations

### [YAML vs Properties File](https://javajee.com/a-quick-comparison-of-yaml-with-properties-file)
