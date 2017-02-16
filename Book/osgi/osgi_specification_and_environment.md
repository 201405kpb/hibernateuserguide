 ### 20.1. OSGi Specification and Environment

    <div class="paragraph">

    Hibernate targets the OSGi 4.3 spec or later.
    It was necessary to start with 4.3, over 4.2, due to our dependency on OSGi&#8217;s `BundleWiring` for entity/mapping scanning.

    </div>
    <div class="paragraph">

    Hibernate supports three types of configurations within OSGi.

    </div>
    <div class="olist arabic">

1.  Container-Managed JPA [Container-Managed JPA](#osgi-managed-jpa)
2.  Unmanaged JPA [Unmanaged JPA](#osgi-unmanaged-jpa)
3.  Unmanaged Native [Unmanaged Native](#osgi-unmanaged-native)
    </div>
    </div>
    <div class="sect2">