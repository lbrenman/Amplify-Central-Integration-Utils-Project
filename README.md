# Amplify Central Integration Utils Project Readme

This [**Amplify Central Integration Utilities**](https://github.com/lbrenman/Amplify-Central-Integration-Utils-Project) project, `AmplifyCentralIntegrationUtils`, contains connectors, data objects and services to aid in creating integrations to Axway Amplify Central and Marketplace using Axway's Amplify Integration.

![](https://i.imgur.com/bXu0h9r.png)

The Connectors are for making API calls back to Amplify using an Amplify [**Service Account**](https://docs.axway.com/bundle/platform-management/page/docs/management_guide/managing_service_accounts/index.html) client Id and secret. You can create a Service Account easily as described [**here**](https://blog.axway.com/product-insights/amplify-platform/axway-amplify-platform-api-calls).

The Services implement common operations such parsing Amplify webhooks and approving subscriptions.

The Data Objects define Amplify resources and webhooks and are used by the Services.

You can import the utils project into your Amplify Integration tenant and then reference the connectors, data objects and services from your Amplify Integration project.

This readme describes the assets in the project and how to use them in your integration.

* You can learn about Amplify Integration [**here**](https://www.axway.com/en/products/amplify-integration)
* You can learn about Amplify Central/Marketplace [**here**](https://www.axway.com/en/products/amplify-enterprise-marketplace)
* You can learn about integrating with Amplify [**here**](https://docs.axway.com/bundle/amplify-central/page/docs/integrate_with_central/index.html)

## Examples

An Examples Project, `AmplifyCentralIntegrationExamples`, includes samples of useful Amplify Central integrations that use the utils project's resources.

Here are some of the examples:
* **Amplify Marketplace Product Subscription Approver/Notifier using MS Teams and Email** - notify approval team of subscription request updates through MS Teams with buttons to approve/reject and notify the Product Team subscription approver via email
* **Amplify Marketplace Product Subscription Approver/Notifier using MS Teams, ServiceNow and Email** - create ServiceNow incident on subscription request and notify approval team of subscription request updates through MS Teams with link to incident for approval/rejection inside of ServiceNow and notify the Product Team subscription approver via email
* **Amplify Central Agent notifier** - for receiving notifications in MS Teams when an Agent is down
* **Amplify API Compliance notifier** - for receiving notifications in MS Teams when a discovered API does not meet your organization's grade criteria for design and/or security
* **Amplify Marketplace Product Review moderated content notifier** - for receiving notifications in MS Teams when a Marketplace Product Review does not meet your organizations criteria for Sentiment, Toxicity and/or PII

These examples are described [**here**](https://gist.github.com/lbrenman/ba9640a5b1650a68c13bb98991090725).

YAML files for creating the webhook and resource hooks in Amplify as described [**here**](https://blog.axway.com/product-insights/amplify-platform/central/create-an-amplify-central-integration-webhook-using-the-axway-cli) are included in the repo.

## Import Project

To use the project's assets you will need to import the `AmplifyCentralIntegrationUtils` project from the Amplify Integration Manager module under Environments.

![](https://i.imgur.com/nLqXQvY.png)
![](https://i.imgur.com/getEd3P.png)

Now you can access the resources that will be in the *AmplifyCentralIntegrationUtils* project
![](https://i.imgur.com/j1Q5dEQ.png)

If you import the example project, `AmplifyCentralIntegrationExamples`,  then you don't need to import the utils project separately as it's included in the project export.

## Connectors

This project includes the following Connections that will aid in accessing data in Amplify:

* *Amplify Central API HTTPS Client* - A connection to the [**Amplify Central APIs**](https://apidocs.axway.com/swagger-ui-NEW/index.html?productname=APIServer&productversion=1.0.0&filename=swagger.json). You will need a client id and secret from an Amplify Service Account with Central Admin role to configure and use this connector.
* *Amplify Platform API HTTPS Client* - A connection to the [**Amplify Platform APIs**](https://platform.axway.com/api-docs.html). You will need a client id and secret from an Amplify Service Account to configure this connector. Depending on the API's and data that you need to access, you may need to add the Service Account to the proper Amplify Team.
* *Webhook Trigger HTTPS Server Basic* - A sample HTTP/S Server connection with Basic authentication for responding to Amplify Webhooks. You will need to configure the Amplify Webhook URL and authentication settings accordingly.

## Data Objects

There are many Data Objects that are used by the included Services but can also be used separately in your integrations.

Here are some:

* Amplify Subscription Webhook - Amplify subscription webhook payload
* Consumer Organizations Payload - Amplify Platform Organization and Team of the Consumer Marketplace payload
* Marketplace - Amplify Central Marketplace Object
* Organization Payload - Amplify Platform Organization payload
* Organization - Amplify Platform Organization object
* Product - Amplify Central Product object
* Product Plan - Amplify Central Product Plan object
* Subscription - Amplify Central Marketplace Product Subscription object
* Subscriptions - Amplify Central Marketplace Product Subscription array object
* Team Payload - Amplify Platform Team payload
* Team Users - Amplify Platform Team Users array object
* User Payload - Amplify Platform User payload
* Users - Amplify Platform User array object

## Services

The Services are the main assets of the utility project. They enable you to more easily create integrations that integrate with Amplify Central and Marketplace by encapsulating operations such as parsing webhook payloads and approving subscriptions without needing to dig into the underlying details.

For example, normally you would need a JSON Parse component and related Data Object (for your webook payload) or a Map component and related Extract variable (for your webhook payload) to parse a incoming Amplify webhook.

![](https://i.imgur.com/2B7rf7M.png)
![](https://i.imgur.com/LvWAc5c.png)

Instead, you can use the *AmplifyWebhookSubscriptionPayloadParser* Service to more easily JSON parse the Amplify Webhook that triggers your integration by just connecting the HTTP/S Server Post body to the Service input as shown below.
![](https://i.imgur.com/79QTWSo.png)
![](https://i.imgur.com/lL8QUk3.png)

When using the utility project you don't need to know the Amplify Webhook payload schema or create data objects or variables as it is exposed by the service for you as shown above.

As another example, in order to approve a Marketplace Product subscription request, you would need to know the API to call and the payload. Instead you can use the *ApproveSubscriptionFromSubscription* Service and simply pass in the subscription object (which is also the body.payload property of a subscription webhook) and two variables to the Service to approve/reject the request.
![](https://i.imgur.com/2boFo1f.png)

A partial list of Services are:
* *AmplifySubscriptionWebhookPayloadParser* - Amplify Subscription webhook payload parser
* *ApproveSubscriptionFromSelfLink* - Approve subscription from a selfLink
* *ApproveSubscriptionFromSubscription* - Approve subscription using a subscription object
* *GetMarketplaceFromSubscription* - Get the full marketplace object from a subscription object
* *GetOrgFromOrgId* - Get organization from an organization id
* *GetPendingSubscriptions* - Get the array of pending subscriptions
* *GetProductFromSubscription* - Get the full product object from a subscription object
* *GetProductPlanFromSubscription* - Get the full Product Plan object from a subscription object
* *GetSubscriptionApproversForProduct* - Get an array of subscription approvers for a Product object
* *GetSubscriptionFromSelfLink* - Get the Subscription from Subscription selfLink
* *isConsumerOrgSubscriptionRequestFromWH* - Checks if a subscription webhook corresponds to a consumer or platform organization subscription request
