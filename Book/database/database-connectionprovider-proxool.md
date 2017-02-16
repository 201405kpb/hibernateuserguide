 ### 7.4. Using Proxool

    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    To use this integration, the application must include the hibernate-proxool module jar (as well as its dependencies) on the classpath.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Hibernate also provides support for applications to use [Proxool](http://proxool.sourceforge.net/) connection pooling.

    </div>
    <div class="paragraph">

    Transaction isolation of the Connections is managed by the `ConnectionProvider` itself. See [ConnectionProvider support for transaction isolation setting](#database-connectionprovider-isolation).

    </div>
    </div>
    <div class="sect2">