#Custom BasicTypes

Hibernate makes it relatively easy for developers to create their own basic type mappings type.
For example, you might want to persist properties of type `java.util.BigInteger` to `VARCHAR` columns, or support completely new types.
Hibernate使开发人员能够相对容易地创建自己的基本类型映射类型。 例如，您可能希望将类型java.util.BigInteger的属性保留到VARCHAR列，或者支持完全新的类型。
There are two approaches to developing a custom type:
有两种方法来开发自定义类型：
>* implementing a `BasicType` and registering it
>* 实现一个BasicType并注册它
>* implement a `UserType` which doesn&#8217;t require type registration
>* 实现不需要类型注册的“UserType”
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