<!-- Keeping yaml frontmatter commented out for now
---
# Metadata required by https://docs.microsoft.com/samples/browse/
# Metadata properties: https://review.docs.microsoft.com/help/contribute/samples/process/onboarding?branch=main#add-metadata-to-readme
languages:
- java
page_type: sample
name: "Java Spring Boot web API that protects its own endpoints and calls Microsoft Graph on behalf of the client application."
description: "This Java API that protects its own endpoints using JWT scope validation and calls Microsoft Graph on behalf of the client. The code in this sample is used by one or more articles on docs.microsoft.com."
products:
- azure
- azure-active-directory
- ms-graph
urlFragment: ms-identity-docs-code-webapi-obo-java

---
-->

# Java Spring Boot | web API | access control (protected routes), protected web API access (Microsoft Graph) | Microsoft identity platform

<!-- Build badges here
![Build passing.](https://img.shields.io/badge/build-passing-brightgreen.svg) ![Code coverage.](https://img.shields.io/badge/coverage-100%25-brightgreen.svg) ![License.](https://img.shields.io/badge/license-MIT-green.svg)
-->

This Java Spring Boot web API uses the Microsoft Authentication Library (MSAL) for Java to require authorization for clients accessing a JWT-protected route, and then calls Microsoft Graph on behalf of the client.

```console
$ curl http://localhost:8080/me -H "Authorization: Bearer {valid-access-token}"
{
  "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#users/$entity",
  "businessPhones": ["+1 (999) 5551001"],
  "displayName": "Contoso Employee",
  "givenName": "Contoso",
  "jobTitle": "Worker",
  "mail": "cemployee@contoso.com",
  "mobilePhone": "1 999-555-1001",
  "officeLocation": "Contoso Plaza/F30",
  "preferredLanguage": null,
  "surname": "Employee",
  "userPrincipalName": "contoso_employee@contoso.com",
  "id": "e3a49d8b-d849-48eb-9947-37c1f9589812"
}
```

<!-- TODO: Link to first tutorial in series when published. -->

## Prerequisites

- Azure Active Directory (Azure AD) tenant and the permissions or role required for managing app registrations in the tenant.
- Java 8+
- Maven

## Setup

### 1. Register the app

First, complete the steps in [Configure an application to expose a web API](https://docs.microsoft.com/azure/active-directory/develop/quickstart-configure-app-expose-web-apis) to register the sample API and expose its scopes.

Use these settings in your app registration.

| App registration <br/> setting | Value for this sample app                                            | Notes                                                                            |
|:------------------------------:|:---------------------------------------------------------------------|:---------------------------------------------------------------------------------|
| **Name**                       | `java-api`                                                           | Suggested value for this sample. <br/> You can change the app name at any time.  |
| **Supported account types**    | **Accounts in this organizational directory only (Single tenant)**   | Suggested value for this sample.                                                 |
| **Platform type**              | _None_                                                               | No redirect URI required; don't select a platform.                               |
| **Scopes defined by this API** | **Scope name**: `user_impersonation`<br/>**Who can consent?**: **Admins and users**<br/>**Admin consent display name**: `Act on behalf of the user`<br/>**Admin consent description**: `Allows the API to act on behalf of the user.`<br/>**User consent display name**: `Act on your behalf`<br/>**User consent description**: `Allows the API to act on your behalf.`<br/>**State**: **Enabled** | Required scope for this sample. |
| **

> :information_source: **Bold text** in the tables above matches (or is similar to) a UI element in the Azure portal, while `code formatting` indicates a value you enter into a text box in the Azure portal.

### 2. Update code sample with app registration values

In [_application.yml_](src/main/resources/application.yml), update the following Azure AD configuration properties with the values from your [app's registration in the Azure portal](https://docs.microsoft.com/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).

```yaml
# 'Tenant ID' of your Azure AD instance - this value is a GUID
tenant-id: value-here

# 'Application ID URI' of app registration in Azure portal - this value typically starts with api://
app-id-uri: value-here

# 'Application (client) ID' of app registration in Azure portal - this value is a GUID
client-id: value-here

# Client secret 'Value' (not its ID) from 'Client secrets' in app registration in Azure portal
client-secret: value-here
```

### 3. Install package(s) and compile application

To install Spring Boot and MSAL libraries:

```bash
mvn install
```

## Run the application

```bash
mvn spring-boot:run
```

## Access the API

Using Postman, curl, or a similar application, issue an HTTP GET request to *http://localhost:8080/me* with an `Authorization` header of `Bearer {VALID-ACCESS-TOKEN}`.

For example, if you use curl and everything worked, the sample you should receive a response from the API similar to this:

```console
$ curl http://localhost:8080/me -H "Authorization: Bearer {VALID-ACCESS-TOKEN}"
{
  "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#users/$entity",
  "businessPhones": ["+1 (999) 5551001"],
  "displayName": "Contoso Employee",
  "givenName": "Contoso",
  "jobTitle": "Worker",
  "mail": "cemployee@contoso.com",
  "mobilePhone": "1 999-555-1001",
  "officeLocation": "Contoso Plaza/F30",
  "preferredLanguage": null,
  "surname": "Employee",
  "userPrincipalName": "contoso_employee@contoso.com",
  "id": "e3a49d8b-d849-48eb-9947-37c1f9589812"
}
```

### Generating a valid access token

Follow the instructions in [Gaining consent for the middle-tier application](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow#gaining-consent-for-the-middle-tier-application), including setting the API's app registration manifest value of **knownClientApplications**.

## About the code

This Java API uses the Spring Boot web framework. The app has a single route that requires an access token. The access token will be automatically validated by MSAL:

- A missing or invalid (expired, wrong audience, etc) token will result in a `401` response.
- An otherwise valid token without the proper scope will result in a `403` response.
- A valid token with the proper scope of `user_impersonation` will be accepted, and the token will then be exchanged in an on-behalf-of OAuth flow to call to Microsoft Graph. The results of the Microsoft Graph call are then returned as the results of the API call.

## Reporting problems

### Sample app not working?

If you can't get the sample working, you've checked [Stack Overflow](http://stackoverflow.com/questions/tagged/msal), and you've already searched the issues in this sample's repository, open an issue report the problem.

1. Search the [GitHub issues](../issues) in the repository - your problem might already have been reported or have an answer.
1. Nothing similar? [Open an issue](../issues/new) that clearly explains the problem you're having running the sample app.

### All other issues

> :warning: WARNING: Any issue in this repository _not_ limited to running one of its sample apps will be closed without being addressed.

For all other requests, see [Support and help options for developers | Microsoft identity platform](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options).

## Contributing

If you'd like to contribute to this sample, see [CONTRIBUTING.MD](/CONTRIBUTING.md).

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information, see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
