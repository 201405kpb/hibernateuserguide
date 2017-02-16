 ### 5.8. Modifying managed/persistent state

    <div class="paragraph">

    Entities in managed/persistent state may be manipulated by the application and any changes will be automatically detected and persisted when the persistence context is flushed.
    There is no need to call a particular method to make your modifications persistent.

    </div>
    <div id="pc-managed-state-jpa-example" class="exampleblock">
    <div class="title">Example 242. Modifying managed state with JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = entityManager.find( Person.class, personId );
    person.setName("John Doe");
    entityManager.flush();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="pc-managed-state-native-example" class="exampleblock">
    <div class="title">Example 243. Modifying managed state with Hibernate API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = session.byId( Person.class ).load( personId );
    person.setName("John Doe");
    entityManager.flush();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">