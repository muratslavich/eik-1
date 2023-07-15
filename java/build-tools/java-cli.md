# java cli

```
- One file
# javac ./src/Main.java
# java -classpath ./src Main

-d destination folder (bin)
# javac -d ./bin ./src/Main.java

for multiple source files compilation needs to specify -sourcepath
# javac -sourcepath ./src -d ./bin ./src/com/slavich/logic/Main.java

use 'package com.slavich.logic;'
# java -classpath ./bin com.slavich.logic.Main

start executable jar
# java -jar path/to/program.jar

start non executable jar
# java -classpath path/to/program.jar package.MainClass

start debugger
# jdb -classpath bin -sourcepath src logic.Main
-- stop at logic.Main:9
-- run
-- list
-- print a
-- dump a
-- cont
-- step
-- step up
-- next
-- eval a.calculate()



link external jar ;: - разделитель
# javac -classpath lib/path/junit-4.8.2.jar -sourcepath ./src -d test_bin test/com/qwertovsky/helloworld/TestCalculator.java
# java -classpath lib/path/junit-4.8.2.jar:./test_bin org.junit.runner.JUnitCore com.qwertovsky.helloworld.TestCalculator

create jar archive
# jar cvf project.jar -C bin .
-C bin .   <- executing archive in bin directory 

start with linked jar
# java -classpath bin:path/to/calculator.jar com.qwertovsky.helloworld.HelloWorld



assembly executable jar
# echo main-class: com.qwertovsky.helloworld.HelloWorld>manifest.mf
# echo class-path: lib/calculator.jar >>manifest.mf
# mkdir lib
# cp path/to/calculator.jar lib/calculator.jar
# jar -cmf manifest.mf helloworld.jar  -C bin .
--
# jar -cmef manifest.mf com.qwertovsky.helloworld.HelloWorld helloworld.jar -C bin .
--
# jar -cef com.qwertovsky.helloworld.HelloWorld helloworld.jar -C lib .
```

{% embed url="https://docs.oracle.com/javase/6/docs/technotes/tools/solaris/jar.html" %}

{% embed url="https://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html" %}
