### 25.1. Schema management

    <div class="paragraph">

    Although Hibernate provides the `update` option for the `hibernate.hbm2ddl.auto` configuration property,
    this feature is not suitable for a production environment.

    </div>
    <div class="paragraph">

    An automated schema migration tool (e.g. [Flyway](https://flywaydb.org/), [Liquibase](http://www.liquibase.org/)) allows you to use any database-specific DDL feature (e.g. Rules, Triggers, Partitioned Tables).
    Every migration should have an associated script, which is stored on the Version Control System, along with the application source code.

    </div>
    <div class="paragraph">

    When the application is deployed on a production-like QA environment, and the deploy worked as expected, then pushing the deploy to a production environment should be straightforward since the latest schema migration was already tested.

    </div>
    <div class="admonitionblock tip">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    You should always use an automatic schema migration tool and have all the migration scripts stored in the Version Control System.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">