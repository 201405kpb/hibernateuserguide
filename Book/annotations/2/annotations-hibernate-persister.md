#### 24.2.67. `@Persister`

<div class="paragraph">

The [`@Persister`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/Persister.html) annotation is used to specify a custom entity or collection persister.

</div>
<div class="paragraph">

For entities, the custom persister must implement the [`EntityPersister`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/persister/entity/EntityPersister.html) interface.

</div>
<div class="paragraph">

For collections, the custom persister must implement the [`CollectionPersister`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/persister/collection/CollectionPersister.html) interface.

</div>
</div>
<div class="sect3">