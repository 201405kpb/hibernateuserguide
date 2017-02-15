# Hibernate的基础类型
**_Table 1. Standard BasicTypes_**
>| Hibernate type | JDBC type | Java type | BasicTypeRegistry key\(s\) |
| :--- | :--- | :--- | :--- |
|StringType|VARCHAR|java.lang.String|string, java.lang.String|
|MaterializedClob|CLOB|java.lang.String|materialized_clob|
|TextType|LONGVARCHAR|java.lang.String|text|
|CharacterType|CHAR|char, java.lang.Character|char, java.lang.Character|
|BooleanType|BIT|boolean, java.lang.Boolean|boolean, java.lang.Boolean|
|NumericBooleanType|INTEGER, 0 is false, 1 is true|boolean, java.lang.Boolean|numeric_boolean|
|YesNoType|CHAR, 'N'/'n' is false, 'Y'/'y' is true. The uppercase value is written to the database.|boolean, java.lang.Boolean|yes_no|
|TrueFalseType|CHAR, 'F'/'f' is false, 'T'/'t' is true. The uppercase value is written to the database.|boolean, java.lang.Boolean|true_false|
|ByteType|TINYINT|byte, java.lang.Byte|byte, java.lang.Byte|
|ShortType|SMALLINT|short, java.lang.Short|short, java.lang.Short|
|IntegerTypes|INTEGER|int, java.lang.Integer|int, java.lang.Integer|
|LongType|BIGINT|long, java.lang.Long|long, java.lang.Long|
|FloatType|FLOAT|float, java.lang.Float|float, java.lang.Float|
|DoubleType|DOUBLE|double, java.lang.Double|double, java.lang.Double|
|BigIntegerType|NUMERIC|java.math.BigInteger|big_integer, java.math.BigInteger|
|BigDecimalType|NUMERIC|java.math.BigDecimal|big_decimal, java.math.bigDecimal|
|TimestampType|TIMESTAMP|java.sql.Timestamp|timestamp, java.sql.Timestamp|
|TimeType|TIME|java.sql.Time|time, java.sql.Time|
|DateType|DATE|java.sql.Date|date, java.sql.Date|
|CalendarType|TIMESTAMP|java.util.Calendar|calendar, java.util.Calendar|
|CalendarDateType|DATE|java.util.Calendar|calendar_date|
|CalendarTimeType|TIME|java.util.Calendar|calendar_time|
|CurrencyType|java.util.Currency|VARCHAR|currency, java.util.Currency|
|LocaleType|VARCHAR|java.util.Locale|locale, java.utility.locale|
|TimeZoneType|VARCHAR, using the TimeZone ID|java.util.TimeZone|timezone, java.util.TimeZone|
|UrlType|VARCHAR|java.net.URL|url, java.net.URL|
|ClassType|VARCHAR (class FQN)|java.lang.Class|class, java.lang.Class|
|BlobType|BLOB|java.sql.Blob|blob, java.sql.Blob|
|ClobType|CLOB|java.sql.Clob|clob, java.sql.Clob|
|BinaryType|VARBINARY|byte[]|binary, byte[]|
|MaterializedBlobType|BLOB|byte[]|materized_blob|
|ImageType|LONGVARBINARY|byte[]|image|
|WrapperBinaryType|VARBINARY|java.lang.Byte[]|wrapper-binary, Byte[], java.lang.Byte[]|
|CharArrayType|VARCHAR|char[]|characters, char[]|
|CharacterArrayType|VARCHAR|java.lang.Character[]|wrapper-characters, Character[], java.lang.Character[]|
|UUIDBinaryType|BINARY|java.util.UUID|uuid-binary, java.util.UUID|
|UUIDCharType|CHAR, can also read VARCHAR|java.util.UUID|uuid-char|
|PostgresUUIDType|PostgreSQL UUID, through Types#OTHER, which complies to the PostgreSQL JDBC driver definition|java.util.UUID|pg-uuid|
|SerializableType|VARBINARY|implementors of java.lang.Serializable|Unlike the other value types, multiple instances of this type are registered. It is registered once under java.io.Serializable, and registered under the specific java.io.Serializable implementation class names.|
|StringNVarcharType|NVARCHAR|java.lang.String|nstring|
|NTextType|LONGNVARCHAR|java.lang.String|ntext|
|NClobType|NCLOB|java.sql.NClob|nclob, java.sql.NClob|
|MaterializedNClobType|NCLOB|java.lang.String|materialized_nclob|
|PrimitiveCharacterArrayNClobType|NCHAR|char[]|N/A|
|CharacterNCharType|NCHAR|java.lang.Character|ncharacter|
|CharacterArrayNClobType|NCLOB|java.lang.Character[]|N/A|

**_Table 2. Java 8 BasicTypes_**

>| Hibernate type | JDBC type | Java type | BasicTypeRegistry key\(s\) |
| :--- | :--- | :--- | :--- |
|DurationType|BIGINT|java.time.Duration|Duration, java.time.Duration|
|InstantType|TIMESTAMP|java.time.Instant|Instant, java.time.Instant|
|LocalDateTimeType|TIMESTAMP|java.time.LocalDateTime|LocalDateTime, java.time.LocalDateTime|
|LocalDateType|DATE|java.time.LocalDate|LocalDate, java.time.LocalDate|
|LocalTimeType|TIME|java.time.LocalTime|LocalTime, java.time.LocalTime|
|OffsetDateTimeType|TIMESTAMP||java.time.OffsetDateTime|OffsetDateTime, java.time.OffsetDateTime|
|OffsetTimeType|TIME|java.time.OffsetTime|OffsetTime, java.time.OffsetTime|
|OffsetTimeType|TIMESTAMP|java.time.ZonedDateTime|ZonedDateTime, java.time.ZonedDateTime|

**_Table 3. Hibernate Spatial BasicTypes_**

| Hibernate type | JDBC type | Java type | BasicTypeRegistry key\(s\) |
| :--- | :--- | :--- | :--- |
|JTSGeometryType|depends on the dialect|com.vividsolutions.jts.geom.Geometry|jts_geometry, or the classname of Geometry or any of its subclasses|
|GeolatteGeometryType|depends on the dialect|org.geolatte.geom.Geometry|geolatte_geometry, or the classname of Geometry or any of its subclasses|




