---

title: Learn about the audit logs in Azure Active Directory
description: Overview of the audit logs available in Azure Active Directory.
services: active-directory
author: shlipsey3
manager: amycolannino
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/04/2022
ms.author: sarahlipsey
ms.reviewer: besiler

---

# Audit logs in Azure Active Directory 

Azure Active Directory (Azure AD) activity logs include audit logs, which is a comprehensive report on every logged event in Azure AD. Changes to applications, groups, users, and licenses are all captured in the Azure AD audit logs.

Two other activity logs are also available to help monitor the health of your tenant:

- **[Sign-ins](concept-sign-ins.md)** – Information about sign-ins and how your resources are used by your users.
- **[Provisioning](concept-provisioning-logs.md)** – Activities performed by the provisioning service, such as the creation of a group in ServiceNow or a user imported from Workday.

This article gives you an overview of the audit logs.

## What can you do with audit logs?

Audit logs in Azure AD provide access to system activity records, often needed for compliance. You can get answers to questions related to users, groups, and applications.

**Users:**

- What types of changes were recently applied to users?
- How many users were changed?
- How many passwords were changed?

**Groups:**

- What groups were recently added?
- Have the owners of group been changed?
- What licenses have been assigned to a group or a user?

**Applications:**

- What applications have been added, updated, or removed?
- Has a service principal for an application changed?
- Have the names of applications been changed?
 
## What do the logs show?

Audit logs have a default list view that shows:

- Date and time of the occurrence
- Service that logged the occurrence
- Category and name of the activity (*what*) 
- Status of the activity (success or failure)
- Target
- Initiator / actor of an activity (*who*)

You can customize and filter the list view by clicking the **Columns** button in the toolbar. Editing the columns enables you to add or remove fields from your view.

### Filtering audit logs

You can filter the audit data using the options visible in your list such as date range, service, category, and activity. 

![Screenshot of the service filter.](./media/concept-audit-logs/audit-log-service-filter.png)

- **Service**: Defaults to all available services, but you can filter the list to one or more by selecting an option from the dropdown list.

- **Category**: Defaults to all categories, but can be filtered to view the category of activity, such as changing a policy or activating an eligible Azure AD role.

- **Activity**: Based on the category and activity resource type selection you make. You can select a specific activity you want to see or choose all.
 
    You can get the list of all Audit Activities using the Graph API: `https://graph.windows.net/<tenantdomain>/activities/auditActivityTypesV2?api-version=beta`

- **Status**: Allows you to look at result based on if the activity was a success or failure.

- **Target**: Allows you to search for the target or recipient of an activity. Search by the first few letters of a name or user principal name (UPN). The target name and UPN are case-sensitive. 

- **Initiated by**: Allows you to search by who initiated the activity using the first few letters of their name or UPN. The name and UPN are case-sensitive.

- **Date range**: Enables to you to define a timeframe for the returned data. You can search the last 7 days, 24 hours, or a custom range. When you select a custom timeframe, you can configure a start time and an end time.

    You can also choose to download the filtered data, up to 250,000 records, by selecting the **Download** button. You can download the logs in either CSV or JSON format. The number of records you can download is constrained by the [Azure Active Directory report retention policies](reference-reports-data-retention.md).

    ![Screenshot of the download data option.](./media/concept-audit-logs/download.png "Download data")

## Microsoft 365 activity logs

You can view Microsoft 365 activity logs from the [Microsoft 365 admin center](/office365/admin/admin-overview/about-the-admin-center). Even though Microsoft 365 activity and Azure AD activity logs share many directory resources, only the Microsoft 365 admin center provides a full view of the Microsoft 365 activity logs. 

You can also access the Microsoft 365 activity logs programmatically by using the [Office 365 Management APIs](/office/office-365-management-api/office-365-management-apis-overview).

> [!NOTE]
> Most standalone or bundled Microsoft 365 subscriptions have back-end dependencies on some subsystems within the Microsoft 365 datacenter boundary. The dependencies require some information write-back to keep directories in sync and essentially to help enable hassle-free onboarding in a subscription opt-in for Exchange Online. For these write-backs, audit log entries show actions taken by “Microsoft Substrate Management”. These audit log entries refer to create/update/delete operations executed by Exchange Online to Azure AD. The entries are informational and don't require any action.

## Next steps

- [Azure AD audit activity reference](reference-audit-activities.md)
- [Azure AD logs retention reference](reference-reports-data-retention.md)
- [Azure AD log latencies reference](./reference-azure-ad-sla-performance.md)
- [Unknown actors in audit report](/troubleshoot/azure/active-directory/unknown-actors-in-audit-reports)
