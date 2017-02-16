    ### 29.1. Creating a `Criteria` instance

    <div class="paragraph">

    The interface `org.hibernate.Criteria` represents a query against a particular persistent class.
    The `Session` is a factory for `Criteria` instances.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Criteria crit = sess.createCriteria(Cat.class);
    crit.setMaxResults(50);
    List cats = crit.list();`</pre>
    </div>
    </div>
    </div>
    <div class="sect2">