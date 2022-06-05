---
title: Warden - Actions / Conditions
---
# Actions / Conditions

## Conditions

### Message conditions

* `message-matches-any`  
Is `true` if any of the patterns that you list are found in the message's content. This kind of searching might be inconvenient in certain use cases: if you prefer to look for whole words instead see the condition `message-contains-word`  
**Accepts:** A list of patterns. Patterns can make use of wildcards such as `*` and `?`  
**Context:** `message`  
**Example:**  
```yaml
- message-matches-any: ["cat"] # This will only match exactly this word
# I like cats -> Is false
# cat -> Is true
# cats -> Is false
- message-matches-any: ["*cat*"] # The wildcard "*" represents any multiple characters
# I like cats -> Is true
# I like cat -> Is true
# I like c4t -> Is false
- message-matches-any: ["*c?t*"] # The wildcard "?" represents any single character
# I like cats -> Is true
# I like cat -> Is true
# I like c4t -> Is true
# xxxxcatxxxx -> Is true
```
* `message-matches-regex`  
Is `true` if the regex returns a match against the message's content  
**Accepts:** A string representing a regular expression. Remember that `\` must be escaped: turn all `\` characters to `\\` when entering the rule.  
**Context:** `message`  
* `message-has-attachment`  
Will check if the message contains an attachment  
**Accepts:** A bool (true or false)    
**Context:** `message`  
* `message-contains-word`  
Is `true` if any of the words you list are found in the message. It does what it is most commonly know "whole word search". Patterns are supported  
**Example:**  
```yaml
- message-contains-word: ["cat"]
# I like cats -> Is false
# I like cat -> Is true
# I like c4t -> Is false
# xxxxcatxxxx -> Is false
- message-contains-word: ["c?t"] # The wildcard "?" represents any single character
# I like cats -> Is false
# I like cat -> Is true
# I like c4t -> Is true
# xxxxcatxxxx -> Is false
```
* `message-contains-url`  
Will check if the message contains a clickable URL.  
Note: URLs lacking a protocol (http/https) will not be detected.  
**Accepts:** A bool (true or false)    
**Context:** `message`  
* `message-contains-invite`  
Will check if the message contains a standard Discord invite. Invites that belong to the server are ignored.  
**Accepts:** A bool (true or false)    
**Context:** `message`  
* `message-contains-media`  
Will check if the message contains any media link  (picture or video)  
**Accepts:** A bool (true or false)    
**Context:** `message`  
* `message-contains-more-than-mentions`  
Will check if the message contains more than X user mentions.  
**Accepts:** A number representing the number of mentions  
**Context:** `message`  
* `message-contains-more-than-unique-mentions`  
Will check if the message contains more than X unique user mentions. As opposed to the non-unique variant the mentioned users are being counted, *not* just the mentions themselves.  
**Accepts:** A number representing the number of unique mentions.  
**Context:** `message`  
* `message-contains-more-than-role-pings`  
Will check if the message contains more than X unique role mentions. Only mentions that result in a ping will be counted.  
**Accepts:** A number representing the number of role mentions  
**Context:** `message`  
* `message-contains-more-than-emojis`  
Will check if the message contains more than X emojis. **Important:** emojis with [modifiers](https://en.wikipedia.org/wiki/Emoticons_(Unicode_block)#Emoji_modifiers), such as a thumbs up emoji with custom skin color, will be considered 2 separate emojis.  
**Accepts:** A number representing the number of emojis.  
**Context:** `message`  
* `message-has-more-than-characters`  
Will check if the message contains more than X characters. Emojis and custom emojis will be considered a single character. Mentions, both users and channels, are counted too.  
**Accepts:** A number representing the number of characters.  
**Context:** `message`  

### User conditions

* `user-id-matches-any`  
Is `true` if any of the IDs that you list match the user's ID  
**Accepts:** A list of IDs (numbers)  
**Context:** `user`  
**Example:** `user-id-matches-any: [123456789, 2626262626]`  
* `username-matches-any`  
Is `true` if any of the patterns that you list are found in the user's username  
**Accepts:** A list of patterns. Patterns can make use of wildcards such as `*` and `?`  
**Context:** `user`  
* `username-matches-regex`  
Is `true` if the regex returns a match against the user's username  
**Accepts:** A string representing a regular expression. Remember that `\` must be escaped: turn all `\` characters to `\\` when entering the rule.  
**Context:** `user`  
* `nickname-matches-any`  
Is `true` if any of the patterns that you list are found in the user's nickname  
**Accepts:** A list of patterns. Patterns can make use of wildcards such as `*` and `?`  
**Context:** `user`  
* `nickname-matches-regex`  
Is `true` if the regex returns a match against the user's nickname  
**Accepts:** A string representing a regular expression. Remember that `\` must be escaped: turn all `\` characters to `\\` when entering the rule.  
**Context:** `user`  
* `user-activity-matches-any`  
Is `true` if any of the patterns that you list are found in any of the user's activities  
**Accepts:** A list of patterns. Patterns can make use of wildcards such as `*` and `?`  
**Context:** `user`  
* `user-status-matches-any`  
Is `true` if any of the statuses that you list are the user's current status  
**Accepts:** A list of statuses. Can be `online`, `idle`, `dnd` or `offline`  
**Context:** `user`  
* `user-created-less-than`  
Is `true` if the user's account has been created less than <defined time> ago.  
**Accepts:** A timedelta. Or a number, representing the amount of hours to be checked  
**Context:** `user`  
**Example:** `user-created-less-than: 2 hours`  
* `user-joined-less-than`  
Is `true` if the user has joined the server less than <defined time> ago.  
**Accepts:** A timedelta. Or a number, representing the amount of hours to be checked  
**Context:** `user`  
**Example:** `user-joined-less-than: 2 hours`  
* `user-has-default-avatar`  
Will check if the user has a default Discord avatar  
**Accepts:** A bool (true or false)  
**Context:** `user`  
**Example:** `user-has-default-avatar: true` will be `true` if the profile picture is a default one  
* `user-has-sent-less-than-messages`  
Will check if the user has sent less than X messages in the server. **Important:** Defender, by default, counts how many messages a user sends in a server. This does *not* check the real total number of messages a user has sent in a server since its creation.  
**Accepts:** A number representing the number of messages  
**Context:** `user`  
* `user-is-rank`  
Will check if the user is the specified rank. This condition allows for *rank-specific* Warden rules and grants more granular control compared to the standard `rank` parameter, which merely indicates the maximum rank a rule will have effect on.  
**Accepts:** A number representing the rank the user must belong to  
**Context:** `user`  

### Channel conditions

* `channel-matches-any`  
Will check if the message was sent in any of the specified channels.  
**Accepts:** A list of channel names or IDs  
**Context:** `message`  
**Example:** `channel-matches-any: [general, testing, 483223782]` will be `true` if the message was sent in any of these channels  
* `category-matches-any`  
Will check if the channel in which the message was sent belongs to any of the specified categories.  
**Accepts:** A list of category names or IDs  
**Context:** `message`  
* `channel-is-public`  
Will check if the message was sent in a publicly viewable channel.  
**Accepts:** A bool (true or false)    
**Context:** `message`  

### Server conditions

* `in-emergency-mode`  
Will check if the server is in emergency mode, either manual or automatic  
**Accepts:** A bool (true or false)    
**Context:** Any  

### Roles conditions

* `user-has-any-role-in`  
Is `true` if the user belongs to any of these roles in the list  
**Accepts:** A list of role names or IDs  
**Context:** `user`  
**Example:** `user-has-any-role-in: [12345678, spider-fighter]` will be `true` if the user belongs to any of these roles  
* `is-staff`  
Will check if the user is a staff member  
**Accepts:** A bool (true or false)    
**Context:** `user`  
* `is-helper`  
Will check if the user is a Defender helper  
**Accepts:** A bool (true or false)    
**Context:** `user`  

### Heat conditions

* `user-heat-is`  
Is `true` if the user has the specified [heat level](#heat-level)  
**Accepts:** A number between 0 and 100    
**Context:** `user`  
* `user-heat-more-than`  
Is `true` if the user exceeds the specified [heat level](#heat-level)  
**Accepts:** A number between 0 and 100    
**Context:** `user`  
* `channel-heat-is`  
Is `true` if the channel has the specified [heat level](#heat-level)  
**Accepts:** A number between 0 and 100    
**Context:** `message`  
* `channel-heat-more-than`  
Is `true` if the channel exceeds the specified [heat level](#heat-level)  
**Accepts:** A number between 0 and 100    
**Context:** `message`  
* `custom-heat-is`  
Is `true` if the [custom heat](/defender-docs/warden/overview#custom-heat-level) has the specified level  
**Accepts:** A list with two elements: the custom heat level name and a number between 0 and 100. Rule name and IDs [context variables](/defender-docs/warden/overview#context-variables) are available for the name.  
**Context:** `Any`  
* `custom-heat-more-than`  
Is `true` if the [custom heat](/defender-docs/warden/overview#custom-heat-level) exceeds the specified level  
**Accepts:** A list with two elements: the custom heat level name and a number between 0 and 100. Rule name and IDs [context variables](/defender-docs/warden/overview#context-variables) are available for the name.  
**Context:** `Any`

### Custom conditions

* `compare`  
Compares two values. Supports a variety of operators, textual and numeric. The comparison is case sensitive. This is most useful when used in conjunction with context variables. Using numeric operators with non-numeric values will raise an error.  
_Supported operators:_ `==`, `!=`, `contains`, `contains-pattern`, `>=`, `<=`, `<`, `>`  
`contains-pattern` supports a pattern, just like the condition `message-matches-any` and it's case insensitive.  
**Example:**  
```yaml
- compare: [abc, "==", abc] # True
- compare: [2, "<", 1] # False

