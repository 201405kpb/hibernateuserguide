 ### 21.24. Determining a suitable partitioning scheme

    <div class="paragraph">

    A possible partitioning scheme for the salary table would be as follows:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">_end revision timestamp_ year = 2008</dt>
    <dd>

    This partition contains audit data that is not very (or no longer) relevant.

    </dd>
    <dt class="hdlist1">_end revision timestamp_ year = 2009</dt>
    <dd>

    This partition contains audit data that is potentially relevant.

    </dd>
    <dt class="hdlist1">_end revision timestamp_ year &gt;= 2010 or null</dt>
    <dd>

    This partition contains the most relevant audit data.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    This partitioning scheme also covers the potential problem of the update of the _end revision timestamp_,
    which occurs if a row in the audited table is modified.
    Even though Envers will update the _end revision timestamp_ of the audit row to the system date at the instant of modification,
    the audit row will remain in the same partition (the 'extension bucket').

    </div>
    <div class="paragraph">

    And sometime in 2011, the last partition (or 'extension bucket') is split into two new partitions:

    </div>
    <div class="olist arabic">

1.  _end revision timestamp_ year = 2010:: This partition contains audit data that is potentially relevant (in 2011).
2.  _end revision timestamp_ year &gt;= 2011 or null:: This partition contains the most interesting audit data and is the new 'extension bucket'.
    </div>
    </div>
    <div class="sect2">
