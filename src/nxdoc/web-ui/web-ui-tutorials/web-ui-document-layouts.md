---
title: "HOWTO: Customize Document Layouts"
review:
    comment: ''
    date: '2017-01-02'
    status: ok
toc: true
details:
    howto:
        excerpt: Learn how to use document layouts with Web UI and View Designer
        level: Intermediate
        tool: View designer, code
        topics: Web UI, View Designer
labels:
    - view-designer
    - nuxeo-web-ui
    - extension
    - layouts
tree_item_index: 400

---
Layouts are used to display a document's metadata in different use cases. It's basically a group of widgets and specifies how these widgets are assembled and displayed.

A given document type can have multiple layouts: a `create` and `edit` layout to set its schema fields when creating or editing the document, a `view` layout to simply display field values and any other layouts according to the application(s) where these are used. In our previous metamodel driven UI, these were known as `modes` and a single layout definition could be rendered in different modes.

{{#> callout type='note' }}
The View Designer is not available for everyone yet, but if you can't wait any longer to try it, do not hesitate to contact your sales representative to enable it on your project.
{{/callout}}

In this tutorial you will learn how to create a document type layout with View Designer from a document type created in Nuxeo Studio and how to add a icon to it.

### Prerequisites

- A [Contract document type]({{page version='' space='nxdoc' page='getting-started-with-nuxeo-studio'}}#step-3-create-a-contract-document-type) created in Nuxeo Studio
- The Web UI addon installed on your instance

Once you are all set, you can navigate to the View Designer application to customize your layouts.

### Create a New Layout

1. Go on the **Layout** tab on the left menu
1. Under **Local Types** you will find your Contract document type
1. Five layouts are available for the customization, let's start with the **Create** layout, click on the **CUSTOMIZE** button in the editor.
  {{#> callout type='tip' }}
  It automatically creates the nuxeo-contract-create-layout.html file, scaffolding:
    - The title and description fields
    - Every custom schema property linked to the document type
  {{/callout}}
1. In the catalog properties on the left, select **File > content:blob > view** and drag and drop the element in the main view.
1. Click on Save.
  {{#> callout type='note' }}
  When customizing a document type, you need to create all layouts in the View Designer in order to display the relevant properties.
  {{/callout}}
1. Click on the **Edit** layout and then on the **CUSTOMIZE** button.
1. Modify and/or delete fields as you wish them to be displayed when editing your document and click on **Save**.
1. Do the same two last steps with the **Metadata** and **View** layouts and don't forget to save.<br/>
  You now have an upload button on your document layout.
1. At the top right-hand corner of the View Designer, switch the editor to code editor.
    You are now in the HTML mode of the View Designer and you can see the element that you just drag and droped on the other editor.
1. Find the description's element and delete it.
1. Click on Save to keep your changes.

You should end up with something like this:
![]({{file name='main-view-contract-vd.png'}} ?w=350,border=true)


The last step is to add your labels to your translations file to display them correctly in the UI. To do so:
1. Click on the **UI** tab (Second tab on the left menu)
2. Click on **Translations**
3. Use the default `messages.json` or create your own language.
4. Create a new entry in the JSON file with key `label.document.type.<document type name>` and the document type name as value. Here it is `"label.document.type.contract":"Contract"`.

You now have a document type ready to be used on your Web UI interface, the last thing that you need is an icon to easily find your document type in your instance.

### Create a Document Type Icon
To do so, you need to go on the **Resources** tab
1. Click on the **UI** folder
2. Click on the **Create** button, select a folder and name it **images**
3. In this **Images** folder, create a `doctypes`folder.
4. In the **doctypes** click on the **Upload** button and select your image. <br/>
  You have to respect the following folder structure, and name the icon as your document type in lowercase in order to display the icon.
  ![]({{file name='tree-structure-icon-contract.png'}} ?w=200,border=true)
  {{#> callout type='warning' }}
  Your image needs to be in .svg format.
  {{/callout}}

You can now deploy your project and see what it looks like!

![]({{file name='contract-web-ui.png'}} ?w=350,border=true)

## Technical Overview

In Nuxeo Web UI, these different layout/mode combinations have been materialized into individual elements, meaning each layout element is bound to a given document type and built for a specific use case. This enables developers to leverage any custom elements they see fit when building their layouts with explicit binding between fields and elements properties - thus doing without the mode handling widget abstraction.

By simply following a convention that maps a document type to a set of pre-defined element names, according to the different use cases (modes), our application can dynamically load and instantiate the appropriate element for a given document.

Regions of the UI which require customizable document layouts rely on custom container elements to handle the loading and instantiation of the appropriate layout elements for the given use case, for instance the default document page has two customizable regions by default:

![]({{file name='document_page_1.png'}} ?w=400,border=true)

1. the `view` layout which usually displays an inline preview of the document and
2. the `metadata` layout which displays the document's metadata.

Each of these regions is defined by a generic layout container element which will import and render the specific layout element for the given document type, in this case `nuxeo-document-view` will rely on `nuxeo-<doctype>-view-layout`and `nuxeo-document-metadata` will use `nuxeo-<doctype>-metadata-layout`, where `<doctype>` is the lower case document type (i.e. *folder*, *note*, *picture*, etc.).

These specific document layout elements are loaded by convention from a `/nuxeo.war/ui/document/<doctype>` folder and the element naming convention adopted is `nuxeo-<doctype>-<mode>-layout.html` where `mode` describes the use case and matches the layout name (i.e. *create*, *view* , *edit*, *import*, etc):

```text
/ui
  └──── document
    ├─── <doctype>
    |  ├── nuxeo-<doctype>-<layout>-layout.html
    │  └── ...
    └── ...
```
The following table lists layout elements used by Nuxeo Web UI:

| Element                      | Layout                             |                                                                  |
|:-----------------------------|:-----------------------------------|:-----------------------------------------------------------------|
| 1. nuxeo-document-view       | nuxeo-{doctype}-view-layout        | ![]({{file name='document_page_1.png'}} ?w=100,border=true)      |
| 2. nuxeo-document-metadata   | nuxeo-{doctype}-metadata-layout    | ![]({{file name='document_page_1.png'}} ?w=100,border=true)      |
| 3. nuxeo-document-edit       | nuxeo-{doctype}-edit-layout        | ![]({{file name='document_page_2.png'}} ?w=100,border=true)      |
| 4. nuxeo-document-create     | nuxeo-{doctype}-create-layout      | ![]({{file name='document_create.png'}} ?w=100,border=true)      |
| 5. nuxeo-document-import     | nuxeo-{doctype}-import-layout      | ![]({{file name='document_import.png'}} ?w=100,border=true)      |

{{#> callout type='warning' }}

Nuxeo Web UI relies on the existence of these document layout elements so, for each registered document type, the application will attempt to load its specific layouts. Since there is no layout registry, there is no fallback.

{{/callout}}

When instantiating and stamping these layout elements, a `document` property will be set to the current document and the layout is expected to update accordingly. The importing and stamping of these elements, executed by container, is simply:

```
this.importHref(this.resolveUrl(layout + '.html'), function() {
  // create the element
  var el = document.createElement(layout);
  // append it to the parent
  this.appendChild(el);
  // setup data binding
  el.document = this.document;
},
```
By building these elements with Polymer, developers can simply declare this `document` property and rely on native data binding support to bind document fields to the the template.

Since these document layouts are regular custom elements, developers have no limitations whatsoever in terms of HTML template, CSS styling or JS logics and are free to leverage any third party libraries or elements when building their own.

{{#> callout type='warning' }}

Nuxeo Web UI is vulcanized for production meaning that shared dependencies are already bundled and loaded in the application and should not be imported again in dynamically loaded elements.
If your elements have other unshared static dependencies then these should be deployed as well.

{{/callout}}

Layouts created and managed by Nuxeo Studio however will rely on introspection of the HTML and JS in order to provide validation and tooling support and will include some extra metadata, in the form of HTML attributes and/or JSDoc annotations.

The following is a sample definition of what an `edit` layout element for a `MyDoc` document type would look like, including Nuxeo Studio generated metadata:

```
<dom-module id="nuxeo-mydoc-edit-layout">
  <template>
    <!-- Custom CSS -->
    <style>
      *[role=widget] {
        padding: 5px;
      }
    </style>

    <!-- A paper-input widget bound to 'dc:title' -->
    <paper-input role="widget" value="\{{document.properties.dc:title}}" label="Title"></paper-input>

    <!-- An input of type date bound to 'my:date' -->
    <div role="widget" class="item">
      <label>My date</label>
      <input type="date" value="[[document.properties.my:date]]">
    </div>

    <!-- Any HTML -->
    <img src="assets/imgs/logo.png">

  </template>

  <script>
    Polymer({
      is: 'nuxeo-mydoc-edit-layout',
      properties: {
        /**
         * You can use either @schema or @doctype annotation in your model.
         * Use to provide validation and auto completion of document properties.
         * @doctype MyDoc
         */
        document: {
          type: Object
        }
      }
    });
  </script>
</dom-module>
```

### Customizing Document Creation

When contributing document types, there are two more contributions that have to be made in order for them to
be properly displayed in the document creation wizard: add a custom icon and a custom label for each type.

The icon must be contributed to `/nuxeo.war/ui/images/doctypes`, named after the format `{doctype}.svg`. Labels must be added to the message files under the format `label.document.type.{doctype}`. In both cases, `doctype` must be the lower case doctype **id**.

{{#> callout type='note' }}
For more information about contributing labels, please check the page [Managing Translations]({{page page='web-ui-managing-translations'}}).
{{/callout}}

You can prevent a doctype from being created via document creation wizard by adding to it the `HiddenInCreation` facet.

Doctypes can also be prevented from being imported even if they can be created, through the `window.nuxeo.importBlacklist`
array, which holds all the doctypes that **cannot** be created. You can simply add or override this array to fit your needs.
One could prevent the `Workspace`, `Folder`, `OrderedFolder` and `Collection` doctypes from being imported on the Web UI
by deploying the following as a WebResource:

```
window.nuxeo.importBlacklist = [
 'Workspace', 'Folder', 'OrderedFolder', 'Collection'
];
```

{{#> callout type='warning' }}
The customization of document creation in Web UI is subject to change in the upcoming versions.
{{/callout}}

{{#> callout type='info' heading='Learn more'}}

*   [Document and Workflow Task Layouts with Nuxeo Studio Designer course from Nuxeo University](https://university.nuxeo.com/store/187249-document-and-workflow-task-layouts-with-nuxeo-studio-designer)

{{/callout}}  

* * *

<div class="row" data-equalizer data-equalize-on="medium"><div class="column medium-6">{{#> panel heading='Related Documentation'}}

- [Customization Tutorial]({{page page='web-ui-customization-tutorial'}}).

{{/panel}}</div><div class="column medium-6">
</div></div>
