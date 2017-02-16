 ### 21.23. Determining a suitable partitioning column

    <div class="paragraph">

    To partition this data, the 'level of relevancy' must be defined. Consider the following:

    </div>
    <div class="olist arabic">

1.  For fiscal year 2006 there is only one revision.
    It has the oldest _revision timestamp_ of all audit rows, but should still be regarded as relevant because it&#8217;s the latest modification for this fiscal year in the salary table (its _end revision timestamp_ is null).
    <div class="literalblock">
    <div class="content">
    <pre>  Also, note that it would be very unfortunate if in 2011 there would be an update of the salary for fiscal year 2006 (which is possible in until at least 10 years after the fiscal year),
      and the audit information would have been moved to a slow disk (based on the age of the __revision timestamp__).
      Remember that, in this case, Envers will have to update the _end revision timestamp_ of the most recent audit row.
    . There are two revisions in the salary of fiscal year 2007 which both have nearly the same _revision timestamp_ and a different __end revision timestamp__.
      On first sight, it is evident that the first revision was a mistake and probably not relevant.
      The only relevant revision for 2007 is the one with _end revision timestamp_ null.</pre>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Based on the above, it is evident that only the _end revision timestamp_ is suitable for audit table partitioning.
    The _revision timestamp_ is not suitable.

    </div>
    </div>
    <div class="sect2">
