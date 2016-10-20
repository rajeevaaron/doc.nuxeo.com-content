---
title: Nuxeo Groups and Rights Audit
review:
    comment: ''
    date: ''
    status: ok
labels:
    - nuxeo-groups-rights-audit
    - export
confluence:
    ajs-parent-page-id: '21299463'
    ajs-parent-page-title: Marketplace Add-Ons
    ajs-space-key: USERDOC60
    ajs-space-name: Nuxeo Platform User Documentation — 6.0
    canonical: Nuxeo+Groups+and+Rights+Audit
    canonical_source: 'https://doc.nuxeo.com/display/USERDOC60/Nuxeo+Groups+and+Rights+Audit'
    page_id: '21299529'
    shortlink: SQFFAQ
    shortlink_source: 'https://doc.nuxeo.com/x/SQFFAQ'
    source_link: /display/USERDOC60/Nuxeo+Groups+and+Rights+Audit
history:
    - 
        author: Solen Guitter
        date: '2015-02-26 11:06'
        message: ''
        version: '5'
    - 
        author: Solen Guitter
        date: '2013-12-20 14:13'
        message: Updated export icon and screenshot
        version: '4'
    - 
        author: Solen Guitter
        date: '2013-08-07 11:35'
        message: ''
        version: '3'
    - 
        author: Solen Guitter
        date: '2013-08-06 14:26'
        message: Added export related topics
        version: '2'
    - 
        author: Solen Guitter
        date: '2013-08-06 12:17'
        message: ''
        version: '1'

---
{{! excerpt}}

The [Nuxeo Groups and Rights Audit addon](https://connect.nuxeo.com/nuxeo/site/marketplace/package/nuxeo-groups-rights-audit) generates an Excel matrix listing every exported documents with permissions for each user.

{{! /excerpt}}

Only administrators can do an audit of the permissions of a folder.

**To get an audit of the permissions set on a folder and its children:**

1.  On the folder, click on the icon ![]({{file name='export.png' page='icons-index'}}).
2.  Click on the link **Permission audit export**.
    ![]({{file name='groups-rights-audit-export-link.png'}} ?w=200,border=true,thumbnail=true)
    The export is processes as a background task. You will receive an email with the exported Excel file when it is completed.

&nbsp;

* * *

<div class="row" data-equalizer data-equalize-on="medium"><div class="column medium-6">{{#> panel heading='Related Documentation'}}

*   [Nuxeo Groups and Rights Audit admin doc]({{page space='nxdoc60' page='nuxeo-groups-and-rights-audit'}})
*   [Exporting Documents]({{page page='exporting-documents'}})
*   [Nuxeo Core Import / Export API]({{page space='nxdoc60' page='nuxeo-core-import-export-api'}})

{{/panel}}</div><div class="column medium-6">

&nbsp;

</div></div>