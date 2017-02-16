 ### 10.1. Optimistic

    <div class="paragraph">

    When your application uses long transactions or conversations that span several database transactions,
    you can store versioning data so that if the same entity is updated by two conversations, the last to commit changes is informed of the conflict,
    and does not override the other conversation&#8217;s work.
    This approach guarantees some isolation, but scales well and works particularly well in _read-often-write-sometimes_ situations.

    </div>
    <div class="paragraph">

    Hibernate provides two different mechanisms for storing versioning information, a dedicated version number or a timestamp.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    A version or timestamp property can never be null for a detached instance.
    Hibernate detects any instance with a null version or timestamp as transient, regardless of other unsaved-value strategies that you specify.
    Declaring a nullable version or timestamp property is an easy way to avoid problems with transitive reattachment in Hibernate,
    especially useful if you use assigned identifiers or composite keys.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">