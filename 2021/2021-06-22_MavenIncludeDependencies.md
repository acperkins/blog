# Including dependencies with Apache Maven
2021-06-22 — Anthony Perkins

---

When building a Java package with Maven, the build was succeeding but the JAR file was not running.
Instead it was throwing `java.lang.NoClassDefFoundError` and `java.lang.ClassNotFoundException`
errors.

```
Exception in thread "main" java.lang.NoClassDefFoundError: jakarta/json/Json
	at com.example.App.main(Test.java:8)
Caused by: java.lang.ClassNotFoundException: jakarta.json.Json
	at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:581)
	at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:178)
	at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:522)
	... 1 more
```

This is related to the dependencies not being found on the classpath. To work around this, it is
possible to bundle dependencies into the JAR with your application. This obviously makes the JAR
larger, but is more convenient for distribution and running on different machines.

To create a self-contained JAR with all dependencies, set Maven's `pom.xml` file similar to the
following:

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.example</groupId>
  <artifactId>App</artifactId>
  <version>1.0</version>

  <dependencies>
  </dependencies>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <maven.compiler.target>11</maven.compiler.target>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.release>11</maven.compiler.release>
  </properties>

  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-assembly-plugin</artifactId>
        <configuration>
          <archive>
            <manifest>
              <mainClass>com.example.App</mainClass>
            </manifest>
          </archive>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
          <appendAssemblyId>true</appendAssemblyId>
        </configuration>
        <executions>
          <execution>
            <id>make-assembly</id>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
</project>
```

Set `mainClass` (in this example, `com.example.App`) to the name of the startup class for your
application that has the `main()` function.

---

Copyright © 2021 Anthony Perkins. Some rights reserved.
