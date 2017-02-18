# Hibernate的基础类型

_**Table 1. Standard BasicTypes**_

> | Hibernate type | JDBC type | Java type | BasicTypeRegistry key\(s\) |
> | :--- | :--- | :--- | :--- |
> | StringType | VARCHAR | java.lang.String | string, java.lang.String |
> | MaterializedClob | CLOB | java.lang.String | materialized\_clob |
> | TextType | LONGVARCHAR | java.lang.String | text |
> | CharacterType | CHAR | char, java.lang.Character | char, java.lang.Character |
> | BooleanType | BIT | boolean, java.lang.Boolean | boolean, java.lang.Boolean |
> | NumericBooleanType | INTEGER, 0 is false, 1 is true | boolean, java.lang.Boolean | numeric\_boolean |
> | YesNoType | CHAR, 'N'/'n' is false, 'Y'/'y' is true. The uppercase value is written to the database. | boolean, java.lang.Boolean | yes\_no |
> | TrueFalseType | CHAR, 'F'/'f' is false, 'T'/'t' is true. The uppercase value is written to the database. | boolean, java.lang.Boolean | true\_false |
> | ByteType | TINYINT | byte, java.lang.Byte | byte, java.lang.Byte |
> | ShortType | SMALLINT | short, java.lang.Short | short, java.lang.Short |
> | IntegerTypes | INTEGER | int, java.lang.Integer | int, java.lang.Integer |
> | LongType | BIGINT | long, java.lang.Long | long, java.lang.Long |
> | FloatType | FLOAT | float, java.lang.Float | float, java.lang.Float |
> | DoubleType | DOUBLE | double, java.lang.Double | double, java.lang.Double |
> | BigIntegerType | NUMERIC | java.math.BigInteger | big\_integer, java.math.BigInteger |
> | BigDecimalType | NUMERIC | java.math.BigDecimal | big\_decimal, java.math.bigDecimal |
> | TimestampType | TIMESTAMP | java.sql.Timestamp | timestamp, java.sql.Timestamp |
> | TimeType | TIME | java.sql.Time | time, java.sql.Time |
> | DateType | DATE | java.sql.Date | date, java.sql.Date |
> | CalendarType | TIMESTAMP | java.util.Calendar | calendar, java.util.Calendar |
> | CalendarDateType | DATE | java.util.Calendar | calendar\_date |
> | CalendarTimeType | TIME | java.util.Calendar | calendar\_time |
> | CurrencyType | java.util.Currency | VARCHAR | currency, java.util.Currency |
> | LocaleType | VARCHAR | java.util.Locale | locale, java.utility.locale |
> | TimeZoneType | VARCHAR, using the TimeZone ID | java.util.TimeZone | timezone, java.util.TimeZone |
> | UrlType | VARCHAR | java.net.URL | url, java.net.URL |
> | ClassType | VARCHAR \(class FQN\) | java.lang.Class | class, java.lang.Class |
> | BlobType | BLOB | java.sql.Blob | blob, java.sql.Blob |
> | ClobType | CLOB | java.sql.Clob | clob, java.sql.Clob |
> | BinaryType | VARBINARY | byte\[\] | binary, byte\[\] |
> | MaterializedBlobType | BLOB | byte\[\] | materized\_blob |
> | ImageType | LONGVARBINARY | byte\[\] | image |
> | WrapperBinaryType | VARBINARY | java.lang.Byte\[\] | wrapper-binary, Byte\[\], java.lang.Byte\[\] |
> | CharArrayType | VARCHAR | char\[\] | characters, char\[\] |
> | CharacterArrayType | VARCHAR | java.lang.Character\[\] | wrapper-characters, Character\[\], java.lang.Character\[\] |
> | UUIDBinaryType | BINARY | java.util.UUID | uuid-binary, java.util.UUID |
> | UUIDCharType | CHAR, can also read VARCHAR | java.util.UUID | uuid-char |
> | PostgresUUIDType | PostgreSQL UUID, through Types\#OTHER, which complies to the PostgreSQL JDBC driver definition | java.util.UUID | pg-uuid |
> | SerializableType | VARBINARY | implementors of java.lang.Serializable | Unlike the other value types, multiple instances of this type are registered. It is registered once under java.io.Serializable, and registered under the specific java.io.Serializable implementation class names. |
> | StringNVarcharType | NVARCHAR | java.lang.String | nstring |
> | NTextType | LONGNVARCHAR | java.lang.String | ntext |
> | NClobType | NCLOB | java.sql.NClob | nclob, java.sql.NClob |
> | MaterializedNClobType | NCLOB | java.lang.String | materialized\_nclob |
> | PrimitiveCharacterArrayNClobType | NCHAR | char\[\] | N/A |
> | CharacterNCharType | NCHAR | java.lang.Character | ncharacter |
> | CharacterArrayNClobType | NCLOB | java.lang.Character\[\] | N/A |

_**Table 2. Java 8 BasicTypes**_

> |  | Hibernate type | JDBC type | Java type | BasicTypeRegistry key\(s\) |
> | :--- | :--- | :--- | :--- | :--- |
> |  | DurationType | BIGINT | java.time.Duration | Duration, java.time.Duration |
> |  | InstantType | TIMESTAMP | java.time.Instant | Instant, java.time.Instant |
> |  | LocalDateTimeType | TIMESTAMP | java.time.LocalDateTime | LocalDateTime, java.time.LocalDateTime |
> |  | LocalDateType | DATE | java.time.LocalDate | LocalDate, java.time.LocalDate |
> |  | LocalTimeType | TIME | java.time.LocalTime | LocalTime, java.time.LocalTime |
> | OffsetDateTimeType | TIMESTAMP |  | java.time.OffsetDateTime | OffsetDateTime, java.time.OffsetDateTime |
> |  | OffsetTimeType | TIME | java.time.OffsetTime | OffsetTime, java.time.OffsetTime |
> |  | OffsetTimeType | TIMESTAMP | java.time.ZonedDateTime | ZonedDateTime, java.time.ZonedDateTime |

_**Table 3. Hibernate Spatial BasicTypes**_

>| Hibernate type | JDBC type | Java type | BasicTypeRegistry key\(s\) |
| :--- | :--- | :--- | :--- |
| JTSGeometryType | depends on the dialect | com.vividsolutions.jts.geom.Geometry | jts\_geometry, or the classname of Geometry or any of its subclasses |
| GeolatteGeometryType | depends on the dialect | org.geolatte.geom.Geometry | geolatte\_geometry, or the classname of Geometry or any of its subclasses |

> ![](/Book/images/org/hibernate/docbook/note.png)  
> To use these hibernate-spatial types just must add the hibernate-spatial dependency to your classpath and use a the org.hibernate.spatial.SpatialDialect. See Spatial for more about spatial types.
>要使用这些hibernate-spatial类型，只需要在类路径中添加hibernate-spatial依赖，并使用org.hibernate.spatial.SpatialDialect。 有关空间类型的更多信息，请参阅空间。

These mappings are managed by a service inside Hibernate called the org.hibernate.type.BasicTypeRegistry, which essentially maintains a map of org.hibernate.type.BasicType (a org.hibernate.type.Type specialization) instances keyed by a name. That is the purpose of the "BasicTypeRegistry key(s)" column in the previous tables.


