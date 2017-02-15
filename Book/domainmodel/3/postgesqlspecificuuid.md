   
    #### 2.3.13. PostgeSQL-specific UUID

    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    When using one of the PostgreSQL Dialects, this becomes the default UUID mapping

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Maps the UUID using PostgreSQL&#8217;s specific UUID data type.
    The PostgreSQL JDBC driver chooses to map its UUID type to the `OTHER` code.
    Note that this can cause difficulty as the driver chooses to map many different data types to `OTHER`.

    </div>
    </div>
    <div class="sect3">