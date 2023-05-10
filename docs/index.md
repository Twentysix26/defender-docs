---
title: Overview
---

# Overview

Welcome to the documentation of [Defender](https://github.com/Twentysix26/x26-Cogs/).  
Defender is a big and complex cog but I promise that if you hang in there you could *just* make it ;)  
It is recommend that you first read this page and then head to the [configuration guide](/defender-docs/configuration).

## What is Defender

Defender is what could best be defined as a suite of tools aimed primarily at rendering your Discord community safe and secure.  
Functionalities are splits in modules:  

- `Auto modules`, which are automated moderation and monitoring functionalities
- `Manual modules`, which are tools that you can use in time of need

Each module is *optional*, meaning that you can decide to only enable specific parts of Defender.  
Despite being designed to counter serious threats Defender can also be a very valuable tool for smaller communities with less traffic.

``` mermaid
graph TD
  A[Auto modules];
  A -.- B[(Invite filter)]
  A -.- C[(Join monitor)];
  A -.- D[(Raider detection)];
  A -.- E[(Comment analysis)];
  A -.- F[(Warden)];
  G[Manual modules];
  G -.- H[(Alert)]
  G -.- I[(Vaporize)];
  G -.- J[(Silence)];
  G -.- K[(Voteout)];
  D -.- G

  linkStyle 9 stroke-width:0px;
```

=== "Auto modules"
    | Module      | Description                          |
    | ----------- | ------------------------------------ |
    | Invite filter       | Detects and takes action on unwanted Discord invites posted in your community |
    | Join monitor       | Detects surges of users joining your server and notifies the staff about newly created accounts. It can also raise your server's verification level if it detects a raid. |
    | Raider detection    | Also commonly referred as antispam / antiraid, it takes action on users spamming messages. |
    | Comment analysis    | Leverages the power of machine learning to detect a wide range of potentially unwanted messages. Powered by Google's Perspective API. |
    | Warden    | A complex module that lets you define *rules*. Rules are sets of *conditions* and *actions* that you can define to automate moderation, monitoring and much more. If something isn't covered by the other modules, it can probably be done with Warden.|

=== "Manual modules"
    | Module      | Description                          |
    | ----------- | ------------------------------------ |
    | Alert       | Allows your helper roles to report threats to the staff. Upon "ringing the bells" the staff is privately pinged with detailed context as to where the emergency is taking place. Additionally, this module can be configured so that after a certain time with no staff activity the server enters a state of emergency and certain modules (such as voteout) are rendered available to helpers. |
    | Vaporize       | Particularly effective against raids. It provides a quick way to ban vast amounts of new users without creating new mod-log entries. |
    | Silence    | Upon activation, it is able to instantly delete messages from certain ranks. This is particularly useful when a raid is in progress. |
    | Voteout    | Starts a voting session to expel a user. This module was designed specifically for emergency mode so that when the staff is absent helper roles are still able to take care of things by themselves. |

All these modules deliver notifications to your designed Defender *notification channel*.  
For more information about how to effectively set up your server for Defender, see the [configuration guide](/defender-docs/configuration).

## Ranks

Defender categorizes your userbase into different ranks. They range from **Rank 1**, your most trusted users until **Rank 4**, new users with little to no activity in your server.  
Thanks to this system, you are able to set up each different module so that they only target people below a certain rank, meaning that your regular users will be safe from the many safety measures that Defender offers.  
A regular user will only be able to rank up until Rank 2. Rank 1 is considered a special rank that can only be attained through designed roles or by being a staff member.

``` mermaid
graph BT
  A[/Rank 4\] --> | Time since join or messages sent | B[/Rank 3\];
  B --> | Time since join | C[/Rank 2\]
  C -.-> | Special role or staff | D[/Rank 1\];
```

  | Rank      | Description                          |
  | ----------- | ------------------------------------ |
  | 1    | Staff, trusted roles and helper roles |
  | 2    | Regular users who don't fit in any of the lower ranks |
  | 3    | Users who joined less than X days ago. Configurable. |
  | 4    | Users who joined less than X days ago and have sent fewer than Y messages. Configurable. |

!!! tip

    Want to know which Rank a particular user is? Use `[p]def identify <user>`

## Warden checks

:sparkles: *New in v2.0* :sparkles:

!!! note

    Warden checks require a basic understanding of the Warden automodule. If you feel lost, it might be better to [read its docs](warden/overview) first.

Warden checks are a powerful feature that allows users to set custom [Warden](warden/overview) conditions to further pinpoint the targets of standard modules. With Warden checks, users can specify their own sets of conditions to ensure that a module only targets certain users, or only certain users under a specific set of conditions.  
For example, a user may want to restrict the Invite Filter automodule to only work in channels A and B or to target users with roles Y and Z. By using Warden checks, the user can define the necessary conditions to achieve this.
Warden checks are evaluated after all other standard checks have been performed, ensuring that they have the final say in determining whether a user should be targeted. They can consist of any combination of conditions and condition blocks, giving users greater flexibility and control over how each module operates.  
To better illustrate this, let's see in simple terms how the Invite Filter automodule would work:

``` mermaid
graph TB
  A[A message containing<br>an invite has been sent] <--> B{Is the message's<br>author staff?};
  B --> | No | C{Is the message's<br>author below rank X?}
  C --> | Yes | D{Are the custom<br>Warden checks satisfied?}
  D --> | Yes | E[Take action on<br>the author]
  F[Do nothing]
  B --> | Yes | F
  C --> | No | F
  D --> | No | F
  style D fill:#ff8166,stroke:#f66,stroke-width:2px,color:#fff,stroke-dasharray: 5 5
```

As you can see, Warden checks are the last thing that gets evaluated before taking action on the user. And they can be any combination of [conditions](warden/statements/#conditions) and [condition blocks](warden/overview/#defining-a-more-complex-rule). A few examples:

```yaml
- channel-matches-any: [general] # Only work in the general channel
```

```yaml
- channel-is-public: true # Only work in public channels
```

```yaml
- if-not: # Users with the spammer role can spam everywhere
  - user-has-any-role-in: [spammer]
- if-not: # Users can only spam in the spam channel
  - channel-matches-any: [spam]
```

Warden checks can be set in the settings subcommands of each module  
e.g.
```yaml
### To set them ###
[p]dset invitefilter wdchecks
- channel-is-public: true
### Display them ###
[p]dset invitefilter wdchecks
### Remove them ###
[p]dset invitefilter wdchecks remove
```