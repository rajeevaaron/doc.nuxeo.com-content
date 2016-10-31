---
title: Managing Topics
review:
    comment: ''
    date: ''
    status: ok
labels:
    - topic
toc: true
confluence:
    ajs-parent-page-id: '16092673'
    ajs-parent-page-title: Forums
    ajs-space-key: USERDOC58
    ajs-space-name: Nuxeo Platform User Documentation - 5.8
    canonical: Managing+Topics
    canonical_source: 'https://doc.nuxeo.com/display/USERDOC58/Managing+Topics'
    page_id: '16092671'
    shortlink: '-431'
    shortlink_source: 'https://doc.nuxeo.com/x/-431'
    source_link: /display/USERDOC58/Managing+Topics
history:
    - 
        author: Solen Guitter
        date: '2016-09-01 14:11'
        message: ''
        version: '12'
    - 
        author: Solen Guitter
        date: '2013-10-22 18:17'
        message: ''
        version: '11'
    - 
        author: Solen Guitter
        date: '2013-09-30 17:11'
        message: ''
        version: '10'
    - 
        author: Solen Guitter
        date: '2013-01-18 17:01'
        message: ''
        version: '9'
    - 
        author: Solen Guitter
        date: '2011-11-24 10:33'
        message: Migrated to Confluence 4.0
        version: '8'
    - 
        author: Solen Guitter
        date: '2011-11-24 10:33'
        message: Added related content
        version: '7'
    - 
        author: Solen Guitter
        date: '2011-11-24 10:30'
        message: Added toc
        version: '6'
    - 
        author: Solen Guitter
        date: '2011-11-09 09:48'
        message: ''
        version: '5'
    - 
        author: Solen Guitter
        date: '2011-06-06 14:47'
        message: ''
        version: '4'
    - 
        author: Solen Guitter
        date: '2010-12-01 11:24'
        message: ''
        version: '3'
    - 
        author: Solen Guitter
        date: '2010-05-10 18:43'
        message: fixed broken links
        version: '2'
    - 
        author: Solen Guitter
        date: '2010-04-27 15:26'
        message: ''
        version: '1'

---
## Adding a Topic

**To add a new topic in a forum:**

1.  In the **Forum** tab of the forum, click on the **New Topic** button.
2.  Type the topic's title and optionally add a description.
3.  Select if the topic is moderated or not. If yes, search and select the moderators.
4.  Click on the **Create** button.
    The **Topic** tab of the topic is displayed, with the form to add a first comment on the topic.
    ![]({{file name='topic-created.png'}} ?w=650,border=true)

The list of the topics available in a forum is displayed in a table in the **Forum** tab.

![]({{file name='forum-topics-list.png'}} ?w=650,border=true)

## Moderating a Topic{{> anchor 'topic-moderation'}}

When a user creates a topic, he or she decides if the topic is moderated or not. Moderation is a process that makes comments available to moderators only when they are created, until they approve or reject the pending comments. Approval is thus mandatory to make comments available for other forum users.

When a user creates a moderated topic, he appoints users to manage comments on the topic. Only these moderators can approve or reject pending comments.

Moderators can see if there are comments pending in the forum tab. The number of comments waiting for approval is indicated for each topic of the forum. They also have a moderation task displayed in their [dashboard]({{page page='dashboard'}}), in their My tasks gadget.

![]({{file name='forum-content-table.png'}} ?w=650,border=true)

### Approving a Comment

Approving a comment means to publish it in the thread and make it available for all forum users.

**To approve a comment:**

1.  Open the topic that has pending comments.
    The pending comments have the status "Waiting for approval".
    ![]({{file name='forum-moderation-comment-waiting.png'}} ?w=650,border=true)
2.  Click on the **Approve** link in the top right corner of the pending comment.
    The comment's status is "Published". It is now available to all forum readers.
    ![]({{file name='forum-moderation-comment-approved.png'}} ?w=650,border=true)

### Rejecting a Comment

Rejecting a comment means that you make the comment permanently unavailable for forum users.

**To reject a comment:**

1.  Open the topic that has pending comments.
    The pending comments have the status "Waiting for approval".
    ![]({{file name='forum-moderation-comment-waiting2.png'}} ?w=650,border=true)
2.  Click on the **Reject** link in the top right corner of the pending comment.
    The comment's status is "Rejected". It is now permanently unavailable.
    ![]({{file name='forum-moderation-comment-rejected.png'}} ?w=650,border=true)

## Deleting a Comment

Only the comment's author and the topic moderators can delete comments.

**To delete a comment in a topic:**

1.  Open the topic.
2.  Click on the **Delete** link located in the top right corner of the comment to delete.
    The comment is immediately and permanently deleted.

## Deleting a Topic

Deleting a topic means deleting its content as well.

When you delete a topic, it is definitively erased from the application.

**To delete a topic:**

1.  In the **Forum** tab of the forum, select the topic you want to delete by checking the corresponding box.
2.  Click on the **Delete** button.
    A confirmation window pops up.
3.  Click on the **OK** button.
    The topic is moved to the forum's trash. Users can then [restore the topic]({{page page='managing-deleted-documents#restore-documents'}}) into the forum or [erase]({{page page='managing-deleted-documents#permanently-delete-documents'}}) the same way as a document in a workspace.

&nbsp;