- var-assign: [value1, "I like bots"]
- var-assign: [value2, "bots"]
- compare: [$value2, "contains", $value1] # True. Notice the $ symbol which denotes the use of context variables.
```
**Context:** `Any`  

## Actions

* `send-message`  
Will send a message to the defined destination. Message editing is also supported. The message may include an embed. Please refer to the [examples](/defender-docs/warden/examples#sending-a-message) to understand its use.  
**Accepts:** A map with the destination, the message's content and the embed. Or a list with 2 elements, destination and message content  
**Context:** `Any`  
* `set-user-nickname`  
Will change the nickname of a user  
**Accepts:** A string representing the new nickname to set. Supports context variables.  
**Context:** `user`  
* `delete-user-message`  
Will delete the user's message  
**Accepts:** Nothing    
**Context:** `message`  
* `add-roles-to-user`  
Will assign the listed roles to the user  
**Accepts:** A list of roles (names / IDs)  
**Context:** `user`  
* `remove-roles-from-user`  
Will remove the listed roles from the user  
**Accepts:** A list of roles (names / IDs)  
**Context:** `user`  
* `ban-user-and-delete`  
Will ban the user from the server and delete X days worth of messages  
**Accepts:** A number representing the days worth of messages to delete  
**Context:** `user`  
* `kick-user`  
Will kick the user from the server  
**Accepts:** Nothing  
**Context:** `user`  
* `softban-user`  
Will kick the user from the server and delete 1 days worth of messages  
**Accepts:** Nothing  
**Context:** `user`  
* `punish-user`  
Will assign to the user the "punish role" that has been set in Defender  
**Accepts:** Nothing  
**Context:** `user`  
* `punish-user-with-message`  
Will assign to the user the "punish role" and deliver the "punish message" that have been set in Defender (see `[p]dset general`)  
**Accepts:** Nothing  
**Context:** `message`  
* `notify-staff`  
Will send a message to staff's notification channel. The message may include an embed. Please refer to the [examples](/defender-docs/warden/examples#notifying-the-staff) to understand its use.    
**Accepts:** A string representing the message to send. Or a map that describes the embed and all the behaviors of the notification.  
**Context:** Any  
* `send-mod-log`  
Will create a new mod-log case of the last expel action issued by the rule  
**Accepts:** A string representing the reason of the expel action. Supports context variables.  
**Context:** Any  
* `set-channel-slowmode`  
Will set (or deactivate) slowmode of the channel in the context's message  
**Accepts:** A string representing the slowmode time. Some examples: '5 seconds', '2 minutes', '4 hours'. '0 seconds' will deactivate slowmode.   
**Context:** `message`  
* `enable-emergency-mode`  
Will toggle emergency mode  
**Accepts:** A bool (true or false), representing whether emergency mode should be enabled or disabled  
**Context:** Any  
* `send-to-monitor`  
Will send a message to Defender's monitor, `[p]defender monitor`  
**Accepts:** A string representing the message to send. Supports context variables.  
**Context:** Any  
* `get-user-info`  
Gets an attribute from an arbitrary user and assigns it to a variable. Many of the [standard discord.py Member attributes](https://discordpy.readthedocs.io/en/latest/api.html#discord.Member) are supported.  
Additionally, the attributes  
`is_staff` - `is_helper` with return value `true` or `false`  
`rank` which returns the rank's number  
`message_count` which returns the number of messages recorded by Defender  
are available.  
If the user cannot be found an error will be raised.   
**Accepts:** The ID of a user (context variables are supported) and a map with the attribute to fetch as the value and the target variable as the key.  
**Context:** Any  
**Example:**  
```yaml
- get-user-info: [$user_id, {joined: joined_at, created: created_at}]
- send-message: [$channel_id, "Your join date is $joined. Your account creation date is $created."]

