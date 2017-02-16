### 20.20. Caveats

    <div class="ulist">

*   Technically, multiple persistence units are supported by Enterprise OSGi JPA and unmanaged Hibernate JPA use.
    However, we cannot currently support this in OSGi.
    In Hibernate 4, only one instance of the OSGi-specific `ClassLoader` is used per Hibernate bundle, mainly due to heavy use of static TCCL utilities.
    We hope to support one OSGi `ClassLoader` per persistence unit in Hibernate 5.
*   Scanning is supported to find non-explicitly listed entities and mappings.
    However, they MUST be in the same bundle as your persistence unit (fairly typical anyway).
    Our OSGi `ClassLoader` only considers the "requesting bundle" (hence the requirement on using services to create `EntityManagerFactory`/`SessionFactory`), rather than attempting to scan all available bundles.
    This is primarily for versioning considerations, collision protections, etc.
*   Some containers (ex: Aries) always return true for `PersistenceUnitInfo#excludeUnlistedClasses`, even if your `persistence.xml` explicitly has `exclude-unlisted-classes` set to `false`.
    They claim it&#8217;s to protect JPA providers from having to implement scanning ("we handle it for you"), even though we still want to support it in many cases.
    The work around is to set `hibernate.archive.autodetection` to, for example, `hbm,class`.
    This tells hibernate to ignore the `excludeUnlistedClasses` value and scan for `*.hbm.xml` and entities regardless.
*   Scanning does not currently support annotated packages on `package-info.java`.
*   Currently, Hibernate OSGi is primarily tested using Apache Karaf and Apache Aries JPA. Additional testing is needed with Equinox, Gemini, and other container providers.
*   Hibernate ORM has many dependencies that do not currently provide OSGi manifests. The QuickStart tutorials make heavy use of 3rd party bundles (SpringSource, ServiceMix) or the `wrap:&#8230;&#8203;` operator.
    </div>
    </div>
    </div>
    </div>
    <div class="sect1">