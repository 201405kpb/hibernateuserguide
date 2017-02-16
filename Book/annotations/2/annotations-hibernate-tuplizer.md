#### 24.2.86. `@Tuplizer`

<div class="paragraph">

The [`@Tuplizer`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/Tuplizer.html) annotation is used to specify a custom tuplizer for the current annotated entity or embeddable.

</div>
<div class="paragraph">

For entities, the tupelizer must implement the [`EntityTuplizer`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tuple/entity/EntityTuplizer.html) interface.

</div>
<div class="paragraph">

For embeddables, the tupelizer must implement the [`ComponentTuplizer`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tuple/component/ComponentTuplizer.html) interface.

</div>
</div>
<div class="sect3">

