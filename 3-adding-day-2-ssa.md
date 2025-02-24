---
title: Adding Day-2 Self-Service Actions
nav: true
---

# Adding Day-2 Self-Service Actions

Until now we have only created the `Create` action. We need also to offer SSAs to modify anexisting FW rule and also be able to delete one.

## Day-2

Here you can reuse most of the `Create` Action, the http method of the Webhook will need to be change to `PUT`.

## Delete

This one will probably contain just one field where we select the FW rule we want to delete. 


