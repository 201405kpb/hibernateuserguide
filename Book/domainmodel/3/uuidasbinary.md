
    #### 2.3.11. UUID as binary

    <div class="paragraph">

    As mentioned, the default mapping for UUID attributes.
    Maps the UUID to a `byte[]` using `java.util.UUID#getMostSignificantBits` and `java.util.UUID#getLeastSignificantBits` and stores that as `BINARY` data.

    </div>
    <div class="paragraph">

    Chosen as the default simply because it is generally more efficient from storage perspective.

    </div>
    </div>
    <div class="sect3">