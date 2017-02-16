### 23.20. Bootstrap properties

    <table class="tableblock frame-all grid-all spread">
    <colgroup>
    <col style="width: 20%;">
    <col style="width: 20%;">
    <col style="width: 60%;">
    </colgroup>
    <tbody>
    <tr>
    <td class="tableblock halign-left valign-top">

    Property
    </td>
    <td class="tableblock halign-left valign-top">

    Example
    </td>
    <td class="tableblock halign-left valign-top">

    Purpose
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.integrator_provider`
    </td>
    <td class="tableblock halign-left valign-top">

    The fully qualified name of an [`IntegratorProvider`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/jpa/boot/spi/IntegratorProvider.html)
    </td>
    <td class="tableblock halign-left valign-top">

    Used to define a list of  [`Integrator`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/integrator/spi/Integrator.html) which are used during bootstrap process to integrate various services.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.strategy_registration_provider`
    </td>
    <td class="tableblock halign-left valign-top">

    The fully qualified name of an [`StrategyRegistrationProviderList`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/jpa/boot/spi/StrategyRegistrationProviderList.html)
    </td>
    <td class="tableblock halign-left valign-top">

    Used to define a list of  [`StrategyRegistrationProvider`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/registry/selector/StrategyRegistrationProvider.html) which are used during bootstrap process to provide registrations of strategy selector(s).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.type_contributors`
    </td>
    <td class="tableblock halign-left valign-top">

    The fully qualified name of an [`TypeContributorList`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/jpa/boot/spi/TypeContributorList.html)
    </td>
    <td class="tableblock halign-left valign-top">

    Used to define a list of  [`TypeContributor`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/model/TypeContributor.html) which are used during bootstrap process to contribute types.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.persister.resolver`
    </td>
    <td class="tableblock halign-left valign-top">

    The fully qualified name of a [`PersisterClassResolver`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/persister/spi/PersisterClassResolver.html) or a `PersisterClassResolver` instance
    </td>
    <td class="tableblock halign-left valign-top">

    Used to define an implementation of the `PersisterClassResolver` interface which can be used to customize how an entity or a collection is being persisted.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.persister.factory`
    </td>
    <td class="tableblock halign-left valign-top">

    The fully qualified name of a [`PersisterFactory`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/persister/spi/PersisterFactory.html) or a `PersisterFactory` instance
    </td>
    <td class="tableblock halign-left valign-top">

    Like a `PersisterClassResolver`, the `PersisterFactory` can be used to customize how an entity or a collection are being persisted.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.service.allow_crawling`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` (default value) or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Crawl all available service bindings for an alternate registration of a given Hibernate `Service`.
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">