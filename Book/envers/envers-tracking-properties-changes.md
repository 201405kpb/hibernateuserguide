 ### 21.7. Tracking entity changes at property level

    <div class="paragraph">

    By default, the only information stored by Envers are revisions of modified entities.
    This approach lets user create audit queries based on historical values of entity properties.
    Sometimes it is useful to store additional metadata for each revision, when you are interested also in the type of changes, not only about the resulting values.

    </div>
    <div class="paragraph">

    The feature described in [Tracking entity names modified during revisions](#envers-tracking-modified-entities-revchanges) makes it possible to tell which entities were modified in a given revision.

    </div>
    <div class="paragraph">

    The feature described here takes it one step further.
    "Modification Flags" enable Envers to track which properties of audited entities were modified in a given revision.

    </div>
    <div class="paragraph">

    Tracking entity changes at property level can be enabled by:

    </div>
    <div class="olist arabic">

1.  setting `org.hibernate.envers.global_with_modified_flag` configuration property to `true`.
    This global switch will cause adding modification flags to be stored for all audited properties of all audited entities.
2.  using `@Audited( withModifiedFlag=true )` on a property or on an entity.
    </div>
    <div class="paragraph">

    The trade-off coming with this functionality is an increased size of audit tables and a very little, almost negligible, performance drop during audit writes.
    This is due to the fact that every tracked property has to have an accompanying boolean column in the schema that stores information about the property modifications.
    Of course it is Envers job to fill these columns accordingly - no additional work by the developer is required.
    Because of costs mentioned, it is recommended to enable the feature selectively, when needed with use of the granular configuration means described above.

    </div>
    <div class="paragraph">

    To see how "Modified Flags" can be utilized, check out the very simple query API that uses them: [Querying for revisions of entity that modified given property](#envers-tracking-properties-changes-queries).

    </div>
    </div>
    <div class="sect2">