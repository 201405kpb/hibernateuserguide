  #### 2.5.4. Declare getters and setters for persistent attributes

    <div class="paragraph">

    The JPA specification requires this, otherwise the model would prevent accessing the entity persistent state fields directly from outside the entity itself.

    </div>
    <div class="paragraph">

    Although Hibernate does not require it, it is recommended to follow the JavaBean conventions and define getters and setters for entity persistent attributes.
    Nevertheless, you can still tell Hibernate to directly access the entity fields.

    </div>
    <div class="paragraph">

    Attributes (whether fields or getters/setters) need not be declared public.
    Hibernate can deal with attributes declared with public, protected, package or private visibility.
    Again, if wanting to use runtime proxy generation for lazy loading, the getter/setter should grant access to at least package visibility.

    </div>
    </div>
    <div class="sect3">