---
tags: [developers, onboarding, operators, protocol manager, users]
sidebar_position: 10
sidebar_label: Rules Overview Tab
sidebar_class_name: green
---

    This excerpt is redacted and renamed to protect client confidentiality. Screenshots are placeholders.



# Rules Overview Tab

The **Rules Overview** Tab in the Rules Manager gives a high-level view of all existing rules. From here, you can manage individual rules, assign them to priority groups, and configure whitelist rules.

## Accessing the Rules Manager

To access the Rules Manager, navigate to the following path in Platform:

`Platform > Sidebar Product Name > Rules Manager`.

The **Rules Overview** tab opens by default, showing all the rules available:

![Rules Manager Overview](/img/rule-manager/rule-overview-tab.png)

## Main Components

1. **Search**: Locate a rule by name or ID. Type the value in the search bar and press **[Enter]** to find the rule.
2. **Status Filter**: Filter rules based on their current [status](#rule-status-values) (e.g., active, paused). Open the dropdown, select the desired status values, and click **[Apply]** to filter rules.
3. **Create New Group**: Click **[New Group]** to create a new priority group. Enter the group name, select priority, and click **[Add New Group]**:

    ![Create New Group](/img/rule-manager/create-new-group.png)

    You can also create a new group by clicking **[Add New Group]** in the **Priority Groups** section below all rules.

4. **[Create New Rule](../path/internal link removed.md)**: Click **[New Rule]** to open the wizard and create a new rule. You can also click **[New Rule]** inside the **Priority Groups** section to create a new rule within a specific group when it is empty.
5. **[Whitelist Rules](#whitelist-rules)**: Manage non-prioritized rules that run alongside prioritized ones.
6. **[Priority Groups](#priority-groups-and-prioritization)**: Organize rules into groups that run in order of priority.

### Rule Status Values

Rules can have the following status values:

- **Draft**: The rule is in the editing phase and not yet active. You can save rules as drafts on any step to continue working on them later.
- **Active**: The rule is running.
- **Scheduled**: The rule is set to run at a specific time.
- **Failed**: The rule failed on start and did not run.
- **Paused**: The rule is temporarily stopped and not running. No new players can be assigned to it, and only players who are already assigned can continue progressing through the current flow. However, their progress will be ignored for new assignments and rewards.
  - If the rule is **resumed** before the flow duration ends, players can complete the flow and receive rewards.
  - If players do not complete the flow before the duration ends while the rule is paused, they will be **kicked** from the rule and won’t receive the remaining rewards.

  :::note
  
  There are cache settings for rules and journeys. This is why pausing or resuming a rule is applied with a delay about *N* seconds.

  Players who log in during this delay period will not be able to immediately participate or stop participating in rules.

  After the cache expires, players will be able to participate in rules as usual.
  
  :::

- **%something% Deploying**: The rule is being deployed with %some% actions.
- **Disabled**: The rule is disabled and not running.
- **Completed**: The rule has run successfully. This status is set when all rule **flows** are in *disabled* status.
- **Archived**: The rule is no longer in use and has been archived. You can archive *failed*, *disabled*, or *completed* rules.

## Whitelist Rules

Whitelist rules are non-priority rules that run independently of the prioritization hierarchy. They are always active and do not conflict with other rules. Triggers in whitelist rules are consistently evaluated without waiting time, although actions within them may have cooldowns to control frequency.

These rules are ideal for background processes, such as monitoring player behavior or running maintenance tasks.

## Priority Groups and Prioritization

In the Rules Manager, prioritization is structured across three levels:

1. Rule Group Priority (Level 1)
2. Rule Priority within a Rule Group (Level 2)
3. Flow Priority within a Rule (Level 3)

This multi-level prioritization ensures that rules run in the correct sequence based on business goals, player segments, and flow-specific configurations.

See [Rules Prioritization](./internal-link-removed.md) for details.

### Priority Group Actions

Click the kebab menu ![kebab](/img/rule-manager/kebab.png) on the right side of the group name to perform the following actions:

![Priority Group Actions](/img/rule-manager/group-actions.png)

- **Rename**: Change the group name.
- **Change Priority**: Change the priority of the group (move the group up or down):

    ![change group priority](/img/rule-manager/change-group-priority.gif)

- **Delete**: Remove the group.

## Rule Actions

From the **Rules Overview** tab, you can hover over the rule to access the following actions:

![Rule Actions](/img/rule-manager/rule-actions.png)

- **View**: Open the rule wizard to view the rule settings, flows, and conditions. You can also double-click the rule to view it.
- **Edit**: Open the rule wizard to modify the rule settings, flows, and conditions. You can also double-click the rule to edit it.
- **Clone**: Create a copy of the rule and open the wizard to make changes. The name will be suffixed with "# clone".
- **Priority**: Assign the rule to a different priority within priority groups.
- **Launch**: Start the rule immediately.
- **Disable**: Temporarily stop the rule from running.
- **Pause**: Temporarily stop the rule and prevent new players from entering it.
- **Resume**: Restart a paused rule.

All actions except **View** and **Edit** require confirmation.
