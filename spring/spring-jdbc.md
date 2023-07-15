# Spring Jdbc

```
compile("org.springframework:spring-jdbc:5.2.1.RELEASE")
```

org.springframework.jdbc.core

* JdbcTemplate
* simple
  * SimpleJdbcInsert
  * SimpleJdbcCall
* namedparam
  * NamedParameterJdbcTemplate

org.springframework.jdbc.datasource

* DataSource
* embedded

org.springframework.jdbc.object

org.springframework.jdbc.support

## JdbcTemplate

Handle creation and release of resources, avoid common errors.

Statement creation and execution, executes SQL and extract results.

DataSource should be configured as a bean.

usage:

```
jdbcTemplate.queryForObject("select count(*) from table", Integer.class);

jdbcTemplate.queryForObject(
        "select count(*) from t_actor where first_name = ?", Integer.class, "Joe");

jdbcTemplate.queryForObject(
        "select last_name from t_actor where id = ?",
        new Object[]{1212L}, 
        String.class);

jdbcTemplate.queryForObject(
        "select first_name, last_name from t_actor where id = ?",
        new Object[]{1212L},
        new RowMapper<Actor>() {
            public Actor mapRow(ResultSet rs, int rowNum) throws SQLException {
                Actor actor = new Actor();
                actor.setFirstName(rs.getString("first_name"));
                actor.setLastName(rs.getString("last_name"));
                return actor;
            }
        });

jdbcTemplate.queryForObject(
        "select first_name, last_name from t_actor where id = ?",
        new Object[]{1212L},
        (rs, rowNum) -> {
                Actor actor = new Actor();
                actor.setFirstName(rs.getString("first_name"));
                actor.setLastName(rs.getString("last_name"));
                return actor;
        });

jdbcTemplate.update(
        "update t_actor set last_name = ? where id = ?",
        "Banjo", 5276L);

jdbcTemplate.execute("create table mytable (id integer, name varchar(100))");
```

JdbcTemplate class are threadsafe - you can configure single instance of a JdbcTemplate.

Common practies:

* configure DataSource in Spring
* inject DataSource bean into DAO classes
* JdbcTemplate is created in the setter for the DataSource

```
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
        <property name="driverClassName" value="${jdbc.driverClassName}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
</bean>
<context:property-placeholder location="jdbc.properties"/>

@Repository
public class SomeDaoImpl implements SomeDao {

    private JdbcTemplate jdbcTemplate;

    public void setDataSource(DataSource dataSource) {
        this.jdbcTemplate = new JdbcTemplate(dataSource);
    }

    // JDBC-backed implementations of the methods on the CorporateEventDao follow...
}
```

### **Testing data access logic with an embedded database**

```
public class DataAccessUnitTestTemplate {

    private EmbeddedDatabase db;

    @Before
    public void setUp() {
        // creates an HSQL in-memory database populated from default scripts
        // classpath:schema.sql and classpath:data.sql
        db = new EmbeddedDatabaseBuilder().addDefaultScripts().build();
    }

    @Test
    public void testDataAccess() {
        JdbcTemplate template = new JdbcTemplate(db);
        template.query(...);
    }

    @After
    public void tearDown() {
        db.shutdown();
    }

}
```

**Embedded db configuration**

[https://www.mkyong.com/spring/spring-embedded-database-examples/](https://www.mkyong.com/spring/spring-embedded-database-examples/)

```
<jdbc:embedded-database id="dataSource" type="H2">
		<jdbc:script location="classpath:db/sql/create-db.sql" />
		<jdbc:script location="classpath:db/sql/insert-data.sql" />
</jdbc:embedded-database>
```
