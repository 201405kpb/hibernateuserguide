#### 24.2.78. `@SqlFragmentAlias`

<div class="paragraph">

The [`@SqlFragmentAlias`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/SqlFragmentAlias.html) annotation is used to specify an alias for a Hibernate [`@Filter`](#annotations-hibernate-filter).

</div>
<div class="paragraph">

The alias (e.g. `myAlias`) can then be used in the `@Filter` `condition` clause using the `{alias}` (e.g. `{myAlias}`) placeholder.

</div>
</div>
<div class="sect3">

