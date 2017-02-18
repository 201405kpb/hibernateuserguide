# Custom BasicTypes

Hibernate makes it relatively easy for developers to create their own basic type mappings type.
For example, you might want to persist properties of type `java.util.BigInteger` to `VARCHAR` columns, or support completely new types.
Hibernate使开发人员能够相对容易地创建自己的基本类型映射类型。 例如，您可能希望将类型java.util.BigInteger的属性保留到VARCHAR列，或者支持完全新的类型。

There are two approaches to developing a custom type:
有两种方法来开发自定义类型：

>*   implementing a `BasicType` and registering it
>*    实现一个BasicType并注册它
>*   implement a `UserType` which doesn&#8217;t require type registration
>*    实现不需要类型注册的“UserType”
 
 As a means of illustrating the different approaches, let&#8217;s consider a use case where we need to support a `java.util.BitSet` mapping that&#8217;s stored as a VARCHAR.
 为了说明不同的方法，让我们考虑一个用例，我们需要支持一个存储为VARCHAR的java.util.BitSet映射。


## Implementing a `BasicType`
## 实现 `BasicType`接口

The first approach is to directly implement the `BasicType` interface. Because the `BasicType` interface has a lot of methods to implement, it&#8217;s much more convenient to extend the `AbstractStandardBasicType`,or the `AbstractSingleColumnStandardBasicType` if the value is stored in a single database column.
第一种方法是直接实现`BasicType`接口。 因为`BasicType`接口有很多实现方法，所以如果值存储在单个数据库列中，那么扩展`AbstractStandardBasicType`或`AbstractSingleColumnStandardBasicType`将会更加方便。

First, we need to extend the `AbstractSingleColumnStandardBasicType` like this:
首先,我们需要像以下方式实现`AbstractSingleColumnStandardBasicType`：

**_Example 7. Custom `BasicType` implementation_**
**_例 7. 自定义`BasicType` 接口的实现_**
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
`AbstractSingleColumnStandardBasicType`需要一个`sqlTypeDescriptor`和`javaTypeDescriptor`。
`sqlTypeDescriptor'是`VarcharTypeDescriptor.INSTANCE`，因为数据库列是一个VARCHAR。
在Java方面，我们需要使用一个`BitSetTypeDescriptor`实例，它可以这样实现：

Example 8. Custom AbstractTypeDescriptor implementation
```java
public class BitSetTypeDescriptor extends AbstractTypeDescriptor<BitSet> {

    private static final String DELIMITER = ",";

    public static final BitSetTypeDescriptor INSTANCE = new BitSetTypeDescriptor();

    public BitSetTypeDescriptor() {
        super( BitSet.class );
    }

    @Override
    public String toString(BitSet value) {
        StringBuilder builder = new StringBuilder();
        for ( long token : value.toLongArray() ) {
            if ( builder.length() > 0 ) {
                builder.append( DELIMITER );
            }
            builder.append( Long.toString( token, 2 ) );
        }
        return builder.toString();
    }

    @Override
    public BitSet fromString(String string) {
        if ( string == null || string.isEmpty() ) {
            return null;
        }
        String[] tokens = string.split( DELIMITER );
        long[] values = new long[tokens.length];

        for ( int i = 0; i < tokens.length; i++ ) {
            values[i] = Long.valueOf( tokens[i], 2 );
        }
        return BitSet.valueOf( values );
    }

    @SuppressWarnings({"unchecked"})
    public <X> X unwrap(BitSet value, Class<X> type, WrapperOptions options) {
        if ( value == null ) {
            return null;
        }
        if ( BitSet.class.isAssignableFrom( type ) ) {
            return (X) value;
        }
        if ( String.class.isAssignableFrom( type ) ) {
            return (X) toString( value);
        }
        throw unknownUnwrap( type );
    }

    public <X> BitSet wrap(X value, WrapperOptions options) {
        if ( value == null ) {
            return null;
        }
        if ( String.class.isInstance( value ) ) {
            return fromString( (String) value );
        }
        if ( BitSet.class.isInstance( value ) ) {
            return (BitSet) value;
        }
        throw unknownWrap( value.getClass() );
    }
}
```

The `unwrap` method is used when passing a `BitSet` as a `PreparedStatement` bind parameter, while the `wrap` method is used to transform the JDBC column value object (e.g. `String` in our case) to the actual mapping object type (e.g. `BitSet` in this example).


The `BasicType` must be registered, and this can be done at bootstrapping time:





Example 9. Register a Custom `BasicType` implementation
```java
configuration.registerTypeContributor( (typeContributions, serviceRegistry) -> {
    typeContributions.contributeType( BitSetType.INSTANCE );
} );
```
  
  or using the `MetadataBuilder`
```java
ServiceRegistry standardRegistry =
    new StandardServiceRegistryBuilder().build();

MetadataSources sources = new MetadataSources( standardRegistry );

MetadataBuilder metadataBuilder = sources.getMetadataBuilder();

metadataBuilder.applyBasicType( BitSetType.INSTANCE );
```


 With the new `BitSetType` being registered as `bitset`, the entity mapping looks like this:

  

Example 10. Custom `BasicType` mapping
```java
@Entity(name = "Product")
public static class Product {

    @Id
    private Integer id;

    @Type( type = "bitset" )
    private BitSet bitSet;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public BitSet getBitSet() {
        return bitSet;
    }

    public void setBitSet(BitSet bitSet) {
        this.bitSet = bitSet;
    }
}
```
  
To validate this new `BasicType` implementation, we can test it as follows:

Example 11. Persisting the custom `BasicType`
```java
BitSet bitSet = BitSet.valueOf( new long[] {1, 2, 3} );

