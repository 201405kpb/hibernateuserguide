## 8. Transactions and concurrency control

    <div class="sectionbody">
    <div class="paragraph">

    It is important to understand that the term transaction has many different yet related meanings in regards to persistence and Object/Relational Mapping.
    In most use-cases these definitions align, but that is not always the case.

    </div>
    <div class="ulist">

*   Might refer to the physical transaction with the database.
*   Might refer to the logical notion of a transaction as related to a persistence context.
*   Might refer to the application notion of a Unit-of-Work, as defined by the archetypal pattern.
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    This documentation largely treats the physical and logic notions of a transaction as one-in-the-same.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="sect2">