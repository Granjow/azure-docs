---
title: Conditional Access - Combined security information - Azure Active Directory
description: Create a custom Conditional Access policy for security info registration

services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 08/22/2022

ms.author: joflore
author: MicrosoftGuyJFlo
manager: amycolannino
ms.reviewer: calebb

ms.collection: M365-identity-device-management
---
# Conditional Access: Securing security info registration

Securing when and how users register for Azure AD multifactor Authentication and self-service password reset is possible with user actions in a Conditional Access policy. This feature is available to organizations who have enabled the [combined registration](../authentication/concept-registration-mfa-sspr-combined.md). This functionality allows organizations to treat the registration process like any application in a Conditional Access policy and use the full power of Conditional Access to secure the experience. Users signing in to the Microsoft Authenticator app or enabling passwordless phone sign-in are subject to this policy.

Some organizations in the past may have used trusted network location or device compliance as a means to secure the registration experience. With the addition of [Temporary Access Pass](../authentication/howto-authentication-temporary-access-pass.md) in Azure AD, administrators can provide time-limited credentials to their users that allow them to register from any device or location. Temporary Access Pass credentials satisfy Conditional Access requirements for multifactor authentication.

## Template deployment

Organizations can choose to deploy this policy using the steps outlined below or using the [Conditional Access templates (Preview)](concept-conditional-access-policy-common.md#conditional-access-templates-preview). 

## Create a policy to secure registration

The following policy applies to the selected users, who attempt to register using the combined registration experience. The policy requires users to be in a trusted network location, do multifactor authentication or use Temporary Access Pass credentials.

1. In the **Azure portal**, browse to **Azure Active Directory** > **Security** > **Conditional Access**.
1. Select **New policy**.
1. In Name, Enter a Name for this policy. For example, **Combined Security Info Registration with TAP**.
1. Under **Assignments**, select **Users or workload identities**.
   1. Under **Include**, select **All users**.

      > [!WARNING]
      > Users must be enabled for the [combined registration](../authentication/howto-registration-mfa-sspr-combined.md).

   1. Under **Exclude**.
      1. Select **All guest and external users**.
      1. Select **Directory roles** and choose **Global administrator**
      
         > [!NOTE]
         > Temporary Access Pass does not work for guest users.

      1. Select **Users and groups** and choose your organization's emergency access or break-glass accounts. 
1. Under **Cloud apps or actions**, select **User actions**, check **Register security information**.
1. Under **Conditions** > **Locations**. 
   1. Set **Configure** to **Yes**. 
      1. Include **Any location**.
      1. Exclude **All trusted locations**.
1. Under **Access controls** > **Grant**. 
   1. Select **Grant access**, **Require multifactor authentication**.
   1. Select **Select**.
1. Confirm your settings and set **Enable policy** to **Report-only**.
1. Select **Create** to create to enable your policy.

After confirming your settings using [report-only mode](howto-conditional-access-insights-reporting.md), an administrator can move the **Enable policy** toggle from **Report-only** to **On**.

Administrators will now have to issue Temporary Access Pass credentials to new users so they can satisfy the requirements for multifactor authentication to register. Steps to accomplish this task, are found in the section [Create a Temporary Access Pass in the Azure AD Portal](../authentication/howto-authentication-temporary-access-pass.md#create-a-temporary-access-pass).

Organizations may choose to require other grant controls with or in place of **Require multifactor authentication** at step 6b. When selecting multiple controls, be sure to select the appropriate radio button toggle to require **all** or **one** of the selected controls when making this change.

### Guest user registration

For [guest users](../external-identities/what-is-b2b.md) who need to register for multifactor authentication in your directory you may choose to block registration from outside of [trusted network locations](concept-conditional-access-conditions.md#locations) using the following guide.

1. In the **Azure portal**, browse to **Azure Active Directory** > **Security** > **Conditional Access**.
1. Select **New policy**.
1. In Name, Enter a Name for this policy. For example, **Combined Security Info Registration on Trusted Networks**.
1. Under **Assignments**, select **Users or workload identities**.
   1. Under **Include**, select **All guest and external users**.
1. Under **Cloud apps or actions**, select **User actions**, check **Register security information**.
1. Under **Conditions** > **Locations**.
   1. Configure **Yes**.
   1. Include **Any location**.
   1. Exclude **All trusted locations**.
1. Under **Access controls** > **Grant**.
   1. Select **Block access**.
   1. Then click **Select**.
1. Confirm your settings and set **Enable policy** to **Report-only**.
1. Select **Create** to create to enable your policy.

After confirming your settings using [report-only mode](howto-conditional-access-insights-reporting.md), an administrator can move the **Enable policy** toggle from **Report-only** to **On**.

## Next steps

[Conditional Access common policies](concept-conditional-access-policy-common.md)

[Determine impact using Conditional Access report-only mode](howto-conditional-access-insights-reporting.md)

[Simulate sign in behavior using the Conditional Access What If tool](troubleshoot-conditional-access-what-if.md)

[Require users to reconfirm authentication information](../authentication/concept-sspr-howitworks.md#reconfirm-authentication-information)
