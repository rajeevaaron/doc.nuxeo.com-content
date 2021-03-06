---
title: "HOWTO: Customize Slots"
review:
    comment: ''
    date: '2017-01-16'
    status: ok
toc: true
details:
    howto:
        excerpt: Learn how to use Nuxeo Slots with Web UI.
        level: Advanced
        tool: code
        topics: Web UI
labels:
    - lts2016-ok
    - nuxeo-web-ui
    - extension
    - nuxeo-slot
    - nuxeo-slot-content
tree_item_index: 800

---
## Concept

Nuxeo slots are mechanisms which extend part of the Nuxeo Web UI. A couple of `nuxeo-slots` are hardcoded in the web-ui source code in order to insert your own HTML elements and extend the Web UI to meet your specific needs.

The concept is simple. Let's assume we introduced the following slot somewhere in the Web UI:

```xml
<nuxeo-slot slot="MY_SLOT_NAME" model="[[mySlotModel]]"></nuxeo-slot>
```
then you can easily define your own content with:

```xml
<nuxeo-slot-content name="defaultMY_SLOT_NAME" slot="MY_SLOT_NAME">
  <template>
      <my-element my-element-property=[[aPropertyFromTheSlotModel]]></my-element>
  </template>
</nuxeo-slot-content>
```
and this content will be inserted in the DOM right before where the `nuxeo-slot` is located.

You can see that `my-element` has its `my-element-property` bound to `aPropertyFromTheSlotModel` which is made available by the `nuxeo-slot` model. The `[[mySlotModel]]` is an object which has a `aPropertyFromTheSlotModel` property.

