#### 24.2.19. `@DynamicInsert`

<div class="paragraph">

The [`@DynamicInsert`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/DynamicInsert.html) annotation is used to specify that the `INSERT` SQL statement should be generated whenever an entity is to be persisted.

</div>
<div class="paragraph">

By default, Hibernate uses a cached `INSERT` statement that sets all table columns.
When the entity is annotated with the `@DynamicInsert` annotation, the `PreparedStatement` is going to include only the non-null columns.

</div>
<div class="paragraph">

See the [`@CreationTimestamp` mapping](#mapping-generated-CreationTimestamp) section for more info on how `@DynamicInsert` works.

</div>
</div>
<div class="sect3">

