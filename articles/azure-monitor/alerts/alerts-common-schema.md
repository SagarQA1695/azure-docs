---
title: Common alert schema for Azure Monitor alerts
description: Understand the common alert schema, why you should use it, and how to enable it.
ms.topic: conceptual
ms.date: 03/14/2019
ms.reviewer: ofmanor
---

# Common alert schema

This article describes what the common alert schema is, the benefits of using it, and how to enable it.

## What is the common alert schema?

The common alert schema standardizes the consumption experience for alert notifications in Azure. Today, Azure has three alert types, metric, log, and activity log. Historically, they've had their own email templates and webhook schemas. With the common alert schema, you can now receive alert notifications with a consistent schema.

Any alert instance describes the resource that was affected and the cause of the alert. These instances are described in the common schema in the following sections:

- **Essentials**: Standardized fields, common across all alert types, describe what resource the alert is on along with other common alert metadata. Examples include severity or description.
- **Alert context**: These fields describe the cause of the alert, with fields that vary based on the alert type. For example, a metric alert would have fields like the metric name and metric value in the alert context. An activity log alert would have information about the event that generated the alert.

You might want to route the alert instance to a specific team based on a pivot such as a resource group. The common schema uses the essential fields to provide standardized routing logic for all alert types. The team can use the context fields for their investigation.

As a result, you can potentially have fewer integrations, which makes the process of managing and maintaining them a much simpler task. Future alert payload enrichments like customization and diagnostic enrichment will only surface in the common schema.

## What enhancements does the common alert schema bring?

You'll see the benefits of using a common alert schema in your alert notifications. A common alert schema provides these benefits:

| Action | Enhancements|
|:---|:---|
| Email | A consistent and detailed email template. You can use it to easily diagnose issues at a glance. Embedded deep links to the alert instance on the portal and the affected resource ensure that you can quickly jump into the remediation process. |
| Webhook/Azure Logic Apps/Azure Functions/Azure Automation runbook | A consistent JSON structure for all alert types. You can use it to easily build integrations across the different alert types. |

The new schema will also enable a richer alert consumption experience across both the Azure portal and the Azure mobile app in the immediate future.

Learn more about the [schema definitions for webhooks, Logic Apps, Azure Functions, and Automation runbooks](./alerts-common-schema-definitions.md).

> [!NOTE]
> The following actions don't support the common alert schema ITSM Connector.

## How do I enable the common alert schema?

Use action groups in the Azure portal or use the REST API to enable the common alert schema. You can enable a new schema at the action level. For example, you must separately opt in for an email action and a webhook action.

> [!NOTE]
> Smart detection alerts support the common schema by default. No opt-in is required.
>
> Alerts generated by [VM insights](../vm/vminsights-overview.md) currently don't support the common schema.
>

### Through the Azure portal

![Screenshot that shows the common alert schema opt in.](media/alerts-common-schema/portal-opt-in.png)

1. Open any existing action or a new action in an action group.
1. Select **Yes** to enable the common alert schema.

### Through the Action Groups REST API

You can also use the [Action Groups API](/rest/api/monitor/actiongroups) to opt in to the common alert schema. While you make the [create or update](/rest/api/monitor/actiongroups/createorupdate) REST API call, you can set the flag "useCommonAlertSchema" to `true` to opt in or `false` to opt out for email, webhook, Logic Apps, Azure Functions, or Automation runbook actions.

For example, the following request body made to the [create or update](/rest/api/monitor/actiongroups/createorupdate) REST API will:

- Enable the common alert schema for the email action "John Doe's email."
- Disable the common alert schema for the email action "Jane Smith's email."
- Enable the common alert schema for the webhook action "Sample webhook."

```json
{
  "properties": {
    "groupShortName": "sample",
    "enabled": true,
    "emailReceivers": [
      {
        "name": "John Doe's email",
        "emailAddress": "johndoe@email.com",
        "useCommonAlertSchema": true
      },
      {
        "name": "Jane Smith's email",
        "emailAddress": "janesmith@email.com",
        "useCommonAlertSchema": false
      }
    ],
    "smsReceivers": [
      {
        "name": "John Doe's mobile",
        "countryCode": "1",
        "phoneNumber": "1234567890"
      },
      {
        "name": "Jane Smith's mobile",
        "countryCode": "1",
        "phoneNumber": "0987654321"
      }
    ],
    "webhookReceivers": [
      {
        "name": "Sample webhook",
        "serviceUri": "http://www.example.com/webhook",
        "useCommonAlertSchema": true
      }
    ]
  },
  "location": "Global",
  "tags": {}
}
```

## Next steps

- [Learn the common alert schema definitions for webhooks, Logic Apps, Azure Functions, and Automation runbooks](./alerts-common-schema-definitions.md)
- [Learn how to create a logic app that uses the common alert schema to handle all your alerts](./alerts-common-schema-integrations.md)
