#### 24.2.8. `@Cascade`

<div class="paragraph">

The [`@Cascade`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/Cascade.html) annotation is used to apply the Hibernate specific [`CascadeType`](http://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/CascadeType.html) strategies (e.g. `CascadeType.LOCK`, `CascadeType.SAVE_UPDATE`, `CascadeType.REPLICATE`) on a given association.

</div>
<div class="paragraph">

For JPA cascading, prefer using the [`javax.persistence.CascadeType`](http://docs.oracle.com/javaee/7/api/javax/persistence/CascadeType.html) instead.

</div>
<div class="paragraph">

When combining both JPA and Hibernate `CascadeType` strategies, Hibernate will merge both sets of cascades.

</div>
</div>
<div class="sect3">

