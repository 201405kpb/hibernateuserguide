    ## 11. Fetching

    <div class="sectionbody">
    <div class="paragraph">

    Fetching, essentially, is the process of grabbing data from the database and making it available to the application.
    Tuning how an application does fetching is one of the biggest factors in determining how an application will perform.
    Fetching too much data, in terms of width (values/columns) and/or depth (results/rows),
    adds unnecessary overhead in terms of both JDBC communication and ResultSet processing.
    Fetching too little data might cause additional fetching to be needed.
    Tuning how an application fetches data presents a great opportunity to influence the application overall performance.

    </div>
    <div class="sect2">
