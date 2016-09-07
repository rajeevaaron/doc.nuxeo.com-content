---
title: Using Kerberos
toc: true
confluence:
    ajs-parent-page-id: '16810077'
    ajs-parent-page-title: 'Authentication, users and groups'
    ajs-space-key: ADMINDOC58
    ajs-space-name: Nuxeo Installation and Administration - 5.8
    canonical: Using+Kerberos
    canonical_source: 'https://doc.nuxeo.com/display/ADMINDOC58/Using+Kerberos'
    page_id: '16809990'
    shortlink: BoAAAQ
    shortlink_source: 'https://doc.nuxeo.com/x/BoAAAQ'
    source_link: /display/ADMINDOC58/Using+Kerberos
history:
    - 
        author: Gildas Lefevre
        date: '2014-07-29 14:58'
        message: ackport modifications done in the FTS documentation
        version: '9'
    - 
        author: Solen Guitter
        date: '2014-07-22 10:12'
        message: Added missing param in Windows commands
        version: '8'
    - 
        author: Solen Guitter
        date: '2014-04-14 11:41'
        message: ''
        version: '7'
    - 
        author: Solen Guitter
        date: '2013-10-14 16:45'
        message: ''
        version: '6'
    - 
        author: Solen Guitter
        date: '2013-03-20 23:07'
        message: ''
        version: '5'
    - 
        author: Solen Guitter
        date: '2013-03-20 22:57'
        message: ''
        version: '4'
    - 
        author: Solen Guitter
        date: '2013-03-20 22:51'
        message: ''
        version: '3'
    - 
        author: Laurent Doguin
        date: '2013-03-20 22:11'
        message: add missing word....
        version: '2'
    - 
        author: Laurent Doguin
        date: '2013-03-15 18:39'
        message: ''
        version: '1'

---
&nbsp;

### Configuring Kerberos on Windows

