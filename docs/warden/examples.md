---
title: Warden - Examples
---

# Examples

!!! info

    If you're looking for ready examples or useful templates to start working from check out
    [the dedicated repository](https://github.com/Twentysix26/warden-rules).
    Contributions are welcome!

### Sending a message
The following rule is about `send-message`. This action can be used to send messages, which may include an embed, to a variety of destinations. It supports context variables for the destination (edit_message_id too) and the message content / embed.  
In this example you will notice that this action supports two formats: simple (a YAML list `[ ]`) and more complex (a YAML map `{ }`).  
Note that when using the more complex format whitespace can be finicky: it is recommended that you use a [YAML validator](https://codebeautify.org/yaml-validator) to preserve your sanity.

```yaml
rank: 1
name: message-test
event: on-message
if:
  - message-matches-any: [test send]
do:
  - send-message: [$channel_id, hello] # Sends a message to the channel in the context
  - send-message: [$user_id, hello] # Sends a message to the user who triggered the rule
  - send-message: [133917673317401111, hello] # Sends a message to the specified id (channel or user)
  - send-message: [general, hello] # Sends a message to the specified channel (channel name)
  - send-message: {id: $channel_id, title: "My embed", description: "A simple embed"} # Sends a simple embed in the context's channel
  # The following one sends a (very complex) embed and it's done just to show the syntax
  # Note that NOT all fields are mandatory: just use the fields you need
  - send-message:
      id: $channel_id
      content: message content # content of the message
      author_name: $user
      author_url: $user_avatar_url
      author_icon_url: $user_avatar_url
      title: Have you ever wondered what happens if you use all the embed options?
      description: "Has science gone too far?" # description of the embed
      footer_text: "Made with Warden"
      footer_icon_url: $user_avatar_url
      image: $user_avatar_url
      add_timestamp: true # Add the current date / time to the embed
      thumbnail: $user_avatar_url
      url: $user_avatar_url
      color: 0x59FF00 # Use a HTML color picker for this, with due adjustments: #59FF00 -> 0x59FF00
      fields: [{name: Some, value: might}, {name: think, value: so, inline: false}] # These are the embed fields, you can add more than 2... Or fewer.
      # edit_message_id: 123456 # You can also edit a message by passing a valid message ID here. Empty values will be ignored.
      # reply_message_id: 123456 # Reply to a message (ID). Must be in the same channel the message is being sent in
      # ping_on_reply: true # Whether to ping on reply or not
```
### Notifying the staff
This rule shows how to use `notify-staff` and its many parameters  
```yaml
rank: 1
name: notify-test
event: on-message
if:
  - message-matches-any: [test notify]
do:
  - notify-staff: "hello" # A basic, text-only, message in the staff channel

# An elaborate staff notification. It will send an embed containing the title + description, 
# a handy link to instantly jump to the message, support for quick reactions
# and additional fields with information about the user / the channel 
  - notify-staff:
      title: "Warning!"
      content: "A user is spamming"
      jump_to_ctx_message: true
      qa_target: $user_id # Quick action target
      qa_reason: "Spamming messages" # Quick action reason, optional
      no_repeat_for: 30 seconds # Ensures that this notif. won't be sent again in the next 30 secs
      no_repeat_key: $rule_name-1 # An unique key that identifies this notif., make sure to set this too

# A notification that uses all the parameters for showing purposes. Only use the parameters you need
  - notify-staff:
      content: "A user is spamming"
      title: "Warning!"
      fields:
        - {name: "Field1", value: "Foo", inline: true}
        - {name: "Field2", value: "Bar", inline: true}
      add_ctx_fields: true
      thumbnail: $user_avatar_url
      footer_text: "my footer text"
      ping: true # Pings the staff role
      jump_to:
        channel_id: "$channel_id"
        message_id: "$message_id"
      # jump_to_ctx_message: true # This cannot be done if jump_to is used
      qa_target: $user_id
      qa_reason: Spamming
      no_repeat_for: 10 seconds
      no_repeat_key: $rule_name-2
      allow_everyone_ping: false
```
### A simple dehoister

This rule renames users who are attempting to hoist. Hoisting means prepending an exclamation mark to the username with the purpose of showing up at the top of the user list. This rule can also be manually run against every server member (`[p]def warden run`). Trusted users, staff and users who already have a nickname are ignored.
```yaml
rank: 2
name: dehoist
event: [on-user-join, manual]
if:
  - username-matches-any: ["!*"]
  - if-not:
      - nickname-matches-any: ["*"]
do:
  - set-user-nickname: "no hoisting"
```
### Preventing attachments from new users
This rule prevents new users (rank 4) from sending attachments. Staff is also notified about the deletion and is being given precise context about where that happened.
```yaml
rank: 4
name: no-attachments-rank4
event: [on-message, on-message-edit]
if:
  - message-has-attachment: true
do:
  - delete-user-message:
  - send-message: [$channel_id, "$user_mention Sorry, you are not allowed to send attachments."]
  - notify-staff:
      title: "Attachment removed"
      content: "A new user attempted to send an attachment"
      jump_to_ctx_message: true
      add_ctx_fields: true
      no_repeat_for: 30 seconds
      no_repeat_key: $rule_name-1
```
### Countering mass mentions
This rule bans any user (except trusted users and staff) who mentions at least 15 different people.
```yaml
rank: 2
name: mention-ban-rank2
event: [on-message]
if:
  - message-contains-more-than-unique-mentions: 14
do:
  - ban-user-and-delete: 1
  - send-mod-log: "User banned for mentioning too many people."
```
### Taking action after multiple infractions
The following two rules take advantage of the [heat level](/defender-docs/warden/overview#heat-level) system to kick a user after 3 infractions. Notice the `priority` parameter in the first rule.  
```yaml
rank: 2
name: bad-word
priority: 1
event: on-message
if:
  - message-matches-any: ["*b?d w?rd*"]
do:
  - delete-user-message:
  - send-message: [$channel_id, "No bad word here!"]
  - add-user-heatpoint: 1h
```

```yaml
rank: 2
name: check-heat
event: on-message
if:
  - user-heat-is: 3
do:
  - kick-user:
```