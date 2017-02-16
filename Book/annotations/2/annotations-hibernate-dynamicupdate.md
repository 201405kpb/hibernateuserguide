#### 24.2.20. `@DynamicUpdate`

<div class="paragraph">

The [`@DynamicUpdate`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/DynamicUpdate.html) annotation is used to specify that the `UPDATE` SQL statement should be generated whenever an entity is modified.

</div>
<div class="paragraph">

By default, Hibernate uses a cached `UPDATE` statement that sets all table columns.
When the entity is annotated with the `@DynamicUpdate` annotation, the `PreparedStatement` is going to include only the columns whose values have been changed.

</div>
<div class="paragraph">

See the [`OptimisticLockType.DIRTY` mapping](#locking-optimistic-lock-type-dirty-example) section for more info on how `@DynamicUpdate` works.

</div>
<div class="admonitionblock note">
<table>
<tr>
<td class="icon">

</td>
<td class="content">
<div class="paragraph">

For reattachment of detached entities, the dynamic update is not possible without having the [`@SelectBeforeUpdate`](#annotations-hibernate-selectbeforeupdate) annotation as well.

</div>
</td>
</tr>
</table>
</div>
</div>
<div class="sect3">

