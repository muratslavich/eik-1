# Gradle



```
- gradle init
- gradle build
- gradle run
```

```
|-build.gradle
|-gradle
    |-wrapper
        |-gradle-wrapper.jar
        |-gradle-wrapper.properties
|-gradlew
|-gradlew.bat
|-settings.gradle    -settings script for configuring the Gradle build
```

```
apply plugin: 'java'

sourceCompability = 1.8
targetCompability = 1.8

// to make output jar runnable
apply plugin: 'application'
mainClassName = 'hello.HelloWorld'

// declare dependencies
repositories {
    mavenCentral()
}

dependencies {
    compile "joda-time:joda-time:2.2"
}
// compile - it should be available during compile time (For WAR included in /WEB-INF/libs)
// providedCompile - required for compiling project, but will be provided by container running the code
// testCompile - it used for compiling and running tests

// specify artifact
jar {
    baseName = 'gs-gradle'
    version = '0.1.0'
}
```



### Gradle Wrapper

* Preffered way of gradle build
* Consists of a batch script which allow to run a Gradle build without installation on system

```
# gradle wrapper --gradle-version 2.13

// it create few new files and folder
-gradlew
-gradlew.bat
-gradle
--wrapper
---gradle-wrapper.jar
---gradle-wrapper.properties

// run the wrapper script to perform build task
# ./gradlew build
# ./gradlew run
```

```
./gradlew package:dependencies
```
