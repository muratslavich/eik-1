# Properties

## Properties

* via Java configuration
* @PropertySource
* via XML \<property-placeholder>

**Register a Properties File via annotation in Java configuration**

```
@Configuration
@PropertySource("classpath:foo.properties")
public class PropertiesWithJavaConfig {
    //...
}
```

using placeholder to dynamically select right file at runtime

```
@PropertySource({ 
  "classpath:persistence-${envTarget:mysql}.properties"
})
```

multiple property locations

```
@PropertySource("classpath:foo.properties")
@PropertySource("classpath:bar.properties")
public class PropertiesWithJavaConfig {
    //...
}

@PropertySources({
    @PropertySource("classpath:foo.properties"),
    @PropertySource("classpath:bar.properties")
})
public class PropertiesWithJavaConfig {
    //...
}
```

### **Register a Properties File in XML**

```
<context:property-placeholder location="classpath:foo.properties, classpath:bar.properties"/>
```

### **Using / Injecting Properties**

```
@Value( "${jdbc.url}" )
private String jdbcUrl;

@Value( "${jdbc.url:DefaultUrl}" )
private String jdbcUrl;

-----
<bean id="dataSource">
  <property name="url" value="${jdbc.url}" />
</bean>

-----
@Autowired
private Environment env;
...
dataSource.setUrl(env.getProperty("jdbc.url"));
```

In Spring boot application.properties is the default property file which will auto detected in src/mai/resources directory.

We don\`t have to explicitly register them.

## @Value

It can be used for injecting values into Spring managed beans/

Needs properties file and need to define @PropertySource in configurations.

usage:

```
@Value("string value")
private String stringValue;

@Value("${value.from.file}")     // if @PropertySource is defined
private String valueFromFile;

@Value("${systemValue}")
private String systemValue;

@Value("${unknown.param:some default}")
private String someDefault;

@Value("${listOfValues}")
private String[] valuesArray;
```

### **Using **_**@Value**_** with **_**Maps**_

```
valuesMap={key1: '1', key2: '2', key3: '3'}

@Value("#{${valuesMap}}")
private Map<String, Integer> valuesMap;

@Value("#{${valuesMap}.key1}")
private Integer valuesMapKey1;

@Value("#{${valuesMap}['unknownKey']}")
private Integer unknownMapKey;

@Value("#{${valuesMap}.?[value>'1']}")
private Map<String, Integer> valuesMapFiltered;

@Value("#{systemProperties}")
private Map<String, String> systemPropertiesMap; // all system properties
```
