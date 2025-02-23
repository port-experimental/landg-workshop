---
title: Bonus tasks
nav: true
---

# Bonus tasks

## Add Approvers to a SSA Action

Let's imagine that when adding a new Firewall Rule, this action must be approved by someone who is on approver duty, that should happen dynamically.
First thing you have to do is to invite other members in your organisation, explore the UI to find how to invite other members, you can invite `landg.workshop+userX@getport.io` where `X` is a number. There are 20 users available. 

The second step is to add a new property `isOnApproverDuty` to the `User` inbuild blueprint.

Now you can build your dyanmic approver flow : use this [documentation page](https://docs.port.io/actions-and-automations/create-self-service-experiences/set-self-service-actions-rbac/dynamic-permissions#team-leader-approval) as help and/or ask the intructor for help.

## Add TTL to Firewall Rules


Let's imagine that adding a new Firewall Rule is only temporary, it will have a TTL. Add a new property to express this, you will also need to add a new automation. Use this [example](https://docs.port.io/actions-and-automations/define-automations/examples#terminate-an-ephemeral-environment-when-its-ttl-is-expired) for inspiration. 






