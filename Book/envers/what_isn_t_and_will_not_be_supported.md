 #### 21.17.1. What isn&#8217;t and will not be supported

    <div class="paragraph">

    Bags are not supported because they can contain non-unique elements.
    Persisting, a bag of `String`s violates the relational database principle that each table is a set of tuples.

    </div>
    <div class="paragraph">

    In case of bags, however (which require a join table), if there is a duplicate element, the two tuples corresponding to the elements will be the same.
    Hibernate allows this, however Envers (or more precisely: the database connector) will throw an exception when trying to persist two identical elements because of a unique constraint violation.

    </div>
    <div class="paragraph">

    There are at least two ways out if you need bag semantics:

    </div>
    <div class="olist arabic">

1.  use an indexed collection, with the `@javax.persistence.OrderColumn` annotation
2.  provide a unique id for your elements with the `@CollectionId` annotation.
    </div>
    </div>
    <div class="sect3">