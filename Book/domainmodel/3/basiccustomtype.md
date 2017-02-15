# Custom BasicTypes

Hibernate makes it relatively easy for developers to create their own basic type mappings type.
For example, you might want to persist properties of type `java.util.BigInteger` to `VARCHAR` columns, or support completely new types.

There are two approaches to developing a custom type:

*   implementing a `BasicType` and registering it
*   implement a `UserType` which doesn&#8217;t require type registration
 
 As a means of illustrating the different approaches, let&#8217;s consider a use case where we need to support a `java.util.BitSet` mapping that&#8217;s stored as a VARCHAR.


## Implementing a `BasicType`

The first approach is to directly implement the `BasicType` interface.
Because the `BasicType` interface has a lot of methods to implement, it&#8217;s much more convenient to extend the `AbstractStandardBasicType`,or the `AbstractSingleColumnStandardBasicType` if the value is stored in a single database column.

First, we need to extend the `AbstractSingleColumnStandardBasicType` like this:
Example 7. Custom `BasicType` implementation
```java
public class BitSetType
        extends AbstractSingleColumnStandardBasicType<BitSet>
        implements DiscriminatorType<BitSet> {

    public static final BitSetType INSTANCE = new BitSetType();

    public BitSetType() {
        super( VarcharTypeDescriptor.INSTANCE, BitSetTypeDescriptor.INSTANCE );
    }

    @Override
    public BitSet stringToObject(String xml) throws Exception {
        return fromString( xml );
    }

    @Override
    public String objectToSQLString(BitSet value, Dialect dialect) throws Exception {
        return toString( value );
    }

    @Override
    public String getName() {
        return "bitset";
    }

}
```
   
The `AbstractSingleColumnStandardBasicType` requires an `sqlTypeDescriptor` and a `javaTypeDescriptor`.
The `sqlTypeDescriptor` is `VarcharTypeDescriptor.INSTANCE` because the database column is a VARCHAR.
On the Java side, we need to use a `BitSetTypeDescriptor` instance which can be implemented like this:

The `unwrap` method is used when passing a `BitSet` as a `PreparedStatement` bind parameter, while the `wrap` method is used to transform the JDBC column value object (e.g. `String` in our case) to the actual mapping object type (e.g. `BitSet` in this example).


The `BasicType` must be registered, and this can be done at bootstrapping time:





Example 9. Register a Custom `BasicType` implementation
  
  or using the `MetadataBuilder`



 With the new `BitSetType` being registered as `bitset`, the entity mapping looks like this:

  

Example 10. Custom `BasicType` mapping
  
To validate this new `BasicType` implementation, we can test it as follows:

Example 11. Persisting the custom `BasicType`
  

When executing this unit test, Hibernate generates the following SQL statements:

Example 12. Persisting the custom `BasicType`
  

As you can see, the `BitSetType` takes care of the _Java-to-SQL_ and _SQL-to-Java_ type conversion.

  ## Implementing a `UserType`

    

The second approach is to implement the `UserType` interface.

Example 13. Custom `UserType` implementation
 

The entity mapping looks as follows:

   
Example 14. Custom `UserType` mapping
 
In this example, the `UserType` is registered under the `bitset` name, and this is done like this:

Example 15. Register a Custom `UserType` implementatio

or using the `MetadataBuilder`

   
Like `BasicType`, you can also register the `UserType` using a simple name.

    
Without registration, the `UserType` mapping requires the fully-classified name:

   
When running the previous test case against the `BitSetUserType` entity mapping, Hibernate executed the following SQL statements:

Example 16. Persisting the custom `BasicType`
  