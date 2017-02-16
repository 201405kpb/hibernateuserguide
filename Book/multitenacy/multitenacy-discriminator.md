### 19.3. Partitioned (discriminator) data

    <div class="paragraph">

    <span class="image">![multitenacy_discriminator](images/multitenancy/multitenacy_discriminator.png)</span>

    </div>
    <div class="paragraph">

    All data is kept in a single database schema.
    The data for each tenant is partitioned by the use of partition value or discriminator.
    The complexity of this discriminator might range from a simple column value to a complex SQL formula.
    Again, this approach would use a single Connection pool to service all tenants.
    However, in this approach, the application needs to alter each and every SQL statement sent to the database to reference the "tenant identifier" discriminator.

    </div>
    </div>
    <div class="sect2">