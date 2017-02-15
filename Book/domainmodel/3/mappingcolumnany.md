 #### 2.3.23. @Any mapping

    <div class="paragraph">

    There is one more type of property mapping.
    The `@Any` mapping defines a polymorphic association to classes from multiple tables.
    This type of mapping requires more than one column.
    The first column contains the type of the associated entity.
    The remaining columns contain the identifier.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    It is impossible to specify a foreign key constraint for this kind of association.
    This is not the usual way of mapping polymorphic associations and you should use this only in special cases (e.g. audit logs, user session data, etc).

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The `@Any` annotation describes the column holding the metadata information.
    To link the value of the metadata information and an actual entity type, the `@AnyDef` and `@AnyDefs` annotations are used.
    The `metaType` attribute allows the application to specify a custom type that maps database column values to persistent classes that have identifier properties of the type specified by `idType`.
    You must specify the mapping from values of the `metaType` to class names.

    </div>
    <div class="paragraph">

    For the next examples, consider the following `Property` class hierarchy:

    </div>
    <div id="mapping-column-any-property-example" class="exampleblock">
    <div class="title">Example 72. `Property` class hierarchy</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`public interface Property&lt;T&gt; {

        String getName();

        T getValue();
    }

    @Entity
    @Table(name="integer_property")
    public class IntegerProperty implements Property&lt;Integer&gt; {

        @Id
        private Long id;

        @Column(name = "`name`")
        private String name;

        @Column(name = "`value`")
        private Integer value;

        public Long getId() {
            return id;
        }

        public void setId(Long id) {
            this.id = id;
        }

        @Override
        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public Integer getValue() {
            return value;
        }

        public void setValue(Integer value) {
            this.value = value;
        }
    }

    @Entity
    @Table(name="string_property")
    public class StringProperty implements Property&lt;String&gt; {

        @Id
        private Long id;

        @Column(name = "`name`")
        private String name;

        @Column(name = "`value`")
        private String value;

        public Long getId() {
            return id;
        }

        public void setId(Long id) {
            this.id = id;
        }

        @Override
        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public String getValue() {
            return value;
        }

        public void setValue(String value) {
            this.value = value;
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    A `PropertyHolder` can reference any such property, and, because each `Property` belongs to a separate table, the `@Any` annotation is, therefore, required.

    </div>
    <div id="mapping-column-any-example" class="exampleblock">
    <div class="title">Example 73. `@Any` mapping usage</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    @Table( name = "property_holder" )
    public class PropertyHolder {

        @Id
        private Long id;

        @Any(
            metaDef = "PropertyMetaDef",
            metaColumn = @Column( name = "property_type" )
        )
        @JoinColumn( name = "property_id" )
        private Property property;

    	//Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE property_holder (
        id BIGINT NOT NULL,
        property_type VARCHAR(255),
        property_id BIGINT,
        PRIMARY KEY ( id )
    )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    As you can see, there are two columns used to reference a `Property` instance: `property_id` and `property_type`.
    The `property_id` is used to match the `id` column of either the `string_property` or `integer_property` tables,
    while the `property_type` is used to match the `string_property` or the  `integer_property` table.

    </div>
    <div class="paragraph">

    The table resolving mapping is defined by the `metaDef` attribute which references an `@AnyMetaDef` mapping.
    Although the `@AnyMetaDef` mapping could be set right next to the `@Any` annotation,
    it&#8217;s good practice to reuse it, therefore it makes sense to configure it on a class or package-level basis.

    </div>
    <div class="paragraph">

    The `package-info.java` contains the `@AnyMetaDef` mapping:

    </div>
    <div id="mapping-column-any-meta-def-example" class="exampleblock">
    <div class="title">Example 74. `@Any` mapping usage</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@AnyMetaDef( name= "PropertyMetaDef", metaType = "string", idType = "long",
        metaValues = {
                @MetaValue(value = "S", targetEntity = StringProperty.class),
                @MetaValue(value = "I", targetEntity = IntegerProperty.class)
            }
        )
    package org.hibernate.userguide.mapping.basic.any;

    import org.hibernate.annotations.AnyMetaDef;
    import org.hibernate.annotations.MetaValue;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    It is recommended to place the `@AnyMetaDef` mapping as a package metadata.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    To see how the `@Any` annotation in action, consider the following example:

    </div>
    <div id="mapping-column-any-persistence-example" class="exampleblock">
    <div class="title">Example 75. `@Any` mapping usage</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`doInHibernate( this::sessionFactory, session -&gt; {
        IntegerProperty ageProperty = new IntegerProperty();
        ageProperty.setId( 1L );
        ageProperty.setName( "age" );
        ageProperty.setValue( 23 );

        StringProperty nameProperty = new StringProperty();
        nameProperty.setId( 1L );
        nameProperty.setName( "name" );
        nameProperty.setValue( "John Doe" );

        session.persist( ageProperty );
        session.persist( nameProperty );

        PropertyHolder namePropertyHolder = new PropertyHolder();
        namePropertyHolder.setId( 1L );
        namePropertyHolder.setProperty( nameProperty );
        session.persist( namePropertyHolder );
    } );

    doInHibernate( this::sessionFactory, session -&gt; {
        PropertyHolder propertyHolder = session.get( PropertyHolder.class, 1L );
        assertEquals("name", propertyHolder.getProperty().getName());
        assertEquals("John Doe", propertyHolder.getProperty().getValue());
    } );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO integer_property
           ( "name", "value", id )
    VALUES ( 'age', 23, 1 )

    INSERT INTO string_property
           ( "name", "value", id )
    VALUES ( 'name', 'John Doe', 1 )

    INSERT INTO property_holder
           ( property_type, property_id, id )
    VALUES ( 'S', 1, 1 )

    SELECT ph.id AS id1_1_0_,
           ph.property_type AS property2_1_0_,
           ph.property_id AS property3_1_0_
    FROM   property_holder ph
    WHERE  ph.id = 1

    SELECT sp.id AS id1_2_0_,
           sp."name" AS name2_2_0_,
           sp."value" AS value3_2_0_
    FROM   string_property sp
    WHERE  sp.id = 1`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="sect4">

    ##### `@ManyToAny` mapping

    <div class="paragraph">

    The `@Any` mapping is useful to emulate a `@ManyToOne` association when there can be multiple target entities.
    To emulate a `@OneToMany` association, the `@ManyToAny` annotation must be used.

    </div>
    <div class="paragraph">

    In the following example, the `PropertyRepository` entity has a collection of `Property` entities.
    The `repository_properties` link table holds the associations between `PropertyRepository` and `Property` entities.

    </div>
    <div id="mapping-column-many-to-any-example" class="exampleblock">
    <div class="title">Example 76. `@ManyToAny` mapping usage</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity
    @Table( name = "property_repository" )
    public class PropertyRepository {

        @Id
        private Long id;

        @ManyToAny(
            metaDef = "PropertyMetaDef",
            metaColumn = @Column( name = "property_type" )
        )
        @Cascade( { org.hibernate.annotations.CascadeType.ALL })
        @JoinTable(name = "repository_properties",
            joinColumns = @JoinColumn(name = "repository_id"),
            inverseJoinColumns = @JoinColumn(name = "property_id")
        )
        private List&lt;Property&lt;?&gt;&gt; properties = new ArrayList&lt;&gt;(  );

    	//Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CREATE TABLE property_repository (
        id BIGINT NOT NULL,
        PRIMARY KEY ( id )
    )

    CREATE TABLE repository_properties (
        repository_id BIGINT NOT NULL,
        property_type VARCHAR(255),
        property_id BIGINT NOT NULL
    )`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    To see how the `@ManyToAny` annotation works, consider the following example:

    </div>
    <div id="mapping-column-many-to-any-persistence-example" class="exampleblock">
    <div class="title">Example 77. `@Any` mapping usage</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`doInHibernate( this::sessionFactory, session -&gt; {
        IntegerProperty ageProperty = new IntegerProperty();
        ageProperty.setId( 1L );
        ageProperty.setName( "age" );
        ageProperty.setValue( 23 );

        StringProperty nameProperty = new StringProperty();
        nameProperty.setId( 1L );
        nameProperty.setName( "name" );
        nameProperty.setValue( "John Doe" );

        session.persist( ageProperty );
        session.persist( nameProperty );

        PropertyRepository propertyRepository = new PropertyRepository();
        propertyRepository.setId( 1L );
        propertyRepository.getProperties().add( ageProperty );
        propertyRepository.getProperties().add( nameProperty );
        session.persist( propertyRepository );
    } );

    doInHibernate( this::sessionFactory, session -&gt; {
        PropertyRepository propertyRepository = session.get( PropertyRepository.class, 1L );
        assertEquals(2, propertyRepository.getProperties().size());
        for(Property property : propertyRepository.getProperties()) {
            assertNotNull( property.getValue() );
        }
    } );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`INSERT INTO integer_property
           ( "name", "value", id )
    VALUES ( 'age', 23, 1 )

    INSERT INTO string_property
           ( "name", "value", id )
    VALUES ( 'name', 'John Doe', 1 )

    INSERT INTO property_repository ( id )
    VALUES ( 1 )

    INSERT INTO repository_properties
        ( repository_id , property_type , property_id )
    VALUES
        ( 1 , 'I' , 1 )

    INSERT INTO repository_properties
        ( repository_id , property_type , property_id )
    VALUES
        ( 1 , 'S' , 1 )

    SELECT pr.id AS id1_1_0_
    FROM   property_repository pr
    WHERE  pr.id = 1

    SELECT ip.id AS id1_0_0_ ,
           integerpro0_."name" AS name2_0_0_ ,
           integerpro0_."value" AS value3_0_0_
    FROM   integer_property integerpro0_
    WHERE  integerpro0_.id = 1

    SELECT sp.id AS id1_3_0_ ,
           sp."name" AS name2_3_0_ ,
           sp."value" AS value3_3_0_
    FROM   string_property sp
    WHERE  sp.id = 1`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">