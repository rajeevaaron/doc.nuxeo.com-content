---
title: Naming Conventions
review:
    comment: ''
    date: ''
    status: ok
labels:
    - content-review-6-0
toc: true
confluence:
    ajs-parent-page-id: '12912674'
    ajs-parent-page-title: Administering your project
    ajs-space-key: Studio
    ajs-space-name: Nuxeo Online Services
    canonical: Naming+Conventions
    canonical_source: 'https://doc.nuxeo.com/display/Studio/Naming+Conventions'
    page_id: '9830645'
    shortlink: 9QCW
    shortlink_source: 'https://doc.nuxeo.com/x/9QCW'
    source_link: /display/Studio/Naming+Conventions
tree_item_index: 400
history:
    -
        author: Karin Touchie
        date: '2016-05-27 15:11'
        message: ''
        version: '25'
    -
        author: Solen Guitter
        date: '2015-09-07 09:25'
        message: Fix format and TOC
        version: '24'
    -
        author: Vincent Dutat
        date: '2014-11-20 05:44'
        message: ''
        version: '23'
    -
        author: Solen Guitter
        date: '2013-07-01 10:53'
        message: Updated operation link to use Explorer
        version: '22'
    -
        author: Solen Guitter
        date: '2012-03-06 10:10'
        message: Migrated to Confluence 4.0
        version: '21'
    -
        author: Solen Guitter
        date: '2012-03-06 10:10'
        message: Added TOC
        version: '20'
    -
        author: Frédéric Vadon
        date: '2012-03-05 18:41'
        message: ''
        version: '19'
    -
        author: Frédéric Vadon
        date: '2012-03-05 18:40'
        message: typo
        version: '18'
    -
        author: Frédéric Vadon
        date: '2012-03-05 17:55'
        message: ''
        version: '17'
    -
        author: Frédéric Vadon
        date: '2012-03-05 17:54'
        message: ''
        version: '16'
    -
        author: Frédéric Vadon
        date: '2012-03-05 17:52'
        message: ''
        version: '15'
    -
        author: Frédéric Vadon
        date: '2012-03-05 17:50'
        message: ''
        version: '14'
    -
        author: Frédéric Vadon
        date: '2012-03-05 17:48'
        message: ''
        version: '13'
    -
        author: Frédéric Vadon
        date: '2012-03-05 14:26'
        message: ''
        version: '12'
    -
        author: Frédéric Vadon
        date: '2012-03-05 11:58'
        message: ''
        version: '11'
    -
        author: Frédéric Vadon
        date: '2012-03-05 11:57'
        message: ''
        version: '10'
    -
        author: Frédéric Vadon
        date: '2012-03-05 11:37'
        message: ''
        version: '9'
    -
        author: Frédéric Vadon
        date: '2012-03-05 10:39'
        message: ''
        version: '8'
    -
        author: Alain Escaffre
        date: '2012-03-02 12:59'
        message: ''
        version: '7'
    -
        author: Frédéric Vadon
        date: '2012-03-02 01:16'
        message: ''
        version: '6'
    -
        author: Frédéric Vadon
        date: '2012-03-02 01:13'
        message: ''
        version: '5'
    -
        author: Frédéric Vadon
        date: '2012-03-02 01:12'
        message: ''
        version: '4'
    -
        author: Frédéric Vadon
        date: '2012-03-02 01:11'
        message: ''
        version: '3'
    -
        author: Frédéric Vadon
        date: '2012-03-02 01:11'
        message: ''
        version: '2'
    -
        author: Frédéric Vadon
        date: '2012-03-01 03:16'
        message: ''
        version: '1'

---
This page proposes naming conventions for Studio customization items in order to facilitate usage, maintenance and support. Although not mandatory, we strongly encourage you to follow these conventions, especially if you are a beginner in Studio. For each rule, an example is provided.

## General rules

In general, there are items that will be easily seen by end users (document types for instance) and under the hood features (automation chains for instance). For very exposed items, words should be separated by an underscore and begin with a capital letter (_Contract_Library_) and for others we will prefer compacity: capital letter for every words but no underscore between them (_SendToValidation_ for instance).

As Studio projects can be mixed together or involve many functionalities (ex: contract management, holiday requests...), it is important that items coming from one project or another can be identified easily. The solution is to prefix every item. For example every item that is about contract_management will start with _CM-_ and _HR-_ will apply for holiday request: _CM-SendToValidation_. Prefixing does not apply to document types, and life cycles (Content Model section in Studio). This is particularly mandatory when you do a plugin that will become an [Application Template]({{page page='external-templates'}}) that can be imported into other projects.

## Content Model

*   Document types - Document types are probably the main items and should be created first. Naming should start with a capital letter, and if it needs several words, these one should be separated with an underscore: _Contract_Library_.
*   Schemas - Explicit name (the same as the document type if linked to one) with underscore separating words but all lower case: _contract_library_. You should use a prefix that is either the name or a shorter prefix like _dc_ for dublin_core.
*   Life Cycle - Upper case for each first letter, underscore to separate them. Most of the time a life cycle is only for one document type, in that case, the life cycle should name after the doc type with Lifecycle at the end: _Contract_Lifecycle_.
*   Life Cycle State - all lower case, words separated by underscore. Transitional states and stable states should be distinguished, for instance a document that requires to be corrected and then validated will have the states: _correction_ > _validation_ > _validated_

## Search and Listings

Named like under the hood items: explicit name, no word separation but capital first letter and the project prefix. For example, a content view that shows all contracts in validation state would be: _CM-ContractsInValidation_.

## Automation

### Automation Chain

Automation chain names should contain a verb to describe what they do.

Some automation chains have a UI context, which enables to use UI operations like [Add Info Message](http://explorer.nuxeo.org/nuxeo/site/distribution/current/viewOperation/Seam.AddInfoMessage). These chains should be prefixed with UI. A example of a chain that send a contract to validation from a user action could be: _CM-UI-SendToValidation_.

Other chains that do not have UI context and are mainly called by event handler or other chains should be prefixed with Sub. Example: _CM-Sub-UpdateAcl_.

Items in automation are clearly under the hood items so it would be explicitly named: no word separation but capital first letter and the project prefix. We also added a few rules to standardized the description.

### Workflow

When automation chains are steps of a workflow, a good idea is to prefix them with a number so that they are displayed in the same order they will be triggered. Example with a workflow on the life cycle [Draft <--> Validation --> Validated ]:

*   _CM-01-UI-SendToValidation_
*   _CM-02-UI-SendToValidated_
*   _CM-03-UI-SendBackToDraft_

Note that _CM-02-UI-SendToValidated_ and _CM-02-UI-SendBackToDraft_ have the same number as they can be triggered from the same state (Validation).

### User Actions

User actions should be named after the automation chain they trigger, adding UA- (User Action) after the project prefix (possibly adding the user action category at the end as there can be several user action launching the automation chains).

Example of a user action to send a contract to validation: _CM-UA-SendToValidation_

### Event Handlers

Same rule as user actions but with EH instead of UA: _CM-EH-UpdateAclOnContractCreation_

## Miscellaneous

Every other item should follow the rules given at the top of this page.

Vocabularies should have a name shorter than 13&nbsp;characters, otherwise, it&nbsp;may be too long for some databases

Most of the time, groups of users come from the AD or LDAP, but when created in Studio, it should reflect the role, in full lower case, with a "s" at the end: _members_, _redactors_, _purchasers_...