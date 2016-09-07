---
title: Adding Custom LDAP Fields to the UI
labels:
    - ldap
confluence:
    ajs-parent-page-id: '17334469'
    ajs-parent-page-title: Authentication and User Management
    ajs-space-key: NXDOC58
    ajs-space-name: 'Nuxeo Platform Developer Documentation - 5.8 '
    canonical: Adding+Custom+LDAP+Fields+to+the+UI
    canonical_source: >-
        https://doc.nuxeo.com/display/NXDOC58/Adding+Custom+LDAP+Fields+to+the+UI
    page_id: '17334500'
    shortlink: 5IAIAQ
    shortlink_source: 'https://doc.nuxeo.com/x/5IAIAQ'
    source_link: /display/NXDOC58/Adding+Custom+LDAP+Fields+to+the+UI
history:
    - 
        author: Solen Guitter
        date: '2016-09-02 07:37'
        message: ''
        version: '10'
    - 
        author: Solen Guitter
        date: '2015-08-28 10:03'
        message: ''
        version: '9'
    - 
        author: Solen Guitter
        date: '2013-09-04 17:38'
        message: ''
        version: '8'
    - 
        author: Solen Guitter
        date: '2013-09-04 10:44'
        message: Added related pages
        version: '7'
    - 
        author: Florent Guillaume
        date: '2011-01-17 12:41'
        message: Migrated to Confluence 4.0
        version: '6'
    - 
        author: Florent Guillaume
        date: '2011-01-17 12:41'
        message: ''
        version: '5'
    - 
        author: Wojciech Sulejman
        date: '2011-01-14 11:42'
        message: ''
        version: '4'
    - 
        author: Wojciech Sulejman
        date: '2011-01-14 11:34'
        message: ''
        version: '3'
    - 
        author: Wojciech Sulejman
        date: '2011-01-13 18:27'
        message: ''
        version: '2'
    - 
        author: Wojciech Sulejman
        date: '2011-01-13 18:26'
        message: ''
        version: '1'

---
&nbsp;

{{! excerpt}}

To add a custom LDAP fields to the User interface you have to:

{{! /excerpt}}

1.  Create a custom schema based on nuxeo's user.xsd schema with custom fields related to the fields in your LDAP system.

    {{#> panel type='code' heading='schemas/myuser.xsd'}}

    ```xml
     <?xml version="1.0"?>
     <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
        xmlns:nxs="http://www.nuxeo.org/ecm/schemas/myuser"
        targetNamespace="http://www.nuxeo.org/ecm/schemas/myuser">

      <xs:include schemaLocation="base.xsd" />

      <xs:element name="username" type="xs:string" />
      <xs:element name="password" type="xs:string" />
      <xs:element name="email" type="xs:string" />
      <xs:element name="firstName" type="xs:string" />
      <xs:element name="lastName" type="xs:string" />
      <xs:element name="company" type="xs:string" />
      <!-- your custom telephone field -->
      <xs:element name="telephone" type="xs:string" />

      <xs:element name="groups" type="nxs:stringList" />

     </xs:schema>

    ```

    {{/panel}}
2.  Add your schema via Nuxeo's extension system.

    {{#> panel type='code' heading='OSGI-INF/schema-contrib.xml'}}

    ```xml
     <?xml version="1.0"?>
     <component name="com.example.myproject.myuser.schema">
      <extension target="org.nuxeo.ecm.core.schema.TypeService" point="schema">
        <schema name="myuser" src="schemas/myuser.xsd" />
      </extension>
     </component>

    ```

    {{/panel}}
3.  Modify your LDAP configuration file in Nuxeo (`default-ldap-users-directory-bundle.xml`) to include:
    1.  your custom schema,

        {{#> panel type='code' heading='default-ldap-users-directory-bundle.xml'}}

        ```xml
          <extension target="org.nuxeo.ecm.directory.ldap.LDAPDirectoryFactory"
            point="directories">

            <directory name="userDirectory">
              <server>default</server>
              <!-- association between your custom schema and the directory -->
              <schema>myuser</schema>

        ```

        {{/panel}}
    2.  mapping between your schema and your LDAP fields.

        {{#> panel type='code' heading='default-ldap-users-directory-bundle.xml (continued)'}}

        ```xml
              <fieldMapping name="username">uid</fieldMapping>
              <fieldMapping name="password">userPassword</fieldMapping>
              <fieldMapping name="firstName">givenName</fieldMapping>
              <fieldMapping name="lastName">sn</fieldMapping>
              <fieldMapping name="company">o</fieldMapping>
              <fieldMapping name="email">mail</fieldMapping>
              <fieldMapping name="telephone">telephoneNumber</fieldMapping>

        ```

        {{/panel}}
4.  Modify the UI.
    1.  Add your custom widget to the layout.

        {{#> panel type='code' heading='default-ldap-users-directory-bundle.xml(continued)'}}

        ```xml
         <extension target="org.nuxeo.ecm.platform.forms.layout.WebLayoutManager"
            point="layouts">

            <layout name="user">
              <templates>
                <template mode="any">/layouts/layout_default_template.xhtml</template>
              </templates>
              <rows>
                <row>
                  <widget>username</widget>
                </row>
              <row>
              <!-- your custom telephone widget-->
                  <widget>telephone</widget>
                </row>

        ```

        {{/panel}}
    2.  Define a new widget for your custom field to be used in the layout above.

        {{#> panel type='code' heading='default-ldap-users-directory-bundle.xml(continued)'}}

        ```xml
         <widget name="telephone" type="text">
         <labels>
         <label mode="any">telephone</label>
         </labels>
         <translated>true</translated>
         <fields>
         <field schema="myuser">telephone</field>
         </fields>
         <widgetModes>
         <mode value="editPassword">hidden</mode>
         </widgetModes>
         <properties widgetMode="edit">
         <property name="required">true</property>
         <property name="styleClass">dataInputText</property>
         </properties>
         </widget>

        ```

        {{/panel}}

        &nbsp;

        &nbsp;

&nbsp;