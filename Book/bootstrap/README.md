    ## 3. Bootstrap

    <div class="sectionbody">
    <div class="paragraph">

    The term bootstrapping refers to initializing and starting a software component.
    In Hibernate, we are specifically talking about the process of building a fully functional `SessionFactory` instance or `EntityManagerFactory` instance, for JPA.
    The process is very different for each.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    This chapter will not focus on all the possibilities of bootstrapping.
    Those will be covered in each specific more-relevant chapters later on.
    Instead, we focus here on the API calls needed to perform the bootstrapping.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="admonitionblock tip">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    During the bootstrap process, you might want to customize Hibernate behavior so make sure you check the [Configurations](#configurations) section as well.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="sect2">