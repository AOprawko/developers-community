---
pagename: Let's Get Started
sitesection: Documents
categoryname: "Getting Started"
documentname: Before You Get Started
permalink: before-you-get-started-let-s-get-started.html
indicator: both
---


Most user actions within LiveEngage can be performed programmatically using the REST API. Since we use a REST model, we recommend you be fluent in:

- JavaScript, and its iterations like Node.JS, as well as JSON. Although most JSON payloads tend to be simple, there are exceptions to this rule, like with Structured Content.
- Swift and Java, respectively, for Mobile App Messaging SDKs for iOS and Android.
- HTTP calls and responses, REST APIs, server to server communication and web applications.
- Retrieving information and data analysis/research for the Data APIs.

Also, if you have not already done so:

1. Read the [API Terms of Use](https://www.liveperson.com/policies/apitou).
2. Read the [Systems Requirements and Language Support](https://s3-eu-west-1.amazonaws.com/ce-sr/CA/Admin/Sys+req/System+requirements.pdf) guide.

And before you can do anything, you must do a few things first. By the end of this Getting Started guide, you will be ready to customize and implement features in LiveEngage.

<p><br></p>
---
<p></p>

### Step 1. Create a LiveEngage account

Before you can use LiveEngage, you must first have a working account. If you don't already have one, you can sign up for a [free trial account](https://developers.liveperson.com/register.html) and then contact your account team or LivePerson support to have features enabled. For example, if you know you are going to build a customized messaging window, you will need to make sure that messaging is enabled on your account.

{:.note}
If you already have a LiveEngage account, you can use that account instead of a creating a free trial account. However, we recommend creating a new account to make sure that any changes and customizations you make do not affect your site visitors until you are ready to launch them.

Some features to get started with include:

- Messaging and push notifications
- Bot for User type
- Authenticated chat
- Audio messaging
- Vibrate on new incoming message
- Photo sharing

### Step 2. Retrieve your domain

Before you get started with any LivePerson API, you must retrieve the base domain of LivePerson using the **Domain API** (a read-only API). [More information on using the Domain API can be found here](essential-resources-domain-api.html).

### Step 3. Select the login method

In this step, you choose whether to access LiveEngage sessions with the **User Login** or **Application Login** method.

#### User Login

The **User Login** method provides a way for _users_ to access and manage a LiveEngage session. A session could be a customized workspace or hosting a LiveEngage user in an external app. Use the LiveEngage username and password.

{:.notice}
Logged in users are managed under the same restrictions as regular users in LiveEngage, including password expiration, concurrent conversation limits, skill groups and so on. If your account has SSO enabled, this method is not standalone, and you must register the relevant user through your hub to provide LiveEngage with an SSO SAML assertion.

#### Application Login

The **Application Login** method provides a way for _applications_ to access and manage LiveEngage sessions, such as bots, virtual agents, or other apps that need to act as an agent. The Application Login method overcomes both issues detailed in the User Login method (above), such as password expiration or SSO integration.

{:.important}
To log in as an application, you must have the _User Type - Bot_ feature activated. If you do not, contact your account team before proceeding.  

<p style="text-align: right">
<a href="essential-resources-authentication.html" center><img src="../../img/btn-view-auth-docs.png"></a></p>

<p><br></p>
---
<p></p>

### Step 4. Authorize API Calls

In this step, now that you’ve chosen your login method.  Some of our APIs require authorization before you can use them.  Every API uses either user login credentials or an API key, or both, which you can find in the overview of the API itself.

1. Use the [Login Service API](login-service-api-overview.html) to log into LiveEngage to get a session token (Bearer) to use for other related API calls.  You can log in as a *user* with credentials or an *application* with an API key.

2. Use this token in your authentication header in the following API calls:

   - [Users API](users-api-overview.html) - updates the LiveEngage user list on regular intervals

   - [Login Service API](login-service-api-overview.html) - provides endpoints to manage the user session in LiveEngage

   - [Domain API](retrieve-api-domains-using-the-domain-api.html) - retrieves the base domain of LivePerson

<p><br></p>
---
<p></p>

### Step 5. Authenticate with LiveEngage

All authenticated information is encrypted and transferred over Secure Sockets Layer (SSL), using the OAuth 2.0 and OpenID Connect standards, via a JSON Web Token (JWT).  Authentication occurs when the brand's Identity Provider (IDP) provides unique information on each customer to LiveEngage via the standard OpenID Connect JWT method. The advantage is that LiveEngage displays the customer information to the brand's agent under OAuth 2.0 and OpenID Connect protocols, providing a high degree of security and certainty regarding the consumer's identity.

{:.notice}
For more details on what information is available from the IDP, refer to the [personalInfo](messaging-interactions-api-engagement-attributes.html#personalinfo) engagement attribute and the [customerInfo](messaging-interactions-api-engagement-attributes.html#customerinfo) engagement attribute.

#### Unauthenticated engagements

For unauthenticated engagements to work, contact your LivePerson account team who can enable the Unauthenticated Messaging feature on your account.

#### Authenticated engagements

1. Log into LiveEngage as Administrator and along the top open the **Campaigns** area.

2. In the footnote, click **Data Sources**.

3. In the Connectors area, and next to the authentication server, click **Configure**.

   The Authentication Server page opens.

4. In the **Define the authentication service URLs** drop-down menu, select your authentication method.

    ![Authentication Server](img/authserver.png)

5.  Configure the following parameters:

   {:.important}
   You can only have one authentication type for both web and in-app messaging. For example, if you use the implicit flow for web messaging, then you must also use implicit flow for in-app message.

   - Implicit or Code Flow

   - OAuth 2.0 Authentication Endpoint (when using a separate browser window)

   - OAuth 2.0 Token Endpoint (when using Code Flow)

   - OAuth 2.0 JWT Public Key

   - OAuth 2.0 Client ID

   - OAuth 2.0 Client Secret (when using Code Flow)

   - JS method name and context (when using LiveEngage embedded window in Web)


<p><br></p>
---
<p></p>

### Step 6. Create an API key

Application keys are security tokens that you use to log into LiveEngage. The application key gets installed automatically and assumes the security settings granted to the associated user in LiveEngage.

{:.notice}
You use the API key when you create bots.

1. Log into LiveEngage with Administrator or Campaign Manager permissions and along the top open the **Campaigns** area.

2. In the footnote click **Data Sources**.

3. Open the API tab and click **Add new**.

   **TIP:** Alternatively, you can click on an existing key to edit its privileges.

4. Provide the name of the application and the developer name. Optionally you can provide a description of the app by clicking the **Add description** link below the Application name field.

5. Select the **Agent Interactions** category, click the **User Login** checkbox to select it, and then click **Save**.

   ![](../../../img/APIKeyCreation.png)

6. Once the API key has been successfully created, the authentication details display for four AUTH request values, which you use in the request body of this API:

   - App key

   - Secret

   - Access token

   - Access token secret

   ![](../../../img/apikeycreation1.png)

7. If the window does not close automatically, click the **X** in the top right to close the window.

   ![](../../../img/close-window.png)

8. Using the authentication details, generate an OAuth header according to the [OAuth Specification Section 9](https://oauth.net/core/1.0/#signing_process).

**Example:**

| **Header Name** | **Authorization** |
| --- | --- |
| Header Value | OAuth<br>oauth_signature="JA0PvBbTAxmtLmzIWINpSVLshrY%3D", <br>oauth_version="1.0",<br>oauth_nonce="c1c04ec4-3125-44cf-9c39-cccb9343541b", <br>oauth_consumer_key="d392e7ff2e204d6c802e38fd775563d1", <br>oauth_signature_method="HMAC-SHA1", <br>oauth_token="61adad31204a4e6fab68d560f1ffb594", <br>oauth_timestamp="1261039670" <br><br>**Note:** The authorization should be contained on a single line. New lines have been inserted for clarity. |

<p><br></p>
---
<p></p>

### Next steps

Congratulations!  You are now ready to customize and implement features in LiveEngage.

- **Integrate LiveEngage with iOS and Android apps** to create and manage digital engagements.  Additionally, customers can communicate with the brand's agents through their channel of choice. For more information, refer to the Mobile Messaging SDK for [Android](mobile-app-messaging-sdk-for-android-overview.html) or [iOS](mobile-app-messaging-sdk-for-ios-overview.html).

- **Enable consumer monitoring and engagement flows** to gather reporting data about the consumer, engagements, and campaigns. By combining monitoring capabilities with our Campaigns feature, you can display tailored engagements to the right consumer at the right time. For more information, refer to the [Monitoring API](monitoring-api-overview.html).

- **Integrate a messaging bot into LiveEngage** to send/receive text messages, send structured content, transfer the conversation to other skills, change Time To Response, and close a conversation. You use the [Agent Messaging SDK](messaging-agent-sdk-overview.html) to connect a bot, but your account must be enabled to support bot users. To check the enabled features for your account, contact your account team or LivePerson Support.

- **Manage users in LiveEngage** to update user lists that may need updating on a regular basis. You use the [Users API](users-api-overview.html) to make updates such as profile pictures, login names, passwords, or user assignments. You can also synchronize any HR or staffing system with LiveEngage.

- **Implement a retry policy**. We recommend that you add a mechanism to your API call to increase reliability and stability. Each component in a network can return an error, which can cause your application to fail.  If an error returns, the mechanism makes sure that your application attempts to retrieve the relevant information. [More information on our retry policy best practices can be found here](retry-and-keepalive-best-practices-overview.html).
