    #### 24.1.28. `@ForeignKey`

    <div class="paragraph">

    The [`@ForeignKey`](http://docs.oracle.com/javaee/7/api/javax/persistence/ForeignKey.html) annotation is used to specify the associated foreign key of a [`@JoinColumn`](#annotations-jpa-joincolumn) mapping.
    The `@ForeignKey` annotation is only used if the automated schema generation tool is enabled, in which case, it allows you to customize the underlying foreign key definition.

    </div>
    <div class="paragraph">

    See the [`@ManyToOne` with `@ForeignKey`](#associations-many-to-one-example) section for more info.

    </div>
    </div>
    <div class="sect3">