doInHibernate( this::sessionFactory, session -> {
    Product product = new Product( );
    product.setId( 1 );
    product.setBitSet( bitSet );
    session.persist( product );
} );

doInHibernate( this::sessionFactory, session -> {
    Product product = session.get( Product.class, 1 );
    assertEquals(bitSet, product.getBitSet());
} );
```
  

When executing this unit test, Hibernate generates the following SQL statements:

Example 12. Persisting the custom `BasicType`
  ```java
  DEBUG SQL:92 -
    insert
    into
        Product
        (bitSet, id)
    values
        (?, ?)

TRACE BasicBinder:65 - binding parameter [1] as [VARCHAR] - [{0, 65, 128, 129}]
TRACE BasicBinder:65 - binding parameter [2] as [INTEGER] - [1]

DEBUG SQL:92 -
    select
        bitsettype0_.id as id1_0_0_,
        bitsettype0_.bitSet as bitSet2_0_0_
    from
        Product bitsettype0_
    where
        bitsettype0_.id=?

TRACE BasicBinder:65 - binding parameter [1] as [INTEGER] - [1]
TRACE BasicExtractor:61 - extracted value ([bitSet2_0_0_] : [VARCHAR]) - [{0, 65, 128, 129}]
  ```

As you can see, the `BitSetType` takes care of the _Java-to-SQL_ and _SQL-to-Java_ type conversion.

  ## Implementing a `UserType`

    

The second approach is to implement the `UserType` interface.

Example 13. Custom `UserType` implementation
```java
public class BitSetUserType implements UserType {

	public static final BitSetUserType INSTANCE = new BitSetUserType();

    private static final Logger log = Logger.getLogger( BitSetUserType.class );

    @Override
    public int[] sqlTypes() {
        return new int[] {StringType.INSTANCE.sqlType()};
    }

    @Override
    public Class returnedClass() {
        return String.class;
    }

    @Override
    public boolean equals(Object x, Object y)
			throws HibernateException {
        return Objects.equals( x, y );
    }

    @Override
    public int hashCode(Object x)
			throws HibernateException {
        return Objects.hashCode( x );
    }

    @Override
    public Object nullSafeGet(
            ResultSet rs, String[] names, SharedSessionContractImplementor session, Object owner)
            throws HibernateException, SQLException {
        String columnName = names[0];
        String columnValue = (String) rs.getObject( columnName );
        log.debugv("Result set column {0} value is {1}", columnName, columnValue);
        return columnValue == null ? null :
				BitSetTypeDescriptor.INSTANCE.fromString( columnValue );
    }

    @Override
    public void nullSafeSet(
            PreparedStatement st, Object value, int index, SharedSessionContractImplementor session)
            throws HibernateException, SQLException {
        if ( value == null ) {
            log.debugv("Binding null to parameter {0} ",index);
            st.setNull( index, Types.VARCHAR );
        }
        else {
            String stringValue = BitSetTypeDescriptor.INSTANCE.toString( (BitSet) value );
            log.debugv("Binding {0} to parameter {1} ", stringValue, index);
            st.setString( index, stringValue );
        }
    }

    @Override
    public Object deepCopy(Object value)
			throws HibernateException {
        return value == null ? null :
            BitSet.valueOf( BitSet.class.cast( value ).toLongArray() );
    }

    @Override
    public boolean isMutable() {
        return true;
    }

    @Override
    public Serializable disassemble(Object value)
			throws HibernateException {
        return (BitSet) deepCopy( value );
    }

    @Override
    public Object assemble(Serializable cached, Object owner)
			throws HibernateException {
        return deepCopy( cached );
    }

    @Override
    public Object replace(Object original, Object target, Object owner)
			throws HibernateException {
        return deepCopy( original );
    }
}
```
 

The entity mapping looks as follows:

   
Example 14. Custom `UserType` mapping
```java
@Entity(name = "Product")
public static class Product {

    @Id
    private Integer id;

    @Type( type = "bitset" )
    private BitSet bitSet;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public BitSet getBitSet() {
        return bitSet;
    }

    public void setBitSet(BitSet bitSet) {
        this.bitSet = bitSet;
    }
}
```
 
In this example, the `UserType` is registered under the `bitset` name, and this is done like this:

Example 15. Register a Custom `UserType` implementatio
```java
configuration.registerTypeContributor( (typeContributions, serviceRegistry) -> {
    typeContributions.contributeType( BitSetUserType.INSTANCE, "bitset");
} );
```

or using the `MetadataBuilder`
```java
ServiceRegistry standardRegistry =
    new StandardServiceRegistryBuilder().build();

MetadataSources sources = new MetadataSources( standardRegistry );

MetadataBuilder metadataBuilder = sources.getMetadataBuilder();

metadataBuilder.applyBasicType( BitSetUserType.INSTANCE, "bitset" );
```
   
Like `BasicType`, you can also register the `UserType` using a simple name.

    
Without registration, the `UserType` mapping requires the fully-classified name:

   
When running the previous test case against the `BitSetUserType` entity mapping, Hibernate executed the following SQL statements:

Example 16. Persisting the custom `BasicType`
```java
DEBUG SQL:92 -
    insert
    into
        Product
        (bitSet, id)
    values
        (?, ?)

DEBUG BitSetUserType:71 - Binding 1,10,11 to parameter 1
TRACE BasicBinder:65 - binding parameter [2] as [INTEGER] - [1]

DEBUG SQL:92 -
    select
        bitsetuser0_.id as id1_0_0_,
        bitsetuser0_.bitSet as bitSet2_0_0_
    from
        Product bitsetuser0_
    where
        bitsetuser0_.id=?

TRACE BasicBinder:65 - binding parameter [1] as [INTEGER] - [1]
DEBUG BitSetUserType:56 - Result set column bitSet2_0_0_ value is 1,10,11
```
  