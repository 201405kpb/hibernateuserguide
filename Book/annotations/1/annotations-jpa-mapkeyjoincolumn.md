    #### 24.1.44. `@MapKeyJoinColumn`

    <div class="paragraph">

    The [`@MapKeyJoinColumn`](http://docs.oracle.com/javaee/7/api/javax/persistence/MapKeyJoinColumn.html) annotation is used to specify that the key of `java.util.Map` association is an entity association.
    The map key column is a FOREIGN KEY in a link table that also joins the `Map` owner&#8217;s table with the table where the `Map` value resides.

    </div>
    <div class="paragraph">

    See the [`@MapKeyJoinColumn` mapping](#collections-map-value-type-entity-key-example) section for more info.

    </div>
    </div>
    <div class="sect3">