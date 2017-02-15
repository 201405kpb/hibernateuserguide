#BaseTypeRegistry

We said before that a Hibernate type is not a Java type, nor a SQL type, but that it understands both and performs the marshalling between them.
But looking at the basic type mappings from the previous examples,
how did Hibernate know to use its `org.hibernate.type.StringType` for mapping for `java.lang.String` attributes,
or its `org.hibernate.type.IntegerType` for mapping `java.lang.Integer` attributes?

 The answer lies in a service inside Hibernate called the `org.hibernate.type.BasicTypeRegistry`, which essentially maintains a map of `org.hibernate.type.BasicType` (a `org.hibernate.type.Type` specialization) instances keyed by a name.

We will see later, in the [Explicit BasicTypes](#basic-type-annotation) section, that we can explicitly tell Hibernate which BasicType to use for a particular attribute.
But first let&#8217;s explore how implicit resolution works and how applications can adjust implicit resolution.


A thorough discussion of the `BasicTypeRegistry` and all the different ways to contribute types to it is beyond the scope of this documentation.
Please see Integrations Guide for complete details.


As an example, take a String attribute such as we saw before with Product#sku.
Since there was no explicit type mapping, Hibernate looks to the `BasicTypeRegistry` to find the registered mapping for `java.lang.String`.
This goes back to the "BasicTypeRegistry key(s)" column we saw in the tables at the start of this chapter.



As a baseline within `BasicTypeRegistry`, Hibernate follows the recommended mappings of JDBC for Java types.
JDBC recommends mapping Strings to VARCHAR, which is the exact mapping that `StringType` handles.
So that is the baseline mapping within `BasicTypeRegistry` for Strings.

Applications can also extend (add new `BasicType` registrations) or override (replace an existing `BasicType` registration) using one of the
`MetadataBuilder#applyBasicType` methods or the `MetadataBuilder#applyTypes` method during bootstrap.
For more details, see [Custom BasicTypes](#basic-custom-type) section.