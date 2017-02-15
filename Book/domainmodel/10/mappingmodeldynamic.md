#### 2.10.1. Dynamic mapping models

    <div class="paragraph">

    Persistent entities do not necessarily have to be represented as POJO/JavaBean classes.
    Hibernate also supports dynamic models (using `Map`s of `Map`s at runtime).
    With this approach, you do not write persistent classes, only mapping files.

    </div>
    <div class="paragraph">

    A given entity has just one entity mode within a given SessionFactory.
    This is a change from previous versions which allowed to define multiple entity modes for an entity and to select which to load.
    Entity modes can now be mixed within a domain model; a dynamic entity might reference a POJO entity, and vice versa.

    </div>
    <div class="exampleblock">
    <div class="title">Example 180. Working with Dynamic Domain Models</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Session s = openSession();
    Transaction tx = s.beginTransaction();

    // Create a customer entity
    Map&lt;String, String&gt;david = new HashMap&lt;&gt;();
    david.put( "name","David" );

    // Create an organization entity
    Map&lt;String, String&gt;foobar = new HashMap&lt;&gt;();
    foobar.put( "name","Foobar Inc." );

    // Link both
    david.put( "organization",foobar );

    // Save both
    s.save( "Customer",david );
    s.save( "Organization",foobar );

    tx.commit();
    s.close();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The main advantage of dynamic models is quick turnaround time for prototyping without the need for entity class implementation.
    The main down-fall is that you lose compile-time type checking and will likely deal with many exceptions at runtime.
    However, as a result of the Hibernate mapping, the database schema can easily be normalized and sound, allowing to add a proper domain model implementation on top later on.

    </div>
    <div class="paragraph">

    It is also interesting to note that dynamic models are great for certain integration use cases as well.
    Envers, for example, makes extensive use of dynamic models to represent the historical data.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    </div>
    <div class="sect2">