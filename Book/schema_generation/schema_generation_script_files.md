### 4.1. Importing script files

    <div class="paragraph">

    To customize the schema generation process, the `hibernate.hbm2ddl.import_files` configuration property must be used to provide other scripts files that Hibernate can use when the `SessionFactory` is started.

    </div>
    <div class="paragraph">

    For instance, considering the following `schema-generation.sql` import file:

    </div>
    <div id="schema-generation-import-file-example" class="exampleblock">
    <div class="title">Example 214. Schema generation import file</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`create sequence book_sequence start with 1 increment by 1`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    If we configure Hibernate to import the script above:

    </div>
    <div id="schema-generation-import-file-configuration-example" class="exampleblock">
    <div class="title">Example 215. Enabling query cache</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;property
        name="hibernate.hbm2ddl.import_files"
        value="schema-generation.sql" /&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Hibernate is going to execute the script file after the schema is automatically generated.

    </div>
    </div>
    <div class="sect2">