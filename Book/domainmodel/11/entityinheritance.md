 ### 2.11. Inheritance

    <div class="paragraph">

    Although relational database systems don&#8217;t provide support for inheritance, Hibernate provides several strategies to leverage this object-oriented trait onto domain model entities:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">MappedSuperclass</dt>
    <dd>

    Inheritance is implemented in domain model only without reflecting it in the database schema. See [MappedSuperclass](#entity-inheritance-mapped-superclass).

    </dd>
    <dt class="hdlist1">Single table</dt>
    <dd>

    The domain model class hierarchy is materialized into a single table which contains entities belonging to different class types. See [Single table](#entity-inheritance-single-table).

    </dd>
    <dt class="hdlist1">Joined table</dt>
    <dd>

    The base class and all the subclasses have their own database tables and fetching a subclass entity requires a join with the parent table as well. See [Joined table](#entity-inheritance-joined-table).

    </dd>
    <dt class="hdlist1">Table per class</dt>
    <dd>

    Each subclass has its own table containing both the subclass and the base class properties. See [Table per class](#entity-inheritance-table-per-class).

    </dd>
    </dl>
    </div>
    <div class="sect3">