# Long form
- get-user-info:
    id: 123456789
    mapping:
       joined: joined_at
       created: created_at
       staff: is_staff
       helper: is_helper
```
* `no-op`  
Does nothing. Useful only for testing a rule's conditions with `[p]defender warden run`  
**Accepts:** Nothing  
**Context:** Any  
* `exit`  
Interrupts the rule's processing, not carrying on any further action. This is useful when used in conjunction with conditional actions.  
**Accepts:** Nothing  
**Context:** Any  
* `add-user-heatpoint`  
Adds a single heat point with the specified lifetime to the user's [heat level](#heat-level)  
**Accepts:** A string representing the heat point's lifetime. Some examples: '5 seconds', '2 minutes', '4 hours'  
**Context:** `user`  
**Example:** `add-user-heatpoint: 5 seconds` `add-user-heatpoint: 1m`  
* `add-user-heatpoints`  
Adds multiple heat points with the specified lifetime to the user's [heat level](#heat-level)  
**Accepts:** A list containing two elements: the amount of heatpoints to assign (1-100) and a string representing the heat points' lifetime. Some examples: '5 seconds', '2 minutes', '4 hours'  
**Context:** `user`  
**Example:** `add-user-heatpoints: [10, 5 seconds]` `add-user-heatpoints: [2, 1m]`  
* `add-channel-heatpoint`  
Adds a single heat point with the specified lifetime to the channel's [heat level](#heat-level)  
**Accepts:** A string representing the heat point's lifetime. Some examples: '5 seconds', '2 minutes', '4 hours'  
**Context:** `message`  
**Example:** `add-channel-heatpoint: 5 seconds` `add-channel-heatpoint: 1m`  
* `add-channel-heatpoints`  
Adds multiple heat points with the specified lifetime to the channel's [heat level](#heat-level)  
**Accepts:** A list containing two elements: the amount of heatpoints to assign (1-100) and a string representing the heat points' lifetime. Some examples: '5 seconds', '2 minutes', '4 hours'  
**Context:** `message`  
**Example:** `add-channel-heatpoints: [10, 5 seconds]` `add-channel-heatpoints: [2, 1m]`  
* `add-custom-heatpoint`  
Adds a single heat point with the specified lifetime to the [custom heat level](/defender-docs/warden/overview#custom-heat-level)  
**Accepts:** A list containing two elements: a string representing the custom heat level name and a string representing the heat point's lifetime. Some examples: '5 seconds', '2 minutes', '4 hours'. Rule name and IDs [context variables](/defender-docs/warden/overview#context-variables) are available for the name.  
**Context:** `Any`  
**Example:** `add-custom-heatpoint: ["filter-$user_id", "5 seconds"]`  
* `add-custom-heatpoints`  
Adds multiple heat points with the specified lifetime to the [custom heat level](/defender-docs/warden/overview#custom-heat-level)  
**Accepts:** A list containing three elements: a string representing the custom heat level name, a number representing the amount of heat points to add and a string representing the heat points lifetime. Some examples: '5 seconds', '2 minutes', '4 hours'. Rule name and IDs [context variables](/defender-docs/warden/overview#context-variables) are available for the name.  
**Context:** `Any`  
**Example:** `add-user-heatpoints: ["$rule_name", 10, 5 seconds]`  
* `empty-user-heat`  
Sets the user's [heat level](#heat-level) to 0  
**Accepts:** Nothing  
**Context:** `user`  
* `empty-channel-heat`  
Sets the channel's [heat level](#heat-level) to 0  
**Accepts:** Nothing  
**Context:** `message`  
* `empty-custom-heat`  
Sets the specified [custom heat level](/defender-docs/warden/overview#custom-heat-level) to 0  
**Accepts:** A string representing the [custom heat level](/defender-docs/warden/overview#custom-heat-level) name. Rule name and IDs [context variables](/defender-docs/warden/overview#context-variables) are available for the name.  
**Context:** `Any`  
* `issue-command`  
Will issue a bot command as if it was issued by the specified user. The specified user must be the rule's author. If no destination is specified issuing a command in a `message` context will make it issue in the same channel as the message's; in any other case the command will be issued in Defender's notification channel.  
**Accepts:** A list with two elements: a number representing the ID of the rule's author and a string representing the command to issue. The command must not contain the prefix. [Context variables](/defender-docs/warden/overview#context-variables) are supported. The long form of this action also supports a destination.  
**Context:** `Any`  
**Example:**  
```yaml
- issue-command: [2626262626, "ban $user_id User has been banned by Warden's rule $rule_name"]
- issue-command:
   issue_as: 2626262626 # User ID
   command: ping # Command
   destination: 123456 # Channel ID
