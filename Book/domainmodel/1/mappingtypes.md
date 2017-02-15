# 类型映射

Hibernate understands both the Java and JDBC representations of application data.  
The ability to read/write this data from/to the database is the function of a Hibernate _type_.  
A type, in this usage, is an implementation of the `org.hibernate.type.Type` interface.  
This Hibernate type also describes various aspects of behavior of the Java type such as how to check for equality, how to clone values, etc.

The Hibernate type is neither a Java type nor a SQL data type.  
It provides information about both of these as well as understanding marshalling between.

When you encounter the term type in discussions of Hibernate, it may refer to the Java type, the JDBC type, or the Hibernate type, depending on context.

To help understand the type categorizations, let’s look at a simple table and domain model that we wish to map.

```sql
create table Contact (
    id integer not null,
    first varchar(255),
    last varchar(255),
    middle varchar(255),
    notes varchar(255),
    starred boolean not null,
    website varchar(255),
    primary key (id)
)
```
```java
public static class Contact {

    @Id
    private Integer id;

    private Name name;

    private String notes;

    private URL website;

    private boolean starred;

    //Getters and setters are omitted for brevity
}

@Embeddable
public class Name {

    private String first;

    private String middle;

    private String last;

    // getters and setters omitted
}
```

In the broadest sense, Hibernate categorizes types into two groups:
* [Value types](categorizationvalue.md)
* [Entity types](#categorization-entity)



