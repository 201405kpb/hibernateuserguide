### 10.4. Pessimistic

    <div class="paragraph">

    Typically, you only need to specify an isolation level for the JDBC connections and let the database handle locking issues.
    If you do need to obtain exclusive pessimistic locks or re-obtain locks at the start of a new transaction, Hibernate gives you the tools you need.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Hibernate always uses the locking mechanism of the database, and never lock objects in memory.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">