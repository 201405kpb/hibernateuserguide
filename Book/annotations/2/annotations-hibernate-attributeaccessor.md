#### 24.2.5. `@AttributeAccessor`

<div class="paragraph">

The [`@AttributeAccessor`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/AttributeAccessor.html) annotation is used to specify a custom [`PropertyAccessStrategy`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/property/access/spi/PropertyAccessStrategy.html).

</div>
<div class="paragraph">

Should only be used to name a custom `PropertyAccessStrategy`.
For property/field access type, the JPA [`@Access`](#annotations-jpa-access) annotation should be preferred.

</div>
<div class="paragraph">

However, if this annotation is used with either value="property" or value="field", it will act just as the corresponding usage of the JPA [`@Access`](#annotations-jpa-access) annotation.

</div>
</div>
<div class="sect3">

