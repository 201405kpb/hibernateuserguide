  ### 16.2. Selecting an entity

    <div class="paragraph">

    This is probably the most common form of query.
    The application wants to select entity instances.

    </div>
    <div id="criteria-typedquery-entity-example" class="exampleblock">
    <div class="title">Example 409. Selecting the root entity</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CriteriaBuilder builder = entityManager.getCriteriaBuilder();

    CriteriaQuery&lt;Person&gt; criteria = builder.createQuery( Person.class );
    Root&lt;Person&gt; root = criteria.from( Person.class );
    criteria.select( root );
    criteria.where( builder.equal( root.get( Person_.name ), "John Doe" ) );

    List&lt;Person&gt; persons = entityManager.createQuery( criteria ).getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The example uses `createQuery()` passing in the `Person` class reference as the results of the query will be `Person` objects.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The call to the `CriteriaQuery#select` method in this example is unnecessary because _root_ will be the implied selection since we have only a single query root.
    It was done here only for completeness of an example.

    </div>
    <div class="paragraph">

    The `Person_.name` reference is an example of the static form of JPA Metamodel reference.
    We will use that form exclusively in this chapter.
    See the documentation for the [Hibernate JPA Metamodel Generator](https://docs.jboss.org/hibernate/orm/5.2/topical/html/metamodelgen/MetamodelGenerator.html) for additional details on the JPA static Metamodel.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">