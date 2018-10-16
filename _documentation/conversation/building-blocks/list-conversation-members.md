---
title: List Conversation Members
description: List Conversation Members.
---

# List Members of a Conversation

You can list the Members of a Conversation with the command:

``` shell
$ nexmo member:list CON-35663e9d-687f-4e5c-bd37-91837294bd76 -v
```

This returns output similar to the following:

``` shell
name  | user_id                                  | user_name | state
----------------------------------------------------------------------
pete  | USR-fa1acfbb-0087-4fa5-911d-045586470edc | pete      | JOINED
alice | USR-cf0e4ff6-fd86-4d4a-9a90-282a59593480 | alice     | INVITED
bobby | USR-6a8cb033-e8fc-482f-8404-63085ab984d6 | bobby     | LEFT
bobby | USR-6a8cb033-e8fc-482f-8404-63085ab984d6 | bobby     | JOINED
```

The will also show the state of the Member such as `JOINED`, `INVITED`, `LEFT` and so on.

You can also use the REST API to get a list of Members:

``` shell
$ curl "https://api.nexmo.com/beta/conversations/CON-35663e9d-687f-4e5c-bd37-91837294bd76/members" \
     -H 'Authorization: Bearer eyJ0eXAiOiJKV1Q....YdVEyolrHqSKQ' \
     -H 'Content-Type: application/json'
```
