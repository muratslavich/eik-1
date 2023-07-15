# Page 1



{% embed url="https://docs.gradle.org/current/userguide/plugin_reference.html" %}

Java Plugin

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>



## Application Plugin

Java plugin + Distribution plugin

Executable JVM application and package the application as a TAR and/or ZIP

```kotlin
plugins {
    application
}

application {
    mainModule.set("org.gradle.sample.app") // name defined in module-info.java
    mainClass.set("org.gradle.sample.Main")
    applicationDefaultJvmArgs = listOf("-Dgreeting.language=en")
    executableDir = "custom_bin_dir"
}

applicationDistribution.from("some/dir") {
  include "*.txt"
}
```

#### Properties <a href="#n11d42" id="n11d42"></a>

| Property                                                                                                                                                                        | Description                                                               |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| [`applicationDefaultJvmArgs`](https://docs.gradle.org/current/dsl/org.gradle.api.plugins.JavaApplication.html#org.gradle.api.plugins.JavaApplication:applicationDefaultJvmArgs) | Array of string arguments to pass to the JVM when running the application |
| [`applicationDistribution`](https://docs.gradle.org/current/dsl/org.gradle.api.plugins.JavaApplication.html#org.gradle.api.plugins.JavaApplication:applicationDistribution)     | The specification of the contents of the distribution.                    |
| [`applicationName`](https://docs.gradle.org/current/dsl/org.gradle.api.plugins.JavaApplication.html#org.gradle.api.plugins.JavaApplication:applicationName)                     | The name of the application.                                              |
| [`executableDir`](https://docs.gradle.org/current/dsl/org.gradle.api.plugins.JavaApplication.html#org.gradle.api.plugins.JavaApplication:executableDir)                         | Directory to place executables in                                         |
| [`mainClass`](https://docs.gradle.org/current/dsl/org.gradle.api.plugins.JavaApplication.html#org.gradle.api.plugins.JavaApplication:mainClass)                                 | The fully qualified name of the application's main class.                 |
| [`mainModule`](https://docs.gradle.org/current/dsl/org.gradle.api.plugins.JavaApplication.html#org.gradle.api.plugins.JavaApplication:mainModule)                               | The name of the application's Java module if it should run as a module.   |



