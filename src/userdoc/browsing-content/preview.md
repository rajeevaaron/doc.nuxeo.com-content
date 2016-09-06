---
title: Preview
labels:
    - annotations
    - preview
    - lts2015-ok
confluence:
    ajs-parent-page-id: '29003022'
    ajs-parent-page-title: Browsing Content
    ajs-space-key: USERDOC710
    ajs-space-name: Nuxeo Platform User Documentation — LTS 2015
    canonical: Preview
    canonical_source: 'https://doc.nuxeo.com/display/USERDOC710/Preview'
    page_id: '29003042'
    shortlink: Io26AQ
    shortlink_source: 'https://doc.nuxeo.com/x/Io26AQ'
    source_link: /display/USERDOC710/Preview
history:
    - 
        author: Manon Lumeau
        date: '2015-10-14 15:02'
        message: ''
        version: '35'
    - 
        author: Gildas Lefevre
        date: '2015-10-13 18:57'
        message: ''
        version: '34'
    - 
        author: Gildas Lefevre
        date: '2015-10-13 18:35'
        message: ''
        version: '33'
    - 
        author: Manon Lumeau
        date: '2015-09-07 14:02'
        message: ''
        version: '32'
    - 
        author: Thibaud Arguillere
        date: '2015-07-12 15:48'
        message: ''
        version: '31'
    - 
        author: Thibaud Arguillere
        date: '2015-07-12 15:47'
        message: ''
        version: '30'
    - 
        author: Solen Guitter
        date: '2015-07-09 12:35'
        message: ''
        version: '29'
    - 
        author: Solen Guitter
        date: '2015-07-09 08:45'
        message: ''
        version: '28'
    - 
        author: Solen Guitter
        date: '2015-07-09 08:33'
        message: ''
        version: '27'
    - 
        author: Solen Guitter
        date: '2015-07-03 14:33'
        message: ''
        version: '26'
    - 
        author: Manon Lumeau
        date: '2015-07-01 12:42'
        message: ''
        version: '25'
    - 
        author: Solen Guitter
        date: '2015-06-22 13:22'
        message: Add new document preview
        version: '24'
    - 
        author: Manon Lumeau
        date: '2014-12-01 16:54'
        message: ''
        version: '23'
    - 
        author: Manon Lumeau
        date: '2014-12-01 16:54'
        message: ''
        version: '22'
    - 
        author: Manon Lumeau
        date: '2014-12-01 16:53'
        message: ''
        version: '21'
    - 
        author: Manon Lumeau
        date: '2014-12-01 16:53'
        message: ''
        version: '20'
    - 
        author: Manon Lumeau
        date: '2014-12-01 16:43'
        message: ''
        version: '19'
    - 
        author: Solen Guitter
        date: '2014-09-11 13:47'
        message: ''
        version: '18'
    - 
        author: Solen Guitter
        date: '2014-09-11 12:13'
        message: ''
        version: '17'
    - 
        author: Solen Guitter
        date: '2014-09-11 12:13'
        message: ''
        version: '16'
    - 
        author: Solen Guitter
        date: '2014-09-11 12:06'
        message: Added formats supported for preview
        version: '15'
    - 
        author: Solen Guitter
        date: '2014-02-24 14:36'
        message: ''
        version: '14'
    - 
        author: Solen Guitter
        date: '2013-11-25 14:33'
        message: ''
        version: '13'
    - 
        author: Solen Guitter
        date: '2013-11-03 14:24'
        message: Added Summary tab displaying number of annotations
        version: '12'
    - 
        author: Solen Guitter
        date: '2013-09-30 17:05'
        message: ''
        version: '11'
    - 
        author: Solen Guitter
        date: '2012-07-04 17:52'
        message: Migrated to Confluence 4.0
        version: '10'
    - 
        author: Solen Guitter
        date: '2012-07-04 17:52'
        message: ''
        version: '9'
    - 
        author: Solen Guitter
        date: '2012-06-27 17:32'
        message: ''
        version: '8'
    - 
        author: Solen Guitter
        date: '2012-01-18 11:16'
        message: Updated screenshots
        version: '7'
    - 
        author: Solen Guitter
        date: '2011-11-24 10:04'
        message: ''
        version: '6'
    - 
        author: Solen Guitter
        date: '2011-06-06 11:44'
        message: ''
        version: '5'
    - 
        author: Solen Guitter
        date: '2010-10-20 10:39'
        message: ''
        version: '4'
    - 
        author: Solen Guitter
        date: '2010-09-14 15:44'
        message: ''
        version: '3'
    - 
        author: Solen Guitter
        date: '2010-09-14 15:44'
        message: ''
        version: '2'
    - 
        author: Solen Guitter
        date: '2010-04-14 11:35'
        message: ''
        version: '1'

