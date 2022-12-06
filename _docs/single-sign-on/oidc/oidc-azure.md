---
title: "Azure (OIDC)"
description: "Setting Up Azure Single Sign-On (SSO)"
group: single-sign-on
sub_group: oidc
redirect_from:
  - /docs/enterprise/sso-azure/
  - /docs/enterprise/single-sign-on/sso-azure/
  - /docs/administration/single-sign-on/sso-azure/
toc: true
---

In this page we will see the process of setting up Azure SSO for Codefresh. Setting up SSO for Azure requires configuration both in Azure and in Codefresh.  
For general instructions on SSO setup, see the [overview page]({{site.baseurl}}/docs/single-sign-on/oidc/).

<!---
(#step-1-register-the-codefresh-application-in-azure)
(#step-2-configure-permissions-for-the-application-in-azure)
(#step-3-create-client-secret-in-azure)-->


## Prerequisites

Make sure that your user in Azure who creates the application is assigned either of these roles:  
**Application Administrator**  
OR  
**Global Administrator**  

If the user who creates the Azure application is not assigned to either of these roles, you will not be able to [sync teams from Azure to Codefresh]({{site.baseurl}}/docs/single-sign-on/oidc//#syncing-of-teams-after-initial-sso-setup) after the SSO integration is complete.


## Step 1: Register the Codefresh application in Azure

To setup Azure Active Directory for SSO, first register a new application in Azure.

1. Log in to the **Azure Portal**, and from the sidebar, select **Azure Active Directory**.

{% include image.html
lightbox="true"
file="/images/administration/sso/azure/register-app-select-azure-ad.png"
url="/images/administration/sso/azure/register-app-select-azure-ad.png"
alt="Azure Active Directory"
caption="Azure Active Directory"
max-width="70%"
%}

{:start="2"}
1. From the sidebar, select **App registrations**, and then click **+ New registration**.
1. Enter a name for the application, for example, `Codefresh`, and retain the default values for all other settings.

{% include image.html
lightbox="true"
file="/images/administration/sso/azure/register-app-name.png"
url="/images/administration/sso/azure/register-app-name.png"
alt="Enter name and register application"
caption="Enter name and register application"
max-width="70%"
%}

{:start="4"}
1. To apply your changes, click **Register**. The application is created and registered in Azure AD.


## Step 2: Configure permissions for the application in Azure

Once the application has been created and registered, configure the required permissions. 

1. Click the name of the application to open **Settings**.
1. Do the following:  
  * Select **API permissions**, and then click **+ Add a permission**.
  * From **Request API Permissions**, select **Microsoft APIs**, and then select **Microsoft Graph**. 

{% include image.html
lightbox="true"
file="/images/administration/sso/azure/config-app-permissions-microsoft-graph.png"
url="/images/administration/sso/azure/config-app-permissions-microsoft-graph.png"
alt="Select Microsoft Graph"
caption="Select Microsoft Graph"
max-width="70%"
%}

{:start="3"}
1. Click **Application permissions** on the left, and select `Group > Read.All`.

> The `User.Read.All (Delegated)` permission is added by default.

{% include image.html
lightbox="true"
file="/images/administration/sso/azure/config-app-permissions-selected.png"
url="/images/administration/sso/azure/config-app-permissions-selected.png"
alt="`Group > Read.All` permissions for Microsoft Graph"
caption="`Group > Read.All` permissions for Microsoft Graph"
max-width="70%"
%}

{:start="4"}
1. Click **Add Permissions**.
1. Click **Grant admin consent for Default Directory** on the bar.

{% include image.html
lightbox="true"
file="/images/administration/sso/azure/config-app-permissions-added.png"
url="/images/administration/sso/azure/config-app-permissions-added.png"
alt="Grant admin consent for Default Directory"
caption="Grant admin consent for Default Directory"
max-width="70%"
%}

## Step 3: Create client secret in Azure

Create a client secret for the application. You will need to provide it when you set up SSO for Azure in Codefresh.

1. From the sidebar, select **Certificates & secrets**, and then click **+ New client secret**.

{% include image.html
lightbox="true"
file="/images/administration/sso/azure/client-secret-select-option.png"
url="/images/administration/sso/azure/client-secret-select-option.png"
alt="Create client secret"
caption="Create client secret"
max-width="70%"
%}

{:start="2"}
1. Optional. Add a meaningful description for the client secret, and either retain the default expiry date or define a custom one.

{% include image.html
lightbox="true"
file="/images/administration/sso/azure/client-secret-add-description.png"
url="/images/administration/sso/azure/client-secret-add-description.png"
alt="Description for client secret"
caption="Description for client secret"
max-width="70%"
%}

> Tip: Make a note of the expiry date in your calendar to renew the key before the expiry date and prevent service interruptions.

{:start="3"}
1. Click **Add**.  
   **Copy the secret key**, as you will need to provide it on setting up Azure SSO in Codefresh.

## Step 4: Configure SSO for Azure in Codefresh

Configure SSO for Azure in the Codefresh UI.

**Before you begin**  
* From Azure AD:
  * Have your client secret handy
  * Go to the application you created, and note down these **Properties: Application ID and Object ID**
  
   {% include image.html
lightbox="true"
file="/images/administration/sso/azure/azure-properties-object-app-ids.png"
url="/images/administration/sso/azure/azure-properties-object-app-ids.png"
alt="Application and Object IDs in Azure"
caption="Application and Object IDs in Azure"
max-width="70%"
%}


**How to**  

1. In the Codefresh UI, select **Account Settings**, and then from the sidebar, select **Single Sign On**. 
1. Click **Add Single Sign-On**, and select **Azure AD**.
1. Enter the following:  
  * **Client Name**: For auto-generation, leave empty. Codefresh generates the client name once you save the settings.
  * **Display Name**: Meaningful name for the SSO provider - Shown as display name in Azure (see below)
  * **Access token** and **Application ID**: The Application ID from your Enterprise Application Properties in Azure AD.
  * **Client Secret**: The key value you copied when you created the client secret in Azure.
  * **Tenant**: `mycompany.onmicrosoft.com` or the ID of `0example1-0000-0aa0-a00a-1example0`
  * **Object ID**: The Object ID from your Enterprise Application Properties in Azure AD.
  * **Auto Sync users and teams to Codefresh**: Select to automatically sync user accounts in Azure AD to your Codefresh account. Optionally, define the time interval, in hours, at which to sync, from 1 to 24. If you don’t specify an interval, the sync is every 12 hours.

  {% include image.html
lightbox="true"
file="/images/administration/sso/azure/sso-codefresh-settings.png"
url="/images/administration/sso/azure/sso-codefresh-settings.png"
alt="SSO settings for Azure in Codefresh"
caption="SSO settings for Azure in Codefres"
max-width="70%"
%}

{:start="4"}
1. Click **Save**.  
   If you left the Client Name empty, Codefresh generates one (as in the example below). Codefresh uses this name to identify the SSO configuration.

{% include image.html
lightbox="true"
file="/images/administration/sso/azure/sso-codefresh-generated-client-id.png"
url="/images/administration/sso/azure/sso-codefresh-generated-client-id.png"
alt="Example of Codefresh-generated Client Name for Azure"
caption="Example of Codefresh-generated Client Name for Azure"
max-width="50%"
%}

We will need this value in the reply URL setting (back in the Azure portal UI).

## Step 5: Configure redirect URIs in Azure

As the final step, add the Codefresh callback URL to the allowed reply URLs for the created application in Azure.

**Before you begin**  
* Make sure you have the Client Name for the Azure SSO configuration from Codefresh 


**How to**  

1. Go to **Azure Active Directory > Apps registrations**, and select the application you registered for SSO.
1. From the sidebar, select **Authentication**.
1. Below **Platform Configuration**, click **Add a platform** and then select **Web**.

{% include image.html
lightbox="true"
file="/images/administration/sso/azure/redirect-uri-web-configure.png"
url="/images/administration/sso/azure/redirect-uri-web-configure.png"
alt="Select Web configuration settings"
caption="Select Web configuration settings"
max-width="70%"
%}

{:start="4"}
1. In the Configure Web form, do the following:  
  * In the **Redirect URIs** field, enter the redirect URI in the format below:  
    `https://g.codefresh.io/api/auth/<your_codefresh_sso_client_name>/callback`  
    where:  
    `<your_codefresh_sso_client_name>` is the Client Name shown in the SSO configuration, either defined by you or created by Codefresh.  
  * Select **ID tokens**.

{% include image.html
lightbox="true"
file="/images/administration/sso/azure/redirect-rui-define-select-id-tokens.png"
url="/images/administration/sso/azure/redirect-rui-define-select-id-tokens.png"
alt="Web configuration settings"
caption="Web configuration settings"
max-width="70%"
%}

You have now completed the SSO setup for Azure.

## What to read next

See the [overview page]({{site.baseurl}}/docs/single-sign-on/oidc/#testing-your-identity-provider) on how to test the integration, activate SSO for collaborators and create sync jobs.