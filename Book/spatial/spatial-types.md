 ### 18.3. Types

    <div class="paragraph">

    Hibernate Spatial comes with the following types:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">jts_geometry</dt>
    <dd>

    Handled by `org.hibernate.spatial.JTSGeometryType` it maps a database geometry column type to a `com.vividsolutions.jts.geom.Geometry` entity property type.

    </dd>
    <dt class="hdlist1">geolatte_geometry</dt>
    <dd>

    Handled by `org.hibernate.spatial.GeolatteGeometryType`, it maps a database geometry column type to an `org.geolatte.geom.Geometry` entity property type.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    It suffices to declare a property as either a JTS or an Geolatte-geom `Geometry` and Hibernate Spatial will map it using the
    relevant type.

    </div>
    <div class="paragraph">

    Here is an example using JTS:

    </div>
    <div id="spatial-types-mapping-example" class="exampleblock">
    <div class="title">Example 483. Type mapping</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`import com.vividsolutions.jts.geom.Point;

    @Entity(name = "Event")
    public static class Event {

        @Id
        private Long id;

        private String name;

        private Point location;

        //Getters and setters are omitted for brevity
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    We can now treat spatial geometries like any other type.

    </div>
    <div id="spatial-types-point-creation-example" class="exampleblock">
    <div class="title">Example 484. Creating a Point</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Event event = new Event();
    event.setId( 1L);
    event.setName( "Hibernate ORM presentation");
    Point point = geometryFactory.createPoint( new Coordinate( 10, 5 ) );
    event.setLocation( point );

    entityManager.persist( event );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Spatial Dialects defines many query functions that are available both in HQL and JPQL queries. Below we show how we
    could use the `within` function to find all objects within a given spatial extent or window.

    </div>
    <div id="spatial-types-query-example" class="exampleblock">
    <div class="title">Example 485. Querying the geometry</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Polygon window = geometryFactory.createPolygon( coordinates );
    Event event = entityManager.createQuery(
        "select e " +
        "from Event e " +
        "where within(e.location, :window) = true", Event.class)
    .setParameter("window", window)
    .getSingleResult();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect1">