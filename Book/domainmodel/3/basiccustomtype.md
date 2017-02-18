# Custom BasicTypes

Hibernate makes it relatively easy for developers to create their own basic type mappings type.

For example, you might want to persist properties of type `java.util.BigInteger` to `VARCHAR` columns, or support completely new types.

Hibernate使开发人员能够相对容易地创建自己的基本类型映射类型。 例如，您可能希望将类型java.util.BigInteger的属性保留到VARCHAR列，或者支持完全新的类型。

There are two approaches to developing a custom type:

有两种方法来开发自定义类型：

> * implementing a `BasicType` and registering it
> 
> * 实现一个BasicType并注册它
> 
> * implement a `UserType` which doesn’t require type registration
> 
> * 实现不需要类型注册的“UserType”

As a means of illustrating the different approaches, let’s consider a use case where we need to support a `java.util.BitSet` mapping that’s stored as a VARCHAR.

为了说明不同的方法，让我们考虑一个用例，我们需要支持一个存储为VARCHAR的java.util.BitSet映射。

## Implementing a `BasicType`

## 实现 `BasicType`接口

The first approach is to directly implement the `BasicType` interface. Because the `BasicType` interface has a lot of methods to implement, it’s much more convenient to extend the `AbstractStandardBasicType`,or the `AbstractSingleColumnStandardBasicType` if the value is stored in a single database column.

第一种方法是直接实现`BasicType`接口。 因为`BasicType`接口有很多实现方法，所以如果值存储在单个数据库列中，那么扩展`AbstractStandardBasicType`或`AbstractSingleColumnStandardBasicType`将会更加方便。

First, we need to extend the `AbstractSingleColumnStandardBasicType` like this:

首先,我们需要像以下方式实现`AbstractSingleColumnStandardBasicType`：

_**Example 7. Custom **_`BasicType`_** implementation**_

_**例 7. 自定义**_`BasicType`_** 接口的实现**_

```java

public class BitSetType extends AbstractSingleColumnStandardBasicType<BitSet> 
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

`AbstractSingleColumnStandardBasicType`需要一个`sqlTypeDescriptor`和`javaTypeDescriptor`。`sqlTypeDescriptor'是`VarcharTypeDescriptor.INSTANCE\`，因为数据库列是一个VARCHAR。在Java方面，我们需要使用一个`BitSetTypeDescriptor`实例，它可以这样实现：

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

The `unwrap` method is used when passing a `BitSet` as a `PreparedStatement` bind parameter, while the `wrap` method is used to transform the JDBC column value object \(e.g. `String` in our case\) to the actual mapping object type \(e.g. `BitSet` in this example\).

The `BasicType` must be registered, and this can be done at bootstrapping time:

Example 9. Register a Custom `BasicType` implementation

```java
    configuration.registerTypeContributor( (typeContributions, serviceRegistry) -> {

        typeContributions.contributeType( BitSetType.INSTANCE );

    } );

```

or using the `MetadataBuilder`

```java

    ServiceRegistry standardRegistry =new StandardServiceRegistryBuilder().build();

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


