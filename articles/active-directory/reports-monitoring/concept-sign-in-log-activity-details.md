---
title: Learn about the sign-in log activity details
description: Learn about the information available on each of the tabs on the Azure AD sign-in log activity details.
services: active-directory
author: shlipsey3
manager: amycolannino
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: report-monitor
ms.date: 08/22/2023
ms.author: sarahlipsey
ms.reviewer: besiler
---

# Learn about the sign-in log activity details

Azure AD logs all sign-ins into an Azure tenant for compliance purposes. As an IT administrator, you need to know what the values in the sign-in logs mean, so that you can interpret the log values correctly.

- [Learn about the sign-in logs](concept-sign-ins.md).
- [Customize and filter the sign-in logs](howto-customize-filter-logs.md)

This article explains the values on the Basic info tab of the sign-ins log.

## Basic info tab

The Basic info tab contains most of the details that are also displayed in the table. You can launch the Sign-in Diagnostic from the Basic info tab. For more information, see [How to use the Sign-in Diagnostic](howto-use-sign-in-diagnostics.md).

### Sign-in error codes

If a sign-in failed, you can get more information about the reason in the Basic info tab of the related log item. The error code and associated failure reason appear in the details. For more information, see [How to troubleshoot sign-in errors.](howto-troubleshoot-sign-in-errors.md).

![Screenshot of the sign-in error code on the basics tab.](media/concept-sign-in-log-activity-details/sign-in-error-code.png)

## Location and Device info

The **Location** and **Device info** tabs display general information about the location and IP address of the user. The Device info tab provides details on the browser and operating system used to sign in. This tab also provides details on if the device is compliant, managed, or hybrid Azure AD joined.

## Authentication details

The **Authentication Details** tab in the details of a sign-in log provides the following information for each authentication attempt:

- A list of authentication policies applied, such as Conditional Access or Security Defaults.
- The sequence of authentication methods used to sign-in.
- If the authentication attempt was successful and the reason why.

This information allows you to troubleshoot each step in a user’s sign-in. Use these details to track:

- The volume of sign-ins protected by MFA. 
- Usage and success rates for each authentication method.
- Usage of passwordless authentication methods, such as Passwordless Phone Sign-in, FIDO2, and Windows Hello for Business.
- How frequently authentication requirements are satisfied by token claims, such as when users aren't interactively prompted to enter a password or enter an SMS OTP.

![Screenshot of the Authentication Details tab.](media/concept-sign-in-log-activity-details/sign-in-activity-details-authentication.png)

When analyzing authentication details, take note of the following details:

- **OATH verification code** is logged as the authentication method for both OATH hardware and software tokens (such as the Microsoft Authenticator app).
- The **Authentication details** tab can initially show incomplete or inaccurate data until log information is fully aggregated. Known examples include: 
    - A **satisfied by claim in the token** message is incorrectly displayed when sign-in events are initially logged. 
    - The **Primary authentication** row isn't initially logged.
- If you're unsure of a detail in the logs, gather the **Request ID** and **Correlation ID** to use for further analyzing or troubleshooting.
- If Conditional Access policies for authentication or session lifetime are applied, they're listed above the sign-in attempts. If you don't see either of these, those policies aren't currently applied. For more information, see [Conditional Access session controls](../conditional-access/concept-conditional-access-session.md).


## Unique identifiers 

In Azure AD, a resource access has three relevant components:

- **Who** – The identity (User) doing the sign-in. 
- **How** – The client (Application) used for the access.  
- **What** – The target (Resource) accessed by the identity.

Each component has an associated unique identifier (ID).

### Tenant

The sign-in log tracks two tenant identifiers:

- **Home tenant** – The tenant that owns the user identity. 
- **Resource tenant** – The tenant that owns the (target) resource.

These identifiers are relevant in cross-tenant scenarios. For example, to find out how users outside your tenant are accessing your resources, select all entries where the home tenant doesn’t match the resource tenant.
For the home tenant, Azure AD tracks the ID and the name. 

### Request ID

The request ID is an identifier that corresponds to an issued token. If you're looking for sign-ins with a specific token, you need to extract the request ID from the token, first.


### Correlation ID

The correlation ID groups sign-ins from the same sign-in session. The identifier was implemented for convenience. Its accuracy isn't guaranteed because the value is based on parameters passed by a client. 

### Sign-in

The sign-in identifier is a string the user provides to Azure AD to identify itself when attempting to sign-in. It's usually a user principal name (UPN), but can be another identifier such as a phone number. 

### Authentication requirement 

This attribute shows the highest level of authentication needed through all the sign-in steps for the sign-in to succeed. Graph API supports `$filter` (`eq` and `startsWith` operators only).

### Sign-in event types 

Indicates the category of the sign in the event represents. For user sign-ins, the category can be `interactiveUser` or `nonInteractiveUser` and corresponds to the value for the **isInteractive** property on the sign-in resource. For managed identity sign-ins, the category is `managedIdentity`. For service principal sign-ins, the category is **servicePrincipal**. The Azure portal doesn't show this value, but the sign-in event is placed in the tab that matches its sign-in event type. Possible values are:

- `interactiveUser`
- `nonInteractiveUser`
- `servicePrincipal`
- `managedIdentity`
- `unknownFutureValue`

The Microsoft Graph API, supports: `$filter` (`eq` operator only)

### User type 

The type of a user. Examples include `member`, `guest`, or `external`.


### Cross-tenant access type 

This attribute describes the type of cross-tenant access used by the actor to access the resource. Possible values are: 

- `none` - A sign-in event that didn't cross an Azure AD tenant's boundaries.
- `b2bCollaboration`- A cross tenant sign-in performed by a guest user using B2B Collaboration.
- `b2bDirectConnect` - A cross tenant sign-in performed by a B2B.
- `microsoftSupport`- A cross tenant sign-in performed by a Microsoft support agent in a Microsoft customer tenant.
- `serviceProvider` - A cross-tenant sign-in performed by a Cloud Service Provider (CSP) or similar admin on behalf of that CSP's customer in a tenant
- `unknownFutureValue` - A sentinel value used by MS Graph to help clients handle changes in enum lists. For more information, see [Best practices for working with Microsoft Graph](/graph/best-practices-concept).

If the sign-in didn't the pass the boundaries of a tenant, the value is `none`.

### Conditional Access evaluation 

This value shows whether continuous access evaluation (CAE) was applied to the sign-in event. There are multiple sign-in requests for each authentication. Some are shown on the interactive tab, while others are shown on the non-interactive tab. CAE is only displayed as true for one of the requests, and it can be on the interactive tab or non-interactive tab. For more information, see [Monitor and troubleshoot sign-ins with continuous access evaluation in Azure AD](../conditional-access/howto-continuous-access-evaluation-troubleshoot.md). 

## Next steps

* [Learn about exporting Azure AD sign-in logs](concept-activity-logs-azure-monitor.md)
* [Explore the sign-in diagnostic in Azure AD](./howto-use-sign-in-diagnostics.md)
