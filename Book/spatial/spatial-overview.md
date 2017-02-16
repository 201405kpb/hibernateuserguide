### 18.1. Overview

    <div class="paragraph">

    Hibernate Spatial was originally developed as a generic extension to Hibernate for handling geographic data.
    Since 5.0, Hibernate Spatial is now part of the Hibernate ORM project,
    and it allows you to deal with geographic data in a standardized way.

    </div>
    <div class="paragraph">

    Hibernate Spatial provides a standardized, cross-database interface to geographic data storage and query functions.
    It supports most of the functions described by the OGC Simple Feature Specification. Supported databases are: Oracle 10g/11g,
    PostgreSql/PostGIS, MySQL, Microsoft SQL Server and H2/GeoDB.

    </div>
    <div class="paragraph">

    Spatial data types are not part of the Java standard library, and they are absent from the JDBC specification.
    Over the years [JTS](http://tsusiatsoftware.net/jts/main.html) has emerged the _de facto_ standard to fill this gap. JTS is
    an implementation of the [Simple Feature Specification (SFS)](https://portal.opengeospatial.org/files/?artifact_id=829). Many databases
    on the other hand implement the SQL/MM - Part 3: Spatial Data specification - a related, but broader specification. The biggest difference is that
    SFS is limited to 2D geometries in the projected plane (although JTS supports 3D coordinates), whereas
    SQL/MM supports 2-, 3- or 4-dimensional coordinate spaces.

    </div>
    <div class="paragraph">

    Hibernate Spatial supports two different geometry models: [JTS](http://tsusiatsoftware.net/jts/main.html) and
    [geolatte-geom](https://github.com/GeoLatte/geolatte-geom). As already mentioned, JTS is the _de facto_
    standard. Geolatte-geom (also written by the lead developer of Hibernate Spatial) is a more recent library that
    supports many features specified in SQL/MM but not available in JTS (such as support for 4D geometries, and support for extended WKT/WKB formats).
    Geolatte-geom also implements encoders/decoders for the database native types. Geolatte-geom has good interoperability with
    JTS. Converting a Geolatte `geometry` to a JTS `geometry, for instance, doesn&#8217;t require copying of the coordinates.
    It also delegates spatial processing to JTS.

    </div>
    <div class="paragraph">

    Whether you use JTS or Geolatte-geom, Hibernate spatial maps the database spatial types to your geometry model of choice. It will, however,
    always use Geolatte-geom to decode the database native types.

    </div>
    <div class="paragraph">

    Hibernate Spatial also makes a number of spatial functions available in HQL and in the Criteria Query API. These functions are
    specified in both SQL/MM as SFS, and are commonly implemented in databases with spatial support (see [Hibernate Spatial dialect function support](#spatial-configuration-dialect-features))

    </div>
    </div>
    <div class="sect2">