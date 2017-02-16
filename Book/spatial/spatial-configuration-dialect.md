#### 18.2.2. Dialects

    <div class="paragraph">

    Hibernate Spatial extends the Hibernate ORM dialects so that the spatial functions of the database are made available within HQL and JPQL.
    So, for instance, instead of using the `PostgreSQL82Dialect`, we use the Hibernate Spatial extension of that dialect which is the `PostgisDialect`.

    </div>
    <div id="spatial-configuration-dialect-example" class="exampleblock">
    <div class="title">Example 482. Specifying a spatial dialect</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;property
        name="hibernate.dialect"
        value="org.hibernate.spatial.dialect.postgis.PostgisDialect"
    /&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Not all databases support all the functions defined by Hibernate Spatial.
    The table below provides an overview of the functions provided by each database. If the function is defined in the
    [Simple Feature Specification](https://portal.opengeospatial.org/files/?artifact_id=829), the description references the
    relevant section.

    </div>
    <table id="spatial-configuration-dialect-features" class="tableblock frame-all grid-all spread">
    <caption class="title">Table 9. Hibernate Spatial dialect function support</caption>
    <colgroup>
    <col style="width: %;">
    <col style="width: %;">
    <col style="width: %;">
    <col style="width: %;">
    <col style="width: %;">
    <col style="width: %;">
    <col style="width: %;">
    </colgroup>
    <tbody>
    <tr>
    <td class="tableblock halign-left valign-top">

    Function
    </td>
    <td class="tableblock halign-left valign-top">

    Description
    </td>
    <td class="tableblock halign-left valign-top">

    PostgresSQL
    </td>
    <td class="tableblock halign-left valign-top">

    Oracle 10g/11g
    </td>
    <td class="tableblock halign-left valign-top">

    MySQL
    </td>
    <td class="tableblock halign-left valign-top">

    SQLServer
    </td>
    <td class="tableblock halign-left valign-top">

    GeoDB (H2)
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Basic functions on Geometry
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `int dimension(Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.1
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `String geometrytype(Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.1
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `int srid(Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.1
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Geometry envelope(Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.1
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `String astext(Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.1
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `byte[] asbinary(Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.1
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `boolean isempty(Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.1
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `boolean issimple(Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.1
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Geometry boundary(Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.1
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon red"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Functions for testing Spatial Relations between geometric objects
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `boolean equals(Geometry, Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.2
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `boolean disjoint(Geometry, Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.2
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `boolean intersects(Geometry, Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.2
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `boolean touches(Geometry, Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.2
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `boolean crosses(Geometry, Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.2
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `boolean within(Geometry, Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.2
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `boolean contains(Geometry, Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.2
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `boolean overlaps(Geometry, Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.2
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `boolean relate(Geometry, Geometry, String)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.2
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon red"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Functions that support Spatial Analysis
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `double distance(Geometry, Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.3
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon red"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Geometry buffer(Geometry, double)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.3
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon red"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Geometry convexhull(Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.3
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon red"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Geometry intersection(Geometry, Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.3
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon red"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Geometry geomunion(Geometry, Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.3 (renamed from union)
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon red"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Geometry difference(Geometry, Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.3
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon red"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Geometry symdifference(Geometry, Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    SFS ¡ì2.1.1.3
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon red"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Common non-SFS functions
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `boolean dwithin(Geometry, Geometry, double)`
    </td>
    <td class="tableblock halign-left valign-top">

    Returns true if the geometries are within the specified distance of one another
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon red"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon red"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Geometry transform(Geometry, int)`
    </td>
    <td class="tableblock halign-left valign-top">

    Returns a new geometry with its coordinates transformed to the SRID referenced by the integer parameter
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon red"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon red"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon red"></span>
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    Spatial aggregate Functions
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `Geometry extent(Geometry)`
    </td>
    <td class="tableblock halign-left valign-top">

    Returns a bounding box that bounds the set of returned geometries
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon green"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon red"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon red"></span>
    </td>
    <td class="tableblock halign-left valign-top">

    <span class="icon red"></span>
    </td>
    </tr>
    </tbody>
    </table>
    <div id="spatial-configuration-dialect-postgis" class="dlist">
    <dl>
    <dt class="hdlist1">Postgis</dt>
    <dd>

    For Postgis from versions 1.3 and later, the best dialect to use is `org.hibernate.spatial.dialect.postgis.PostgisDialect`.

    <div class="paragraph">

    This translates the HQL spatial functions to the Postgis SQL/MM-compliant functions.
    For older, pre v1.3 versions of Postgis, which are not SQL/MM compliant, the dialect `org.hibernate.spatial.dialect.postgis.PostgisNoSQLMM` is provided.

    </div>
    </dd>
    </dl>
    </div>
    <div id="spatial-configuration-dialect-mysql" class="dlist">
    <dl>
    <dt class="hdlist1">MySQL</dt>
    <dd>

    There are several dialects for MySQL:

    <div class="dlist">
    <dl>
    <dt class="hdlist1">`MySQLSpatialDialect`</dt>
    <dd>

    a spatially-extended version of Hibernate `MySQLDialect`

    </dd>
    <dt class="hdlist1">`MySQLSpatialInnoDBDialect`</dt>
    <dd>

    a spatially-extended version of Hibernate `MySQLInnoDBDialect`

    </dd>
    <dt class="hdlist1">`MySQLSpatial56Dialect`</dt>
    <dd>

    a spatially-extended version of Hibernate `MySQL5DBDialect`.

    </dd>
    <dt class="hdlist1">`MySQLSpatial5InnoDBDialect`</dt>
    <dd>

    the same as `MySQLSpatial56Dialect`, but with support for the InnoDB storage engine.

    </dd>
    </dl>
    </div>
    </dd>
    </dl>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    MySQL versions before 5.6.1 had only limited support for spatial operators.
    Most operators only took account of the minimum bounding rectangles (MBR) of the geometries, and not the geometries themselves.

    </div>
    <div class="paragraph">

    This changed in version 5.6.1 were MySQL introduced `ST_*` spatial operators.
    The dialects `MySQLSpatial56Dialect` and `MySQLSpatial5InnoDBDialect` use these newer, more precise operators.

    </div>
    <div class="paragraph">

    These dialects may therefore produce results that differ from that of the other spatial dialects.

    </div>
    <div class="paragraph">

    For more information, see this page in the MySQL reference guide (esp. the section [Functions That Test Spatial Relations Between Geometry Objects](https://dev.mysql.com/doc/refman/5.7/en/spatial-relation-functions.html))

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div id="spatial-configuration-dialect-oracle" class="dlist">
    <dl>
    <dt class="hdlist1">Oracle10g/11g</dt>
    <dd>

    There is currently only one Oracle spatial dialect: `OracleSpatial10gDialect` which extends the Hibernate dialect `Oracle10gDialect`.
    This dialect has been tested on both Oracle 10g and Oracle 11g with the `SDO_GEOMETRY` spatial database type.

    <div class="paragraph">

    This dialect is the only dialect that can be configured using these Hibernate properties:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`hibernate.spatial.connection_finder`</dt>
    <dd>

    the fully-qualified classname for the Connection finder for this Dialect (see below).

    </dd>
    </dl>
    </div>
    </dd>
    </dl>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="title">The `ConnectionFinder` interface</div>
    <div class="paragraph">

    The `SDOGeometryType` requires access to an `OracleConnection` object wehen converting a geometry to SDO_GEOMETRY.
    In some environments, however, the `OracleConnection` is not available (e.g. because a Java EE container or connection pool proxy wraps the connection object in its own `Connection` implementation).
    A `ConnectionFinder` knows how to retrieve the `OracleConnection` from the wrapper or proxy Connection object that is passed into prepared statements.

    </div>
    <div class="paragraph">

    The default implementation will, when the passed object is not already an `OracleConnection`, attempt to retrieve the `OracleConnection` by recursive reflection.
    It will search for methods that return `Connection` objects, execute these methods and check the result.
    If the result is of type `OracleConnection` the object is returned, otherwise it recurses on it.

    </div>
    <div class="paragraph">

    In may cases this strategy will suffice.
    If not, you can provide your own implementation of this interface on the class path, and configure it in the `hibernate.spatial.connection_finder` property.
    Note that implementations must be thread-safe and have a default no-args constructor.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">SQL Server</dt>
    <dd>

    The dialect `SqlServer2008Dialect` supports the `GEOMETRY` type in SQL Server 2008 and later.

    </dd>
    </dl>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The `GEOGRAPHY` type is not currently supported.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">GeoDB (H2)</dt>
    <dd>

    The `GeoDBDialect` supports the GeoDB a spatial extension of the H2 in-memory database.

    </dd>
    </dl>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The dialect has been tested with GeoDB version 0.7

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    </div>
    <div class="sect2">