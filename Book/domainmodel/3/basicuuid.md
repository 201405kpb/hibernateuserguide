 #### 2.3.10. Mapping UUID Values

    <div class="paragraph">

    Hibernate also allows you to map UUID values, again in a number of ways.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The default UUID mapping is as binary because it represents more efficient storage.
    However many applications prefer the readability of character storage.
    To switch the default mapping, simply call `MetadataBuilder.applyBasicType( UUIDCharType.INSTANCE, UUID.class.getName() )`

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect3">