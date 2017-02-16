### 11.6. Dynamic fetching via Hibernate profiles

    <div class="paragraph">

    Suppose we wanted to leverage loading by natural-id to obtain the `Employee` information in the "projects for and employee" use-case.
    Loading by natural-id uses the statically defined fetching strategies, but does not expose a means to define load-specific fetching.
    So we would leverage a fetch profile.

    </div>
    <div id="fetching-strategies-dynamic-fetching-profile-example" class="exampleblock">
    <div class="title">Example 289. Fetch profile example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Employee")
    @FetchProfile(
        name = "employee.projects",
        fetchOverrides = {
            @FetchProfile.FetchOverride(
                entity = Employee.class,
                association = "projects",
                mode = FetchMode.JOIN
            )
        }
    )`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`session.enableFetchProfile( "employee.projects" );
    Employee employee = session.bySimpleNaturalId( Employee.class ).load( username );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Here the `Employee` is obtained by natural-id lookup and the Employee&#8217;s `Project` data is fetched eagerly.
    If the `Employee` data is resolved from cache, the `Project` data is resolved on its own.
    However, if the `Employee` data is not resolved in cache, the `Employee` and `Project` data is resolved in one SQL query via join as we saw above.

    </div>
    </div>
    <div class="sect2">
