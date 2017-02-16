 ### 5.6. Obtain an entity with its data initialized

    <div class="paragraph">

    It is also quite common to want to obtain an entity along with its data (e.g. like when we need to display it in the UI).

    </div>
    <div id="pc-find-jpa-example" class="exampleblock">
    <div class="title">Example 234. Obtaining an entity reference with its data initialized with JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = entityManager.find( Person.class, personId );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="pc-find-native-example" class="exampleblock">
    <div class="title">Example 235. Obtaining an entity reference with its data initialized with Hibernate API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = session.get( Person.class, personId );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="pc-find-by-id-native-example" class="exampleblock">
    <div class="title">Example 236. Obtaining an entity reference with its data initialized using the `byId()` Hibernate API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = session.byId( Person.class ).load( personId );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    In both cases null is returned if no matching database row was found.

    </div>
    <div class="paragraph">

    It&#8217;s possible to return a Java 8 `Optional` as well:

    </div>
    <div id="tag::pc-find-optional-by-id-native-example" class="exampleblock">
    <div class="title">Example 237. Obtaining an Optional entity reference with its data initialized using the `byId()` Hibernate API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Optional&lt;Person&gt; optionalPerson = session.byId( Person.class ).loadOptional( personId );`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">