```
* `delete-last-message-sent-after`  
Will delete the last message sent through a Warden action after the specified time. The time must be between 1 and 15 minutes.  
**Accepts:** A string representing the time to wait before deleting the message  
**Context:** `Any`  
**Example:** `send-message: [$channel_id, I'm going to disappear in 5 seconds!]`  
             `delete-last-message-sent-after: 5 seconds`  
### Variables related actions  
* `var-assign`  
Assigns a value to a variable.  
```yaml
- var-assign: [my_var, 123]
- var-assign:
    var_name: my_var
    value: 123
    evaluate: false # (optional, if true any context variable contained in value will be evaluated)
# The variable my_var will be equal to 123
```
* `var-assign-random`  
Assigns a random value to a variable, picking one from a list of choices. Supports weighted choices  
```yaml
# A simple, non-weighted choice
- var-assign-random: [my_var, [apple, banana, pear]]
- var-assign-random:
    var_name: my_var
    choices: [apple, banana, pear]
    evaluate: false # (optional, if true any context variable contained in value will be evaluated)
# A weighted choice
- var-assign-random:
    var_name: my_var
    choices:
       apple: 10
       banana: 1
       pear: 2
    evaluate: false # (optional, if true any context variable contained in value will be evaluated)
```
* `var-split`  
Splits a variable into parts, using separator as the delimiter, and assigns the parts to different variables. If there are more parts than provided variables, the remaining parts will be discarded. If there are fewer parts than provided variables, an empty string will be assigned to the remaining variables. Maximum splits can be specified, by default there is no limit. 
```yaml
- var-assign: [fruit, "apple pear banana tomato"]

# fruit1 will contain "apple", fruit2 "pear", etc. Splits are done left to right.
- var-split: [fruit, " ", [fruit1, fruit2, fruit3, fruit4]]

# As before fruit1 will contain "apple", fruit2 "pear" but the remaining parts will be lost
- var-split: [fruit, " ", [fruit1, fruit2]]

# A max_split of 1 is specified, fruit1 will still be "apple", but fruit2 will be "pear banana tomato"
- var-split: [fruit, " ", [fruit1, fruit2], 1]
- compare: ["$fruit2", "==", "pear banana tomato"] # True!

# Same as before, but fruit3 will be an empty string: only one split has been done, and there's nothing left to assign to fruit3.
- var-split: [fruit, " ", [fruit1, fruit2, fruit3], 1]
- compare: ["$fruit1", "==", "apple"] # True!
- compare: ["$fruit3", "==", ""] # True!

# Long form:
- var-split:
    var_name: fruit
    separator: " "
    split_into: [fruit1, fruit2, fruit3]
    max_split: 1 # (optional)
```
* `var-slice`  
Slices a variable according to the indices (the position of the characters) that are being passed. If a variable name is passed with slice_into the original variable will be left untouched. The optional parameter step, found in Python's string slicing, can also be passed. Remember than indices start from 0, not 1.
```yaml
- var-assign: [my_var, "abcdefgh"]
- var-slice: [my_var, 0, 2, sliced]
- compare: ["$sliced", "==", "ab"] # True!

- var-slice: [my_var, 0, 4]
- compare: ["$my_var", "==", "abcd"] # True!

- var-assign: [my_var, "abcdefgh"]
# Long form
- var-slice:
    var_name: my_var
    index: 0
    end_index: 99
    slice_into: sliced
    step: 2
- compare: ["$sliced", "==", "aceg"] # True!
```
* `var-replace`  
Modifies a variable with all the occurrences of strings replaced by substring. Strings can be a single value or a list of values.
```yaml
- var-assign: [my_var, "I like apples a lot"]
- var-replace: [my_var, a, 4]
- compare: ["$sliced", "==", "I like 4pples 4 lot"] # True!

- var-assign: [my_var, "I like apples a lot"]
- var-replace: [my_var, [a, p], x]
- compare: ["$sliced", "==", "I like xxxles x lot"] # True!

# Long form
- var-replace:
    var_name: my_var
    strings: a # Can also be a list, as shown above
    substring: b
```
* `var-transform`  
Modifies a variable depending on the "operation" that it's being requested. Supported operations are capitalize, lowercase, reverse, uppercase, title.
```yaml
- var-assign: [my_var, "I like apples A LOT"]
- var-transform: [my_var, lowercase]
- compare: ["$my_var", "==", "i like apples a lot"] # True!

- var-transform: [my_var, uppercase]
- compare: ["$my_var", "==", "I LIKE APPLES A LOT"] # True!

- var-assign: [my_var, "I like apples a lot"]
- var-transform: [my_var, title] # Capitalizes the first letter of each word
- compare: ["$my_var", "==", "I Like Apples A Lot"] # True!

- var-assign: [my_var, "two words"]
- var-transform: [my_var, capitalize] # Only capitalizes the first letter
- compare: ["$my_var", "==", "Two words"] # True! 

# Long form
- var-transform:
    var_name: my_var
    operation: uppercase
```