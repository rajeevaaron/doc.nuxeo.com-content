---
title: Upgrade from 5.3.1 to 5.3.2
review:
    comment: ''
    date: ''
    status: ok
confluence:
    ajs-parent-page-id: '16810015'
    ajs-parent-page-title: How to Upgrade Nuxeo
    ajs-space-key: ADMINDOC58
    ajs-space-name: Nuxeo Installation and Administration - 5.8
    canonical: Upgrade+from+5.3.1+to+5.3.2
    canonical_source: 'https://doc.nuxeo.com/display/ADMINDOC58/Upgrade+from+5.3.1+to+5.3.2'
    page_id: '16810050'
    shortlink: QoAAAQ
    shortlink_source: 'https://doc.nuxeo.com/x/QoAAAQ'
    source_link: /display/ADMINDOC58/Upgrade+from+5.3.1+to+5.3.2
history:
    - 
        author: Solen Guitter
        date: '2013-07-02 11:14'
        message: ''
        version: '6'
    - 
        author: Solen Guitter
        date: '2012-05-22 17:38'
        message: Migrated to Confluence 4.0
        version: '5'
    - 
        author: Solen Guitter
        date: '2012-05-22 17:38'
        message: ''
        version: '4'
    - 
        author: Julien Carsique
        date: '2011-06-15 13:59'
        message: ''
        version: '3'
    - 
        author: Solen Guitter
        date: '2011-04-12 09:59'
        message: ''
        version: '2'
    - 
        author: Thierry Delprat
        date: '2010-10-14 10:51'
        message: ''
        version: '1'

---
{{! multiexcerpt name='5.3.1-to-5.3.2-upgrade-page'}}

## **Code migration**

5.3.2 is fully backward compatible with 5.3.1 (no compat package is needed).

So, you should have no issues with running your custom code against 5.3.2\. If you have any problems, you can contact Nuxeo Support.

## **Packaging**

The packaging system is basically the same as the one used in 5.3.1.

The only change that may have an impact involves resources that are now managed by the new template system.

This means that resources are no longer embedded inside the EAR but handled in a separated templates directory.

This makes changing configurations easier (like switching from H2 to PostgreSQL) and will also allow for upgrades without having to redo all custom system configurations.

The documentation has been updated accordingly:

*   [Installation Guide]({{page space='nxdoc53' page='installation-and-administration-guide'}})
*   [Description of the new configuration system]({{page space='nxdoc53' page='configuring-nuxeo-ep'}})

## **Data**

The only changes done between 5.3.1 and 5.3.2 are the way tags are stored.

Because the Tag Service is now directly part of VCS, some small changes have been done.

Nevertheless, migration should be automatic and transparent.

If you have any problems, you can contact Nuxeo Support.

## **Configuration**

We have changed the way Nuxeo starts OpenOffice.

This is an intermediate solution before we upgrade to JODConverter 3.

The new OOolauncher (that replaces OOodeamon) should:

*   be more stable,
*   be easier to set up (removed the dependencies on JNI UNO libs).

{{! /multiexcerpt}}

&nbsp;

&nbsp;