1.  In Microsoft Active Directory, create a user. Option "password does not expire" must be checked and "user must change password" unchecked.
2.  In a command-line window, register the service principal name(s) you want this user to respond to (generally the server name in its short and fully qualified forms and it corresponds to the Nuxeo Platform server name).

    {{#> callout type='note' heading='Service principal name format'}}

    The service principal _has_&nbsp;to correspond to the server's canonical name in DNS.

    {{/callout}}

    <pre>setspn -a HTTP/servername@REALM <username>
    setspn -a HTTP/fully.qualified.servername@REALM <username></pre>

3.  Check the SPNs with the command:

    <pre>setspn -l <username></pre>

4.  Export the keytab.

    <pre>ktpass /out C:\temp\http.keytab /princ HTTP/servername@REALM /mapuser domain\username /pass password /crypto RC4-HMAC-NT /ptype KRB5_NT_PRINCIPAL /kvno 0</pre>

    `/mapuser` and `/pass` should not be strictly necessary after `setspn`, but better be safe than sorry.

### Generic

1.  Copy the `keytab` on your Nuxeo server.

2.  Configure krb5.conf (`/etc/krb5.conf` or `C:\Windows\krb5.ini`, depending on the OS).

    {{#> callout type='tip' }}

    Note that on Linux servers, while it is not strictly necessary, you should really install the MIT Kerberos user tools (krb5-user in Debian-like). This will setup a basic `krb5.conf` and give you debugging tools.

    {{/callout}}

    The&nbsp;`krb5.conf` should minimally contain:

    <pre> [libdefaults]
            default_realm    REALM
         [realms]
              REALM = {
                   kdc = <kdc>
                   admin_server = <admin_server>
              }
         [domain_realm]
              domain = REALM
              .domain = REALM</pre>

    *   `kdc` and `admin_server` are the names of your kdc and admin servers (duh). On Windows, both are your AD server. On Linux, `kdc` is where you've installed `krb5-kdc` and `admin_server` in where you've installed `krb5-admin-server`. Usually it's the same machine.

    *   `domain` is the DNS domain you want to map to a realm. In Linux clients, specifying the realm is necessary. It's not on Windows, but again, better be safe than sorry.

3.  Test the Kerberos installation. On Linux servers, you can test it with the command:

    <pre>kinit -k -t /path/to/keytab HTTP/servername@REALM</pre>

    There should be no errors and klist should list a krbtgt for your service. On my machine that looks like this:

    <pre>Ticket cache: FILE:/tmp/krb5cc_1000
    Default principal: HTTP/loremipsum@LOREMIPSUM
    Valid starting     Expires            Service principal
    15/12/12 11:35:36  15/12/12 21:35:36  krbtgt/LOREMIPSUM@LOREMIPSUM
      renew until 16/12/12 11:35:36</pre>

4.  If you use your server as a Kerberos client too (e.g. it's a development machine!), delete the tgt with the command `kdestroy`.

### Configuring Java

To enable Kerberos, you need to use a login configuration implementation. You have two ways of doing this. Either change the default Java configuration or use JAVA_OPTS. Take a look at&nbsp;[JAAS](http://docs.oracle.com/javase/6/docs/technotes/guides/security/jaas/JAASRefGuide.html#AppendixA) documentation for more details. If you have installed the Marketplace package, the JAVA_OPTS is automatically added.

#### Changing the Default JRE Configuration

1.  In $`JAVA_HOME/jre/lib/security/java.security`, find the login config url (it's commented out by default):

    <pre>   #login.config.url.1=file:${user.home}/.java.login.config </pre>

2.  Set this to a regular file, e.g. `/opt/nuxeo/java.login.config.`

#### Giving a Custom Login File as Java Argument

In&nbsp;`nuxeo.conf`, add the following line:

<pre>JAVA_OPTS=$JAVA_OPTS -Djava.security.auth.login.config=./java.login.config </pre>

Note that using one equal sign appends or overrides parts of the default `java.security` file, whereas using two equal signs completely overrides the default `java.security` file content.

<pre>JAVA_OPTS=$JAVA_OPTS -Djava.security.auth.login.config==./java.login.config </pre>

### Configuring JAAS

If you have installed the Marketplace package, this file is already available at `$NUXEO_HOME/java.login.config.`

Open the `java.login.config` file you've setup and add the following configuration:

<pre>   Nuxeo {
  com.sun.security.auth.module.Krb5LoginModule required
  debug=true
  storeKey=true
  useKeyTab=true
  keyTab="/complete/path/to/keytab"
  principal="HTTP/servername@REALM";
 }; 
</pre>

{{#> callout type='note' heading='Login configuration name'}}

The login configuration MUST be called Nuxeo with an uppercase N.

{{/callout}}

### Configuring Nuxeo (We're Almost There!)

1.  Deploy the bundle in `$NUXEO_HOME/nxserver/bundles`.
2.  Create a `$NUXEO_HOME/nxserver/config/kerberos-config.xml` with the following content:

    ```html/xml
    <?xml version="1.0"?>
     <component name="Kerberos-config">
      <require>org.nuxeo.ecm.platform.ui.web.auth.WebEngineConfig</require>
      <require>org.nuxeo.ecm.platform.ui.web.auth.defaultConfig</require>
      <require>org.nuxeo.ecm.platform.login.Kerberos</require>
      <documentation> This Authentication Plugin uses Kerberos to assert user identity. </documentation>
      <extension target="org.nuxeo.ecm.platform.ui.web.auth.service.PluggableAuthenticationService" point="authenticators">
       <authenticationPlugin name="KRB5_AUTH" enabled="true" class="org.nuxeo.ecm.platform.ui.web.auth.krb5.Krb5Authenticator">
        <loginModulePlugin>Trusting_LM</loginModulePlugin>
       </authenticationPlugin>
      </extension>
      <extension target="org.nuxeo.ecm.platform.ui.web.auth.service.PluggableAuthenticationService" point="chain">
       <authenticationChain>
        <plugins>
         <plugin>BASIC_AUTH</plugin>
         <plugin>KRB5_AUTH</plugin>
         <plugin>FORM_AUTH</plugin>
        </plugins>
       </authenticationChain>
      </extension>
      </component> 
    ```

    For now we assume all configuration - realm, kdc, server principal, etc. &minus; lives in the server's standard configuration, i.e. either `java.login.config` or `krb5.conf`. An interesting update would be to make these configurable in Nuxeo.

3.  Start Nuxeo.

### Configuring Client

On Windows:

*   If the client is IE or Chrome, no further configuration should be necessary. Jjust ensure your Nuxeo server is in the Local intranet or Trusted sites security zone.
*   If the client is Firefox, go to about:config, search for `network.negotiate-auth.trusted-uris` and set it to your full server URL.

On Linux, if the client is Firefox:

1.  Go to about:config.
2.  Search for `network.negotiate-auth.trusted-uris` and set it to your full server URL.
3.  Configure `krb5.conf` as above.
4.  Get a Kerberos ticket with `kinit`.

In the browser, type [http://nuxeo_server:8080/nuxeo](http://nuxeo_server:8080/nuxeo) and&hellip; you should be authenticated!

{{#> callout type='note' heading='URL'}}

Do not use localhost in the URL but the server's canonical name, that you mapped to the SPN.

{{/callout}}

### Note

The authenticator supports a magic request header to disable it. Simply set the `X-Skip-Kerberos` request header and Nuxeo will move on to the next filter on the list. This is useful if you want integrated authentication from within a corporate network but not from outside: simply setup two Apache virtual hosts with an "internal" URL and an "external" URL. In the external virtual host, add the following directive:

<pre>RequestHeader set X-Skip-Kerberos true</pre>

and this will move on to form authentication.

### Troubleshooting

If you have issues with the configuration of Kerberos in your environment, here is a few steps or tips that might help you finding the source of the issue.

Exception `Realm not local to kdc while getting initial credentials` while testing the kinit

*   The Real defined in the keytab file is probably incorrect. If you are using AD, by default, the real name is the DNS domain name (not the Netbios domain name) in upper case

Exception `Cannot create LoginContext, disabling Kerberos module` during startup of the Nuxeo Platform server

*   In the file java.login.config, verify that keytab file's path is properly set.
*   In the file java.login.config, verify that the principal set is correct.

Tips for troubleshooting :

*   It is possible to get the values of the principals present in the keytab file by executing the command kutil. More details on the [Oracle documentation](http://docs.oracle.com/cd/E19683-01/806-4078/6jd6cjs1q/index.html).
*   To enable the debug mode in the logs for Kerberos, edit the nuxeo.conf file to add `-Dsun.security.krb5.debug=true`

More troubleshooting tips on the [Oracle documentation](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jgss/tutorials/Troubleshooting.html).