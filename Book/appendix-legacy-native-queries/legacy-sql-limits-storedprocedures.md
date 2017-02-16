### 30.4. Legacy rules/limitations for using stored procedures

    <div class="paragraph">

    You cannot use stored procedures with Hibernate unless you follow some procedure/function rules.
    If they do not follow those rules they are not usable with Hibernate.
    If you still want to use these procedures you have to execute them via `session.doWork()`.

    </div>
    <div class="paragraph">

    The rules are different for each database since database vendors have different stored procedure semantics/syntax.

    </div>
    <div class="paragraph">

    Stored procedure queries cannot be paged with `setFirstResult()/setMaxResults()`.

    </div>
    <div class="paragraph">

    The recommended call form is standard SQL92: `{ ? = call functionName(&lt;parameters&gt;) }` or `{ ? = call procedureName(&lt;parameters&gt;}`.
    Native call syntax is not supported.

    </div>
    <div class="paragraph">

    For Oracle the following rules apply:

    </div>
    <div class="ulist">

*   A function must return a result set.
    The first parameter of a procedure must be an `OUT` that returns a result set.
    This is done by using a `SYS_REFCURSOR` type in Oracle 9 or 10.
    In Oracle you need to define a `REF CURSOR` type.
    See Oracle literature for further information.
    </div>
    <div class="paragraph">

    For Sybase or MS SQL server the following rules apply:

    </div>
    <div class="ulist">

*   The procedure must return a result set.
    Note that since these servers can return multiple result sets and update counts, Hibernate will iterate the results and take the first result that is a result set as its return value.
    Everything else will be discarded.
*   If you can enable `SET NOCOUNT ON` in your procedure it will probably be more efficient, but this is not a requirement.
    </div>
    </div>
    <div class="sect2">