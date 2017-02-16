
    ### 6.2. `COMMIT` flush

    <div class="paragraph">

    JPA also defines a COMMIT flush mode, which is described as follows:

    </div>
    <div class="quoteblock">
    > <div class="paragraph">
> 
>     If `FlushModeType.COMMIT` is set, the effect of updates made to entities in the persistence context upon queries is unspecified.
> 
>     </div>
    <div class="attribution">
    &#8212; Section 3.10.8 of the JPA 2.1 Specification
    </div>
    </div>
    <div class="paragraph">

    When executing a JPQL query, the persistence context is only flushed when the current running transaction is committed.

    </div>
    <div id="flushing-commit-flush-jpql-example" class="exampleblock">
    <div class="title">Example 268. `COMMIT` flushing on JPQL</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = new Person("John Doe");
    entityManager.persist(person);

    entityManager.createQuery("select p from Advertisement p")
        .setFlushMode( FlushModeType.COMMIT)
        .getResultList();

    entityManager.createQuery("select p from Person p")
        .setFlushMode( FlushModeType.COMMIT)
        .getResultList();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT a.id AS id1_0_ ,
           a.title AS title2_0_
    FROM   Advertisement a

    SELECT p.id AS id1_1_ ,
           p.name AS name2_1_
    FROM   Person p

    INSERT INTO Person (name, id) VALUES ('John Doe', 1)`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Because the JPA doesn&#8217;t impose a strict rule on delaying flushing, when executing a native SQL query, the persistence context is going to be flushed.

    </div>
    <div id="flushing-commit-flush-sql-example" class="exampleblock">
    <div class="title">Example 269. `COMMIT` flushing on SQL</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = new Person("John Doe");
    entityManager.persist(person);

    assertTrue(((Number) entityManager
        .createNativeQuery("select count(*) from Person")
        .getSingleResult()).intValue() == 1);`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO Person (name, id) VALUES ('John Doe', 1)

    SELECT COUNT(*) FROM Person`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">