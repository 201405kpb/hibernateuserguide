#### 24.2.82. `@Synchronize`

<div class="paragraph">

The [`@Synchronize`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/Synchronize.html) annotation is usually used in conjunction with the [`@Subselect`](#annotations-hibernate-subselect) annotation to specify the list of database tables used by the `@Subselect` SQL query.

</div>
<div class="paragraph">

With this information in place, Hibernate will properly trigger an entity flush whenever a query targeting the `@Subselect` entity is to be executed while the Persistence Context has scheduled some insert/update/delete actions against the database tables used by the `@Subselect` SQL query.

</div>
<div class="paragraph">

Therefore, the `@Synchronize` annotation prevents the derived entity from returning stale data when executing entity queries against the `@Subselect` entity.

</div>
</div>
<div class="sect3"