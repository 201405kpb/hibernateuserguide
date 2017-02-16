 ### 15.16. Distinct

    <div class="paragraph">

    For JPQL and HQL, `DISTINCT` has two meanings:

    </div>
    <div class="olist arabic">

1.  It can be passed to the database so that duplicates are removed from a result set
2.  It can be used to filter out the same parent entity references when join fetching a child collection
    </div>
    <div class="sect3">