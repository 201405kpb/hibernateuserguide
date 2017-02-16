### 5.3. Making entities persistent

    <div class="paragraph">

    Once you&#8217;ve created a new entity instance (using the standard `new` operator) it is in `new` state.
    You can make it persistent by associating it to either a `org.hibernate.Session` or `javax.persistence.EntityManager`.

    </div>
    <div id="pc-persist-jpa-example" class="exampleblock">
    <div class="title">Example 228. Making an entity persistent with JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = new Person();
    person.setId( 1L );
    person.setName("John Doe");

    entityManager.persist( person );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="pc-persist-native-example" class="exampleblock">
    <div class="title">Example 229. Making an entity persistent with Hibernate API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = new Person();
    person.setId( 1L );
    person.setName("John Doe");

    session.save( person );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    `org.hibernate.Session` also has a method named persist which follows the exact semantic defined in the JPA specification for the persist method.
    It is this `org.hibernate.Session` method to which the Hibernate `javax.persistence.EntityManager` implementation delegates.

    </div>
    <div class="paragraph">

    If the `DomesticCat` entity type has a generated identifier, the value is associated with the instance when the save or persist is called.
    If the identifier is not automatically generated, the manually assigned (usually natural) key value has to be set on the instance before the save or persist methods are called.

    </div>
    </div>
    <div class="sect2">