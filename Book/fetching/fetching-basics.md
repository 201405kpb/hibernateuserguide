### 11.1. The basics

    <div class="paragraph">

    The concept of fetching breaks down into two different questions.

    </div>
    <div class="ulist">

*   When should the data be fetched? Now? Later?
*   How should the data be fetched?
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    "now" is generally termed eager or immediate. "later" is generally termed lazy or delayed.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    There are a number of scopes for defining fetching:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">_static_</dt>
    <dd>

    Static definition of fetching strategies is done in the mappings.
    The statically-defined fetch strategies is used in the absence of any dynamically defined strategies

    <div class="dlist">
    <dl>
    <dt class="hdlist1">SELECT</dt>
    <dd>

    Performs a separate SQL select to load the data. This can either be EAGER (the second select is issued immediately) or LAZY (the second select is delayed until the data is needed).
    This is the strategy generally termed N+1.

    </dd>
    <dt class="hdlist1">JOIN</dt>
    <dd>

    Inherently an EAGER style of fetching. The data to be fetched is obtained through the use of an SQL outer join.

    </dd>
    <dt class="hdlist1">BATCH</dt>
    <dd>

    Performs a separate SQL select to load a number of related data items using an IN-restriction as part of the SQL WHERE-clause based on a batch size.
    Again, this can either be EAGER (the second select is issued immediately) or LAZY (the second select is delayed until the data is needed).

    </dd>
    <dt class="hdlist1">SUBSELECT</dt>
    <dd>

    Performs a separate SQL select to load associated data based on the SQL restriction used to load the owner.
    Again, this can either be EAGER (the second select is issued immediately) or LAZY (the second select is delayed until the data is needed).

    </dd>
    </dl>
    </div>
    </dd>
    <dt class="hdlist1">_dynamic_ (sometimes referred to as runtime)</dt>
    <dd>

    Dynamic definition is really use-case centric. There are multiple ways to define dynamic fetching:

    <div class="dlist">
    <dl>
    <dt class="hdlist1">_fetch profiles_</dt>
    <dd>

    defined in mappings, but can be enabled/disabled on the `Session`.

    </dd>
    <dt class="hdlist1">HQL/JPQL</dt>
    <dd>

    and both Hibernate and JPA Criteria queries have the ability to specify fetching, specific to said query.

    </dd>
    <dt class="hdlist1">entity graphs</dt>
    <dd>

    Starting in Hibernate 4.2 (JPA 2.1) this is also an option.

    </dd>
    </dl>
    </div>
    </dd>
    </dl>
    </div>
    </div>
    <div class="sect2">