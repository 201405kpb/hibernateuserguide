#### 2.6.13. Optimizers

    <div class="paragraph">

    Most of the Hibernate generators that separately obtain identifier values from database structures support the use of pluggable optimizers.
    Optimizers help manage the number of times Hibernate has to talk to the database in order to generate identifier values.
    For example, with no optimizer applied to a sequence-generator, every time the application asked Hibernate to generate an identifier it would need to grab the next sequence value from the database.
    But if we can minimize the number of times we need to communicate with the database here, the application will be able to perform better.
    Which is, in fact, the role of these optimizers.

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">none</dt>
    <dd>

    No optimization is performed. We communicate with the database each and every time an identifier value is needed from the generator.

    </dd>
    <dt class="hdlist1">pooled-lo</dt>
    <dd>

    The pooled-lo optimizer works on the principle that the increment-value is encoded into the database table/sequence structure.
    In sequence-terms this means that the sequence is defined with a greater-that-1 increment size.

    <div class="paragraph">

    For example, consider a brand new sequence defined as `create sequence m_sequence start with 1 increment by 20`.
    This sequence essentially defines a "pool" of 20 usable id values each and every time we ask it for its next-value.
    The pooled-lo optimizer interprets the next-value as the low end of that pool.

    </div>
    <div class="paragraph">

    So when we first ask it for next-value, we&#8217;d get 1.
    We then assume that the valid pool would be the values from 1-20 inclusive.

    </div>
    <div class="paragraph">

    The next call to the sequence would result in 21, which would define 21-40 as the valid range. And so on.
    The "lo" part of the name indicates that the value from the database table/sequence is interpreted as the pool lo(w) end.

    </div>
    </dd>
    <dt class="hdlist1">pooled</dt>
    <dd>

    Just like pooled-lo, except that here the value from the table/sequence is interpreted as the high end of the value pool.

    </dd>
    <dt class="hdlist1">hilo; legacy-hilo</dt>
    <dd>

    Define a custom algorithm for generating pools of values based on a single value from a table or sequence.

    <div class="paragraph">

    These optimizers are not recommended for use. They are maintained (and mentioned) here simply for use by legacy applications that used these strategies previously.

    </div>
    </dd>
    </dl>
    </div>
    <div class="paragraph">

    Applications can also implement and use their own optimizer strategies, as defined by the `org.hibernate.id.enhanced.Optimizer` contract.

    </div>
    </div>
    <div class="sect3">
