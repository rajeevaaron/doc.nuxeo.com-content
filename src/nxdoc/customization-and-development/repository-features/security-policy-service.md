---
title: Security Policy Service
labels:
    - security-policy
toc: true
confluence:
    ajs-parent-page-id: '17334376'
    ajs-parent-page-title: Repository features
    ajs-space-key: NXDOC58
    ajs-space-name: 'Nuxeo Platform Developer Documentation - 5.8 '
    canonical: Security+Policy+Service
    canonical_source: 'https://doc.nuxeo.com/display/NXDOC58/Security+Policy+Service'
    page_id: '17334473'
    shortlink: yYAIAQ
    shortlink_source: 'https://doc.nuxeo.com/x/yYAIAQ'
    source_link: /display/NXDOC58/Security+Policy+Service
history:
    - 
        author: Solen Guitter
        date: '2014-02-27 11:12'
        message: dded note about unrestricted session
        version: '8'
    - 
        author: Solen Guitter
        date: '2013-11-13 15:53'
        message: Updated links to 5.8 release
        version: '7'
    - 
        author: Solen Guitter
        date: '2013-09-04 18:26'
        message: Formatting
        version: '6'
    - 
        author: Solen Guitter
        date: '2013-08-27 15:23'
        message: Added TOC
        version: '5'
    - 
        author: Florent Guillaume
        date: '2013-08-13 12:09'
        message: adding cmisql
        version: '4'
    - 
        author: Solen Guitter
        date: '2012-09-11 22:25'
        message: Migrated to Confluence 4.0
        version: '3'
    - 
        author: Solen Guitter
        date: '2012-09-11 22:25'
        message: ''
        version: '2'
    - 
        author: Florent Guillaume
        date: '2010-12-28 17:15'
        message: ''
        version: '1'

---
### Document Security Check

To check security on a given document, Nuxeo Core calls `checkPermission` with a number of parameters, among which the document, the user and the permission to check, on all the security policies registered. The policies should return `Access.DENY` or `Access.UNKNOWN` based on the information provided. If `Access.DENY` is returned, then access to the document (for the given permission) will be denied. If `Access.UNKNOWN` is returned, then other policies will be checked. Finally if all policies return `Access.UNKNOWN` then standard Nuxeo EP ACLs will be checked.

There is a third possible value, `Access.GRANT`, which can immediately grant access for the given permission, but this bypasses ACL checks. This is all right for most permissions but not for the `Browse` permission, because `Browse` is used for NXQL and CMISQL searches in which case it's recommended to implement `getQueryTransformer` instead (see below).

Note that `checkPermission` receives a document which is a `org.nuxeo.ecm.core.model.Document` instance, different from and at a lower level than the usual `org.nuxeo.ecm.core.api.DocumentModel` manipulated by user code.

{{#> callout type='info' heading='Unrestricted sessions'}}

If the query has been called in the context of an unrestricted session, the principal will be `system`. It is a good practice to check for that username since if the query is run unrestrictedly, it functionally means that you should not restrict anything with the query transformer

{{/callout}}

### NXQL Security Check

All NXQL queries have ACL-based security automatically applied with the `Browser` permission (except for superusers).

A security policy can modify this behavior but only by adding new restrictions in addition to the ACLs. To do so, it can simply implement the `checkPermission` described above, but this gets very costly for big searches. The efficient approach is to make `isExpressibleInQuery` return `true` and implement `getQueryTransformer`.

The `getQueryTransformer(repositoryName)`&nbsp;method returns a `SQLQuery.Transformer` instance, which is a class with one `transform` method taking a NXQL query in the form of a `org.nuxeo.ecm.core.query.sql.model.SQLQuery` abstract syntax tree. It should transform this tree in order to add whatever restrictions are needed. Note that ACL checks will always be applied after this transformation.

### CMISQL Security Check

Since Nuxeo 5.6.0-HF21 and Nuxeo 5.7.2, all CMISQL queries also require implementation of the relevant&nbsp; `getQueryTransformer`&nbsp;API in order to secure CMIS-based searches.

The&nbsp;`getQueryTransformer(repositoryName, "CMISQL")`&nbsp;method returns a&nbsp;`SecurityPolicy.QueryTransformer`&nbsp;instance, which is a class with one&nbsp;`transform`&nbsp;method taking a query in the form of `String`. It should transform this query in order to add whatever restrictions are needed (this will require parsing the CMISQL and adding whatever clauses are needed). Note that ACL checks will always be applied after this transformation.

## Example Security Policy Contribution

To register a security policy, you need to write a contribution specifying the class name of your implementation.

```
<?xml version="1.0"?>
<component name="com.example.myproject.securitypolicy">

  <extension target="org.nuxeo.ecm.core.security.SecurityService"
    point="policies">

    <policy name="myPolicy"
      class="com.example.myproject.NoFileSecurityPolicy" order="0" />

  </extension>

</component>

```

Here is a sample contributed class:

```
import org.nuxeo.ecm.core.query.sql.model.*;
import org.nuxeo.ecm.core.query.sql.NXQL;
import org.nuxeo.ecm.core.security.AbstractSecurityPolicy;
import org.nuxeo.ecm.core.security.SecurityPolicy;

/**
 * Sample policy that denies access to File objects.
 */
public class NoFileSecurityPolicy extends AbstractSecurityPolicy implements SecurityPolicy {

    @Override
    public Access checkPermission(Document doc, ACP mergedAcp,
            Principal principal, String permission,
            String[] resolvedPermissions, String[] additionalPrincipals) {
        // Note that doc is NOT a DocumentModel
        if (doc.getType().getName().equals("File")) {
            return Access.DENY;
        }
        return Access.UNKNOWN;
    }

    @Override
    public boolean isRestrictingPermission(String permission) {
        // could only restrict Browse permission, or others
        return true;
    }

    @Override
    public boolean isExpressibleInQuery() {
        return true;
    }

    @Override
    public SQLQuery.Transformer getQueryTransformer() {
        return NO_FILE_TRANSFORMER;
    }

    public static final Transformer NO_FILE_TRANSFORMER = new NoFileTransformer();

    /**
     * Sample Transformer that adds {@code AND ecm:primaryType <> 'File'} to the query.
     */
    public static class NoFileTransformer implements SQLQuery.Transformer {

        /** {@code ecm:primaryType <> 'File'} */
        public static final Predicate NO_FILE = new Predicate(
                new Reference(NXQL.ECM_PRIMARYTYPE), Operator.NOTEQ, new StringLiteral("File"));

        @Override
        public SQLQuery transform(Principal principal, SQLQuery query) {
            WhereClause where = query.where;
            Predicate predicate;
            if (where == null || where.predicate == null) {
                predicate = NO_FILE;
            } else {
                // adds an AND ecm:primaryType <> 'File' to the WHERE clause
                predicate = new Predicate(NO_FILE, Operator.AND, where.predicate);
            }
            // return query with updated WHERE clause
            return new SQLQuery(query.select, query.from, new WhereClause(predicate),
                    query.groupBy, query.having, query.orderBy, query.limit, query.offset);
        }
    }

}

```

### CMISQL Security Checks

To find examples of security policies using CMISQL query transformers, please check the [`TitleFilteringSecurityPolicy2`](https://github.com/nuxeo/nuxeo-chemistry/blob/release-5.8/nuxeo-opencmis-tests/src/test/java/org/nuxeo/ecm/core/opencmis/impl/TitleFilteringSecurityPolicy2.java) in the unit tests.