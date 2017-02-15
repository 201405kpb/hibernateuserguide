#### 2.6.10. Using identifier table

    <div class="paragraph">

    Hibernate achieves table-based identifier generation based on its `org.hibernate.id.enhanced.TableGenerator` id generator which defines a table capable of holding multiple named value segments for any number of entities.

    </div>
    <div class="exampleblock">
    <div class="title">Example 119. Table generator table structure</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`create table hibernate_sequences(
        sequence_name VARCHAR NOT NULL,
        next_val INTEGER NOT NULL
    )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The basic idea is that a given table-generator table (`hibernate_sequences` for example) can hold multiple segments of identifier generation values.

    </div>
    <div class="exampleblock">
    <div class="title">Example 120. Unnamed table generator</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    public class MyEntity {

        @Id
        @GeneratedValue( generation = TABLE )
        public Integer id;

        ...
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    If no table name is given Hibernate assumes an implicit name of `hibernate_sequences`.
    Additionally, because no `javax.persistence.TableGenerator#pkColumnValue` is specified, Hibernate will use the default segment (`sequence_name='default'`) from the hibernate_sequences table.

    </div>
    </div>
    <div class="sect3">
