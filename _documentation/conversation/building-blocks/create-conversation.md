---
title: Create a Conversation
---

# Create a Conversation

Intro

## Example

Ensure the following variables are set to your required values using any convenient method:

Key | Description
-- | --
`NEXMO_APPLICATION_ID` | The ID of the application that you created.
`FROM_NUMBER` | The phone number you are sending the message from.
`TO_NUMBER` | The phone number you are sending the message to.
`FB_SENDER_ID` | Your Page ID. The `FB_SENDER_ID` is the same as the `to.id` value you received in the inbound messenger event on your Inbound Message Webhook URL.
`FB_RECIPIENT_ID` | The PSID of the user you want to reply to. The `FB_RECIPIENT_ID` is the PSID of the Facebook User you are messaging. This value is the `from.id` value you received in the inbound messenger event on your Inbound Message Webhook URL.

> **NOTE:** Don't use a leading `+` or `00` when entering a phone number, start with the country code, for example 447700900000.

```building_blocks
source: '_examples/dispatch/send-facebook-message-with-failover'
application:
  type: dispatch
  name: 'Send a message with failover'
```

## Creating a Conversation

Conversations can be created via the REST API, using one of the SDKs, or on the command line.

To create a Conversation via the command line:

``` shell
$ nexmo conversation:create display_name="Nexmo Chat"
```

This returns a unique ID for the Conversation:

```
CON-35663e9d-687f-4e5c-bd37-91837294bd76
```


## Try it out

...