---
{{! multiexcerpt name='preview_functional_overview'}}

The preview enables you to see an insight of your document.

Several means to preview documents are available.

*   For office and PDF documents: Click on the icon ![]({{file name='preview.png' page='icons-index'}}) next to the attachment name on the **Summary** tab.
    The preview opens in a new browser tab.
    ![]({{file name='DocumentPreviewer2.png'}} ?w=600,h=331,border=true)
    This preview leverages [pdf.js by Mozzilla](https://mozilla.github.io/pdf.js/).
*   For all documents on the **Summary** tab of a document, click on **More** > **Preview.**
    The document preview is displayed in a popup window.
    ![]({{file name='preview_popup.png'}} ?w=600,border=true)
    This preview leverages the [Nuxeo Platform preview module]({{page space='nxdoc710' page='preview'}}).
*   You can also preview a document along with its main metadata on the [Info-View pop-up]({{page space='nxdoc710' page='how-to-customize-the-info-view-pop-up'}}), accessible from any thumbnail listing.
    ![]({{file name='Screenshot 2014-12-01 16.42.07.png'}} ?w=600,border=true)

## Supported Formats and Requirements

The table below shows for which file formats the preview is available. The installation of [third-party software]({{page space='admindoc710' page='installing-and-setting-up-related-software'}}) is required for some formats of the list below.

{{#> callout type='note' heading='Reminder'}}

To preview files in the formats AI, EPS, PSD and TIFF, the created document must be a 'Picture' and not 'File' type.

{{/callout}}<table><tbody><tr><th colspan="1">File format</th><th colspan="1">Preview supported</th></tr><tr><td colspan="1">.ai</td><td colspan="1">Yes</td></tr><tr><td colspan="1">.doc</td><td colspan="1">Yes</td></tr><tr><td colspan="1">.docx</td><td colspan="1">Yes</td></tr><tr><td colspan="1">.eps</td><td colspan="1">Yes</td></tr><tr><td colspan="1">.html</td><td colspan="1">Yes</td></tr><tr><td colspan="1">.jpg</td><td colspan="1">Yes</td></tr><tr><td colspan="1">.odp</td><td colspan="1">Yes</td></tr><tr><td colspan="1">.ods</td><td colspan="1">Yes</td></tr><tr><td colspan="1">.odt</td><td colspan="1">Yes</td></tr><tr><td colspan="1">.pdf</td><td colspan="1">Yes</td></tr><tr><td colspan="1">.png</td><td colspan="1">Yes</td></tr><tr><td colspan="1">.ppt</td><td colspan="1">Yes</td></tr><tr><td colspan="1">.pptx</td><td colspan="1">Yes</td></tr><tr><td colspan="1">.psd</td><td colspan="1">Yes</td></tr><tr><td colspan="1">.tiff</td><td colspan="1">Yes</td></tr><tr><td colspan="1">.xls</td><td colspan="1">Yes</td></tr><tr><td colspan="1">.xlsx</td><td colspan="1">Yes</td></tr><tr><td colspan="1">.xml</td><td colspan="1">Yes</td></tr></tbody></table>{{! /multiexcerpt}}

&nbsp;

* * *