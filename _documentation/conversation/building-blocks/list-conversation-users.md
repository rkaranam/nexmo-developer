---
title: List Users in a Conversation
description: List Users in a Conversation
---

# List Users associated with a Conversation

You can also list Users associated with a Conversation via the command line:

``` shell
$ nexmo member:list CON-35663e9d-687f-4e5c-bd37-91837294bd76 -v
```

Returns something similar to:

``` shell
name  | user_id                                  | user_name | state  
----------------------------------------------------------------------
bob  | USR-fa1acfbb-0087-4fa5-911d-045586470edc | bob      | JOINED
alice | USR-cf0e4ff6-fd86-4d4a-9a90-282a59593480 | alice     | INVITED
```
