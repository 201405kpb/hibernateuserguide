#### 24.2.44. `@LazyCollection`

<div class="paragraph">

The [`@LazyCollection`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/LazyCollection.html) annotation is used to specify the lazy fetching behavior of a given collection.
The possible values are given by the `[LazyCollectionOption](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/LazyCollectionOption.html)` enumeration:

</div>
<div class="dlist">
<dl>
<dt class="hdlist1">`TRUE`</dt>
<dd>

Load it when the state is requested.

</dd>
<dt class="hdlist1">`FALSE`</dt>
<dd>

Eagerly load it.

</dd>
<dt class="hdlist1">`EXTRA`</dt>
<dd>

Prefer extra queries over full collection loading.

</dd>
</dl>
</div>
<div class="paragraph">

The `TRUE` and `FALSE` values are deprecated since you should be using the JPA [`FetchType`](http://docs.oracle.com/javaee/7/api/javax/persistence/FetchType.html) attribute of the [`@ElementCollection`](#annotations-jpa-elementcollection), [`@OneToMany`](#annotations-jpa-onetomany), or [`@ManyToMany`](#annotations-jpa-manytomany) collection.

</div>
<div class="paragraph">

The `EXTRA` value has no equivalent in the JPA specification, and it&#8217;s used to avoid loading the entire collection even when the collection is accessed for the first time.
Each element is fetched individually using a secondary query.

</div>
</div>
<div class="sect3">

