---
title: Adding a backend to your SSA
nav: true
---

# Configuring the Webhook

Go to the `Backend` tab  and look at the default configuration, pay some attention on how the body is currently constructed, try it out with the button `<> Test JQ`

Now the real payload which is expected is the following one : 

```
{
    "identifier": "r_phK5hvHMbbQ8lnuV",
    "title": "Request Firewall Rule",
    "icon": "DefaultBlueprint",
    "environment": "Development",
    "application_name": {
        "identifier": "448c8b97-d5f1-45a0-b01d-9344c1",
        "title": "AppBench",
        "icon": "Microservice",
        "blueprint": "rt_appben_retail_apps",
        "team": [
            "ukp-appbench-platform-po-team"
        ],
        "properties": {
            "applicationid": "21",
            "name": "AppBench",
            "applicationshortcode": "apboe01",
            "productowner": "Mike.Atkins@landg.com",
            "scrummaster": "saswati.sethi@landg.com",
            "testlead": "seema.patil@landg.com",
            "releasemanager": "mike.atkins@landg.com",
            "costcentre": "",
            "applicationsupportgroup": "UK Protection Cloud Platform Team",
            "infrastructuresupportgroup": "UK Protection Cloud Platform Team",
            "department": "IT",
            "appnumber": "448c8b97-d5f1-45a0-b01d-9344c10517a6",
            "lean_ix_id": null,
            "apptio_id": null
        },
        "relations": {
            "lg_shared_bu": null
        },
        "createdAt": "2024-11-20T15:54:26.544Z",
        "createdBy": "K7RM5Pz7Nw8uPQOAMSyoGhM4UeorMdjz",
        "updatedAt": "2025-02-10T16:43:09.259Z",
        "updatedBy": "K7RM5Pz7Nw8uPQOAMSyoGhM4UeorMdjz",
        "scorecards": {},
        "scorecardsStats": 75,
        "scorecardsRuleCount": 8
    },
    "source_port": "HTTPS: 443",
    "action": "Allow",
    "source_type": "IP Address",
    "source_ip_address": "192.168.1.0",
    "source_cidr_notation": "/26",
    "destination_port": "HTTPS: 443",
    "destination_type": "IP Address",
    "destination_ip_address": "192.168.11.0",
    "destination_cidr_notation": "/26",
    "is_dr_required": false,
    "firstName": "Mike",
    "lastName": "Atkins",
    "email": "adm-ma81830@landg.com",
    "id": "user_BnDSS62g4sT75M8j",
    "new_request": true,
    "relations": {
        "rt_appben_retail_apps": "448c8b97-d5f1-45a0-b01d-9344c1"
    }
}
```

First thing that you notice is that we have a property called `application_name` and which contains the payload of an `rt_appben_retail_apps` entity. Our current payload have a property `application` with the type `Text`. 
Modify your SSA, so that you are able to select an `rt_appben_retail_apps` entity and the property should have the name `application_name`.

Change the request type to `Sync`. 

The Webhook URL is : `https://cptpoc.azurewebsites.net:443/api/New_Firewall_Request/triggers/When_a_HTTP_request_is_received/invoke?api-version=2022-05-01&sp=%2Ftriggers%2FWhen_a_HTTP_request_is_received%2Frun&sv=1.0&sig=3dbe8dcMYjl_flQ-6RZpiLDJol1XTLB1R7cSAgVlQaM` 


## Adding FW Rules to the catalog

Once you get your SSA working, you want to have visibility on the FW Rules that you have added. Usually you would have a Port exporter polling the backend and sending it back to Port. 

Another approach could creating an automation that gets triggered when an action runs successfully. Try to implement this approach. 

Use this as source of inspiration, you will have to update the mapping section : 

```
{
  "identifier": "create_firewall_rule_entity",
  "title": "Create Firewall Rule Entity",
  "icon": "Firewall",
  "description": "Creates a firewall rule entity when the create_firewall_rule action is completed",
  "trigger": {
    "type": "automation",
    "event": {
      "type": "RUN_UPDATED",
      "actionIdentifier": "create_firewall_rule"
    }
  },
  "invocationMethod": {
    "type": "UPSERT_ENTITY",
    "blueprintIdentifier": "firewall_rule",
    "mapping": {
      "identifier": "{{ .trigger.payload.properties.rule_id }}",
      "title": "{{ .trigger.payload.properties.rule_name }}",
      "properties": {
        "rule_id": "{{ .trigger.payload.properties.rule_id }}",
        "source_ip": "{{ .trigger.payload.properties.source_ip }}",
        "destination_ip": "{{ .trigger.payload.properties.destination_ip }}",
        "port": "{{ .trigger.payload.properties.port }}",
        "protocol": "{{ .trigger.payload.properties.protocol }}"
      }
    }
  },
  "publish": true
}

```



