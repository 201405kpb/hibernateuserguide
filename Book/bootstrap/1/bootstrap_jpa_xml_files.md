 #### 3.1.2. Externalizing XML mapping files

    <div class="paragraph">

    JPA offers two mapping options:

    </div>
    <div class="ulist">

*   annotations
*   XML mappings
    </div>
    <div class="paragraph">

    Although annotations are much more common, there are projects were XML mappings are preferred.
    You can even mix annotations and XML mappings so that you can override annotation mappings with XML configurations that can be easily changed without recompiling the project source code.
    This is possible because if there are two conflicting mappings, the XML mappings takes precedence over its annotation counterpart.

    </div>
    <div class="paragraph">

    The JPA specifications requires the XML mappings to be located on the class path:

    </div>
    <div class="quoteblock">
    > <div class="paragraph">
> 
>     An object/relational mapping XML file named `orm.xml` may be specified in the `META-INF` directory in the root of the persistence unit or in the `META-INF` directory of any jar file referenced by the `persistence.xml`.
> 
>     </div>
>     <div class="paragraph">
> 
>     Alternatively, or in addition, one or more mapping files may be referenced by the mapping-file elements of the persistence-unit element. These mapping files may be present anywhere on the class path.
> 
>     </div>
    <div class="attribution">
    &#8212; Section 8.2.1.6.2 of the JPA 2.1 Specification
    </div>
    </div>
    <div class="paragraph">

    Therefore, the mapping files can reside in the application jar artifacts, or they can be stored in an external folder location with the cogitation that that location be included in the class path.

    </div>
    <div class="paragraph">

    Hibernate is more lenient in this regard so you can use any external location even outside of the application configured class path.

    </div>
    <div id="bootstrap-jpa-compliant-persistence-xml-external-mappings-example" class="exampleblock">
    <div class="title">Example 203. META-INF/persistence.xml configuration file for external XML mappings</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence
                 http://xmlns.jcp.org/xml/ns/persistence/persistence_2_1.xsd"
                 version="2.1"&gt;

        &lt;persistence-unit name="CRM"&gt;
            &lt;description&gt;
                Persistence unit for Hibernate User Guide
            &lt;/description&gt;

            &lt;provider&gt;org.hibernate.jpa.HibernatePersistenceProvider&lt;/provider&gt;

            &lt;mapping-file&gt;file:///etc/opt/app/mappings/orm.xml&lt;/mapping-file&gt;

            &lt;properties&gt;
                &lt;property name="javax.persistence.jdbc.driver"
                          value="org.h2.Driver" /&gt;

                &lt;property name="javax.persistence.jdbc.url"
                          value="jdbc:h2:mem:db1;DB_CLOSE_DELAY=-1;MVCC=TRUE" /&gt;

                &lt;property name="javax.persistence.jdbc.user"
                          value="sa" /&gt;

                &lt;property name="javax.persistence.jdbc.password"
                          value="" /&gt;

                &lt;property name="hibernate.show_sql"
                          value="true" /&gt;

                &lt;property name="hibernate.hbm2ddl.auto"
                          value="update" /&gt;
            &lt;/properties&gt;

        &lt;/persistence-unit&gt;

    &lt;/persistence&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    In the `persistence.xml` configuration file above, the `orm.xml` XML file containing all JPA entity mappings is located in the `/etc/opt/app/mappings/` folder.

    </div>
    </div>
    </div>
    <div class="sect2">