For a better understanding, please refer to the [DOCUMENT_ACTIONS](#document_actions) and where we concretely detail how additional document actions are added by the [Nuxeo Drive]({{page version='' space='nxdoc' page='nuxeo-drive'}}) addon.

## Web UI Slots

### Summary
Here are the `nuxeo-slots` available in the Nuxeo Web UI.

{{#> callout type='warning' }}

The name and the location of the following slots are temporary and subject to change until the release of the final version.

{{/callout}}

| Slot name                                                                                       | Extension purpose                               | Where                                                               |
|:------------------------------------------------------------------------------------------------|:------------------------------------------------|:--------------------------------------------------------------------|
| [DOCUMENT_ACTIONS](#document_actions)                                                           | Additional Current document actions             | ![]({{file name='DOCUMENT_ACTIONS.png'}} ?w=100,border=true)        |
| [DOCUMENT_VIEWS_ITEMS](#document_view_items) <br/> [DOCUMENT_VIEWS_PAGES](#document_view_items) | Additional Current document views               | ![]({{file name='DOCUMENT_VIEWS_ITEMS.png'}} ?w=100,border=true)    |
| [BLOB_ACTIONS](#blob_actions)                                                                   | Additional Current document blobs actions       | ![]({{file name='BLOB_ACTIONS.png'}} ?w=100,border=true)            |
| [BROWSE_ACTIONS](#browse_actions)                                                               | Additional Current browse selection actions     | ![]({{file name='BROWSE_ACTIONS.png'}} ?w=100,border=true)          |
| [SEARCH_MENU_BUTTONS](#search_menu_buttons)                                                     | Additional Search                               | ![]({{file name='SEARCH_MENU_BUTTONS.png'}} ?w=100,border=true)     |
| [ADMINISTRATION_MENU](#administration_menu) <br/> [ADMINISTRATION_PAGES](#administration_menu)  | Additional Administration menu                  | ![]({{file name='ADMINISTRATION_MENU.png'}} ?w=100,border=true)     |
| [USER_MENU](#user_menu)                                                                         | Additional User menu                            | ![]({{file name='USER_MENU.png'}} ?w=100,border=true)               |
| [DRAWER_PAGES](#drawer_pages) <br/> [PAGES](#drawer_pages)                                      | Additional main menu items                      | ![]({{file name='DRAWER_PAGES.png'}} ?w=100,border=true)            |
| [DOCUMENT_CREATE_ACTIONS](#document_create_actions)                                             | Additional document creation actions            | ![]({{file name='DOCUMENT_CREATE_ACTIONS.png'}} ?w=100,border=true) |
| [FILE_UPLOAD_ACTIONS](#file_upload_actions)                                                     | Additional document import wizards              | ![]({{file name='FILE_UPLOAD_ACTIONS.png'}} ?w=100,border=true)     |
| [SEARCH_ACTIONS](#search_actions)                                                               | Additional search results selection actions     | ![]({{file name='SEARCH_ACTIONS.png'}} ?w=100,border=true)          |
| [COLLECTION_ACTIONS](#collection_actions)                                                       | Additional collection members selection actions | ![]({{file name='COLLECTION_ACTIONS.png'}} ?w=100,border=true)      |
| [ANALYTICS_ITEMS](#analytics_pages) <br/> [ANALYTICS_PAGES](#analytics_pages)                   | Additional analytics pages                      | ![]({{file name='ANALYTICS_ITEMS.png'}} ?w=100,border=true)         |

### Details

#### Document Browsing Slots

The following slots allow you to extend available pages and actions when browsing a given document. They are located in [nuxeo-browser.html](https://github.com/nuxeo/nuxeo-web-ui/blob/0.8/elements/nuxeo-browser/nuxeo-browser.html).

##### DOCUMENT_ACTIONS{{> anchor 'document_actions'}}

This slot defines the available top right actions to be performed on the current document such as *Add to collection*, *Share*, *Export*, etc.

![]({{file name='DOCUMENT_ACTIONS.png'}} ?w=400,border=true)

A typical use case for extending the Web UI is you'd like to add new document actions. Let's have a look on how it is done with the [Nuxeo Drive]({{page version='' space='nxdoc' page='nuxeo-drive'}}) addon which adds a new action to synchronize a document with the local file system.

First, the DOCUMENT_ACTIONS `nuxeo-slot` is defined in the [nuxeo-browser.html](https://github.com/nuxeo/nuxeo-web-ui/blob/0.8/elements/nuxeo-browser/nuxeo-browser.html#L181) element like this:
```xml
<div class="document-actions">
  <nuxeo-slot slot="DOCUMENT_ACTIONS" model="[[actionContext]]"></nuxeo-slot>
</div>
```
and the web UI defines the default slot content in [nuxeo-document-actions.html](https://github.com/nuxeo/nuxeo-web-ui/blob/0.8/elements/nuxeo-document-actions/nuxeo-document-actions.html#L31)
```xml
<nuxeo-slot-content name="defaultDocumentActions" slot="DOCUMENT_ACTIONS">
  <template>
    <nuxeo-add-to-collection-button document="[[document]]"></nuxeo-add-to-collection-button>
    <nuxeo-favorites-toggle-button document="[[document]]"></nuxeo-favorites-toggle-button>
    <nuxeo-share-button document="[[document]]"></nuxeo-share-button>
    <nuxeo-notifications-toggle-button document="[[document]]"></nuxeo-notifications-toggle-button>
    <nuxeo-clipboard-toggle-button document="[[document]]" clipboard="[[clipboard]]"></nuxeo-clipboard-toggle-button>
    <nuxeo-lock-toggle-button document="[[document]]"></nuxeo-lock-toggle-button>
    <nuxeo-preview-button document="[[document]]"></nuxeo-preview-button>
    <nuxeo-export-button document="[[document]]"></nuxeo-export-button>
    <nuxeo-download-button document="[[document]]"></nuxeo-download-button>
  </template>
</nuxeo-slot-content>
```
Now, the Nuxeo Drive addon is able to add its own actions there with [nuxeo-drive.html](https://github.com/nuxeo/nuxeo-drive-server/blob/8.10/nuxeo-drive-web-ui/src/main/resources/web/nuxeo.war/ui/nuxeo-drive/nuxeo-drive.html#L8):
```xml
<nuxeo-slot-content name="driveSyncToggleButton" slot="DOCUMENT_ACTIONS">
  <template>
    <nuxeo-drive-sync-toggle-button document="[[document]]"></nuxeo-drive-sync-toggle-button>
  </template>
</nuxeo-slot-content>
```
Next, in the above snippet, how is the bound `document` property resolved? The answer is found in [nuxeo-browser.html](https://github.com/nuxeo/nuxeo-web-ui/blob/0.8/elements/nuxeo-browser/nuxeo-browser.html#L258) which defines the model of the `DOCUMENT_ACTIONS` slot through the computed `actionContext` property defined [here](https://github.com/nuxeo/nuxeo-web-ui/blob/0.8/elements/nuxeo-browser/nuxeo-browser.html#L258):
```javascript
_actionContext: function() {
  return {document: this.document, clipboard: this.clipboard};
},
```
The `DOCUMENT_ACTIONS` has therefore the following:

**Slot Model Properties**

| Property    | Description                        |
|:------------|:-----------------------------------|
| `document`  | The current document.              |
| `clipboard` | The application clipboard content. |

##### DOCUMENT_VIEWS_ITEMS and DOCUMENT_VIEWS_PAGES{{> anchor 'document_view_items'}}

The **DOCUMENT_VIEWS_ITEMS** slot allows you to define the available items to navigate the current document views such as *View*, *Permissions* and *History*.

![]({{file name='DOCUMENT_VIEWS_ITEMS.png'}} ?w=400,border=true)

The **DOCUMENT_VIEWS_PAGES** slot must define the pages introduced by the **DOCUMENT_VIEWS_ITEMS** slot. Each new item of **DOCUMENT_VIEWS_ITEMS** slot triggers a navigation to a page defined in this slot.

**Slot Model Properties**

| Property   | Description           |
|:-----------|:----------------------|
| `document` | The current document. |

##### BLOB_ACTIONS{{> anchor 'blob_actions'}}

This slot is available on a current document that has attached blobs. Default actions are *Preview*, *Delete* and *Open with Nuxeo Drive* (when the [Nuxeo Drive]({{page version='' space='nxdoc' page='nuxeo-drive'}}) addon is installed).

![]({{file name='BLOB_ACTIONS.png'}} ?w=400,border=true)

**Slot Model Properties**

| Property   | Description                                                                                                                        |
|:-----------|:-----------------------------------------------------------------------------------------------------------------------------------|
| `document` | The current document.                                                                                                              |
| `blob`     | The blob description (i.e. `{name: "nuxeo_fact sheet_0.1.png", mime-type: "image/png", encoding: null, digestAlgorithm: "MD5",…}`) |
| `xpath`    | The blob property xpath                                                                                                            |

##### BROWSE_ACTIONS{{> anchor 'browse_actions'}}

This slot is displayed when selecting one or more children documents of a Folderish current document. It provides bulked actions on the selection such as *Add to collection*, *Delete selected items*, etc.

![]({{file name='BROWSE_ACTIONS.png'}} ?w=400,border=true)

**Slot Model Properties**

| Property        | Description                             |
|:----------------|:----------------------------------------|
| `document`      | The current document.                   |
| `selectedItems` | An array of selected content documents. |

#### Main Application Menu slots

The Web UI revolves around a left drawer menu allowing to navigate to documents, searches, application administration, collections, etc. The following slots show how to extend this menu.

##### SEARCH_MENU_BUTTONS{{> anchor 'search_menu_buttons'}}

This slot allows you to define additional searches accessible from the left menu.

![]({{file name='SEARCH_MENU_BUTTONS.png'}} ?w=400,border=true)

The [Nuxeo DAM](https://github.com/nuxeo/nuxeo-dam/blob/8.10/nuxeo-dam-web-ui/src/main/resources/web/nuxeo.war/ui/nuxeo-dam/nuxeo-dam.html) addon defines its own `Asset Search` with the following:
```xml
<nuxeo-slot-content name="damSearchMenuButtons" slot="SEARCH_MENU_BUTTONS">
  <template>
    <nuxeo-menu-icon id="assetsSearchButton" name="assetsSearch" route="search:assets" icon="icons:perm-media" label="dam.assets.heading">
    </nuxeo-menu-icon>
  </template>
</nuxeo-slot-content>
```
See this [documentation]({{page version='' space='nxdoc' page='web-ui-search'}}) for further information on creating your own search for the Web UI.

**Slot Model Properties**

| Property      | Description           |
|:--------------|:----------------------|
| `document`    | The current document. |
| `currentUser` | The current user.     |

##### ADMINISTRATION_MENU and ADMINISTRATION_PAGES{{> anchor 'administration_menu'}}

This ADMINISTRATION_MENU slot allows you to add additional Administration sub menus.

![]({{file name='ADMINISTRATION_MENU.png'}} ?w=400,border=true)

**Slot Model Properties**

| Property      | Description           |
|:--------------|:----------------------|
| `document`    | The current document. |
| `currentUser` | The current user.     |

##### USER_MENU{{> anchor 'user_menu'}}

This USER_MENU slot allows you to add additional User sub menu items.

![]({{file name='USER_MENU.png'}} ?w=400,border=true)

On the above screenshot, you can see there's a Nuxeo Drive User menu item which is not part of the default Web UI. It is extended by the [Nuxeo Drive]({{page version='' space='nxdoc' page='nuxeo-drive'}}) addon which contributes the `USER_MENU` slot with [nuxeo-drive.html](https://github.com/nuxeo/nuxeo-drive-server/blob/8.10/nuxeo-drive-web-ui/src/main/resources/web/nuxeo.war/ui/nuxeo-drive/nuxeo-drive.html#L24):

```xml
<nuxeo-slot-content name="drivePageLink" slot="USER_MENU">
  <template>
    <nuxeo-menu-item route="page:drive" label="app.user.drive"></nuxeo-menu-item>
  </template>
</nuxeo-slot-content>
```
Note the:
```properties
route="page:drive"
```
Thanks to this property, clicking on this menu item will navigate to a page named `drive`. This page is also contributed to the Web UI with the [PAGES](#drawer_pages) slot extension also from [nuxeo-drive.html](https://github.com/nuxeo/nuxeo-drive-server/blob/8.10/nuxeo-drive-web-ui/src/main/resources/web/nuxeo.war/ui/nuxeo-drive/nuxeo-drive.html#L31):

```xml
<nuxeo-slot-content name="drivePage" slot="PAGES">
  <template>
    <nuxeo-drive-page class="flex" name="drive"></nuxeo-drive-page>
  </template>
</nuxeo-slot-content>
```
which will be inserted in [nuxeo-app.html](https://github.com/nuxeo/nuxeo-web-ui/blob/0.8/elements/nuxeo-app/nuxeo-app.html#L338).

**Slot Model Properties**

| Property      | Description           |
|:--------------|:----------------------|
| `document`    | The current document. |
| `currentUser` | The current user.     |

##### DRAWER_PAGES and PAGES {{> anchor 'drawer_pages'}}

The `DRAWER_PAGES` allows you to add new items to the main left drawer menu (see screenshot below) and works exactly the same as the [USER_MENU](#user_menu).

![]({{file name='DRAWER_PAGES.png'}} ?w=400,border=true)

**Slot Model Properties**

| Property      | Description           |
|:--------------|:----------------------|
| `document`    | The current document. |
| `currentUser` | The current user.     |

#### Document Creation

##### DOCUMENT_CREATE_ACTIONS{{> anchor 'document_create_actions'}}

This slot displays actions when hovering over the bottom right **Floating Action Button** to create new documents. By default, it inserts [nuxeo-document-create-shortcuts.html](https://github.com/nuxeo/nuxeo-web-ui/blob/0.8/elements/nuxeo-document-create-actions/nuxeo-document-create-shortcuts.html) which shows shortcuts to the latest created document types wizard.

![]({{file name='DOCUMENT_CREATE_ACTIONS.png'}} ?w=400,border=true)

**Slot Model Properties**

| Property      | Description                                                              |
|:--------------|:-------------------------------------------------------------------------|
| `hostVisible` | Boolean which is true if hovering over the FAB.                             |
| `subtypes`    | Array of the document types that can be created in the current location. |


##### FILE_UPLOAD_ACTIONS{{> anchor 'file_upload_actions'}}

This slot is used in the [Nuxeo Live Connect]({{page version='' space='nxdoc' page='nuxeo-liveconnect'}}) addon which inserts additional import wizards to upload Files to cloud services.

![]({{file name='FILE_UPLOAD_ACTIONS.png'}} ?w=400,border=true)

**Slot Model Properties**

There are no properties for this slot.

#### Search and Collection Browsing Slots

The screen to browse Search results and Collection contents are very similar. When selecting items in the search results or the collection contents, some bulk actions are displayed in a top menu bar (like [BROWSE_ACTIONS](#browse_actions)). These actions can be extended with the following slots.

##### SEARCH_ACTIONS{{> anchor 'search_actions'}}

![]({{file name='SEARCH_ACTIONS.png'}} ?w=400,border=true)

**Slot Model Properties**

| Property        | Description                                                                                                                                     |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| `items`         | Array of displayed result items returned by the search, selected or not. (Note: more results could be loaded if you keep scrolling for results) |
| `selectedItems` | Array of selected results.                                                                                                                      |

##### COLLECTION_ACTIONS{{> anchor 'collection_actions'}}

![]({{file name='COLLECTION_ACTIONS.png'}} ?w=400,border=true)

**Slot Model Properties**

| Property        | Description                                                                                                                    |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------|
| `items`         | Array of displayed collection members, selected or not. (Note: more members could be loaded if you keep scrolling for results) |
| `selectedItems` | Array of selected collection members.                                                                                          |

#### Other Slots

##### ANALYTICS_ITEMS and ANALYTICS_PAGES {{> anchor 'analytics_pages'}}

The **ANALYTICS_ITEMS** slot allows to define the available items in the analytics dashboard (accessible from the Admin menu) such as  *Document distribution*, *Repository content*, *Workflow*, etc.

![]({{file name='ANALYTICS_ITEMS.png'}} ?w=400,border=true)

**Slot Model Properties**

There are no properties for this slot.
