# Maven



## Maven

![figs/web/pom-relationships\_pom-small.png](https://books.sonatype.com/mvnref-book/reference/figs/web/pom-relationships\_pom-small.png)

Properties

```
${...}

env - environment variables - ${env.PATH}
project - any part of the POM - ${project.groupId}
settings - variables from settings.xml - ${settings.offline}

<properties>
    <foo>bar</foo>
</properties>

${foo}=bar
```

```
# mvn archetype:generate -DgroupId=ru.slavich -DartifactId=proj -Dversion=1.0-SNAPSHOT -DarchetypeArtifactId=maven-archetype-quickstart
```

```
# mvn clean <- remove all from target dir
```

![Image result for maven phases goals](https://i.stack.imgur.com/ciktU.png)

![Image result for maven phases goals](https://i.stack.imgur.com/Ub3Bd.png)

![Image result for maven phases goals](https://i.stack.imgur.com/MOw5a.png)

```
// maven phases of default lifecycle
- validate
- compile
- test
- package
- verify
- install
- deploy

// standart lifecycles
- clean
- default
- site
```

* dependencies
* plugins - provides goals
* phase
* goals - special task, which concerning with managing and packaging project. Can be tied to phases.
* profiles
* versions
* A goal not bound to any build phase coukd be executed by direct invocation

```
# mvn help:effective-pom
```

* If a phase has no goals bound to it, it phase will not not execute.
* If phase has one or more goals it will execute all those goals

```
```

| Phase             | Action               | Description                          |
| ----------------- | -------------------- | ------------------------------------ |
| prepare-resources | copying resources    | copying can be managed               |
| compile           | compiling            | source code compiling                |
| package           | creating jar/war/ear | type of archive specifyed in pom.xml |
| install           | install              | install archive to repository        |

### Packaging

```
<packaging>war</packaging> // jar war ear pom
```

* Each packaging contains a list of goals to bind to a phases

For example folowing goals binded to build phases in jar packaging

| **Phase**              | **plugin:goal**         |
| ---------------------- | ----------------------- |
| process-resources      | resources:resources     |
| compile                | compiler:compile        |
| process-test-resources | resources:testResources |
| test-compile           | compiler:testCompile    |
| test                   | surefire:test           |
| package                | jar:jar                 |
| install                | install:install         |
| deploy                 | deploy:deploy           |

### Plugins

General way to add goals to phases is configure plugins in pom.xml

Plugins are artifacts that provides goals.

Plugins can contain information phase binded goal to

If several goals bound to phase, the order used is first executed from packaging, followed by those configured in POM.

\<executions> element provide control over the order of paricular goals.

Binding example maven-anteun-plugin:run -> pre-clean

```
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-antrun-plugin</artifactId>
      <version>1.1</version>
      <executions>
        <execution>
          <id>id.pre-clean</id>
          <phase>pre-clean</phase>
          <goals>
            <goal>run</goal>
          </goals>
          <configuration>
            <tasks>
              <echo>pre-clean phase</echo>
            </tasks>
          </configuration>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>

# mvn pre-clean
```

Define plugin

```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-checkstyle-plugin</artifactId>
    <version>2.6</version>
</plugin>
```

Bind goal to phase

```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-checkstyle-plugin</artifactId>
    <version>2.6</version>
    <executions>
        <execution>
            <phase>package</phase>
            <goals>
                <goal>check</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

Configure plugin

```
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>tomcat-maven-plugin</artifactId>
    <version>1.1</version>
    <configuration>
        <fork>false</fork>
        <server>test-server</server>
        <url>http://test-server/manager</url>
    </configuration>
</plugin>
```

[http://java-online.ru/maven-plugins.xhtml](http://java-online.ru/maven-plugins.xhtml)

### maven-compiler-plugin

```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.1</version>
    <configuration>
        <source>1.8</source>
        <target>1.8</target>
        <encoding>UTF-8</encoding>
        <executable>${JAVA_HOME}/bin/javac</executable>
        <compileArgs>
            <arg>-verbose</arg>
            <arg>-Xlint:all,-options,-path</arg>
        </compileArgs>
    </configuration>
</plugin>
```

* available by default
* target - jvm version
* executable - path to javac compiler
* compileArgs

Has two goals

* compiler:compile - bound with compile phase by default
* compiler:testCompile - test-compile phase

Do compile by non java compiler

```
<plugin>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.1</version>
    <configuration>
        <compilerId>csharp</compilerId>
    </configuration>
    <dependencies>
        <dependency>
            <groupId>org.codehaus.plexus</groupId>
            <artifactId>plexus-compiler-csharp</artifactId>
            <version>1.6</version>
        </dependency>
    </dependencies>
</plugin>
```

### maven-resources-plugin

```
<plugin>
    <artifactId>maven-resources-plugin</artifactId>
    <version>2.6</version>
    <executions>
        <execution>
            <id>copy-resources</id>
            <phase>validate</phase>
            <goals>
                <goal>copy-resources</goal>
            </goals>
            <configuration>
                <outputDirectory>
                       ${basedir}/target/resources
                </outputDirectory>
                <resources>
                    <resource>
                        <directory>src/main/resources/props</directory>
                        <filtering>true</filtering>
                        <includes>
                            <include>**/*.properties</include>
                        </includes>
                    </resource>
                    <resource>
                        <directory>src/main/resources/images</directory>
                        <includes>
                            <include>**/*.png</include>
                        </includes>
                    </resource>
                </resources>
            </configuration>
        </execution>
    </executions>
</plugin>
```

* Intended for copying resources to directory target
* \<outputDirectory>
* \<resources> - from directory
* \<filtering>

### maven-source-plugin

```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-source-plugin</artifactId>
    <version>2.2.1</version>
    <executions>
        <execution>
            <id>attach-sources</id>
            <phase>verify</phase>
            <goals>
                <goal>jar</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

* Intended to attach sorces to final assembly

### maven-dependency-plugin

```
<plugin> 
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-dependency-plugin</artifactId>
    <version>2.5.1</version>
    <configuration>
        <outputDirectory>
            ${project.build.directory}/lib/
        </outputDirectory>
        <overWriteReleases>false</overWriteReleases>
        <overWriteSnapshots>false</overWriteSnapshots>
        <overWriteIfNewer>true</overWriteIfNewer>
    </configuration>
    <executions>
        <execution> 
            <id>copy-dependencies</id>
            <phase>package</phase>
            <goals>
                <goal>copy-dependencies</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

* Intended for copying dependencies to assembly directory

Has several goals:

* dependendency:analyze
* dependency:analyze-duplicate
* dependency:resolve
* dependency:resolve-plugin
* dependency:tree

### maven-jar-plugin

```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <version>2.4</version>
    <configuration>
        <includes>
            <include>**/properties/*</include>
        </includes>
        <excludes>
            <exclude>**/*.png</exclude>
        </excludes>
        <archive>
           <manifestFile>src/main/resources/META-INF/MANIFEST.MF</manifestFile>
        </archive>
    </configuration>
</plugin>
```

* Form manifest
* describe required resources
* Assembly project to jar archive

```
<configuration>
    <archive>
        <manifest>
            <addClasspath>true</addClasspath>
            <classpathPrefix>lib/</classpathPrefix>
            <mainClass>ru.company.AppMain</mainClass>
        </manifest>
    </archive>
</configuration>
```

## Inheritance project

In parent

```
<modules>
    <module>project1</module>
    <module>project2</module>
</modules>
```

In child

```
<parent>
    <groupId>com.example</groupId>
    <artifactId>project</artifactId>
    <version>0.0.1</version>
</parent>
```
