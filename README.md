# appsync-access-control
System Architecture for Access Control using AppSync and Cognito

## Introduction

This document provides a comprehensive overview of an access control solution designed for web and mobile applications. The access control system encompasses both authentication and authorization, crucial elements in ensuring the security and functionality of applications.

### Authentication and Authorization

Authentication serves the fundamental purpose of validating a user's identity, confirming that they are who they claim to be. On the other hand, authorization is the process of restricting or granting user actions based on their roles and permissions within the system. For instance, an administrative user may have the authority to manage orders, while a regular customer is limited in their actions. These components play a pivotal role in safeguarding a system against unauthorized access and misuse of functionalities.

## TL;DR;

![Access control diagram](https://github.com/MatthewFay/appsync-access-control/blob/main/access_control_appsync.svg)

* AppSync is used as the API gateway, i.e. the primary entry point for clients to interact with the system.
* Cognito is used as an identity/user store. Users, passwords, etc. are stored here.
* Amplify SDK is used by the frontend clients to authenticate and communicate with AppSync (e.g., query or mutate data).
* Authorization is handled by AppSync and Cognito. Specific actions and even data fields can be restricted to designated users, ensuring a finely tuned access control system.
* Cost-effective pay-as-you-go model.

## Rationale

### AppSync

AppSync is a managed, serverless GraphQL service provided by AWS. GraphQL has many benefits over REST, such as reducing under and over-fetching of data. The client can request exactly the data it needs from the backend, and even have it returned in the way it wants, so it's flexible and powerful. AppSync supports subscriptions, so you can push data async to the client over a WebSocket, as data changes. This allows you to support many use cases (e.g., chat, real-time data updates) and improves the UX of apps. AppSync supports auto-scaling and high-availability. AppSync integrates very well with Amazon Cognito for authorization purposes. Additionally, both AppSync and Cognito integrate very well with AWS Amplify for authentication and communicating with the AppSync/GraphQL API.

#### Pros

1. Real-time data capabilities
2. Flexible data sources (HTTP, DynamoDB, etc.)
3. GraphQL benefits (e.g., under/over fetching of data)
4. Offline support
5. Authentication and authorization with Cognito
6. Managed service (auto patching, scaling, high availability, monitoring, etc.)

#### Cons

1. Potential GraphQL learning curve
2. Vendor lock-in: Using AppSync ties you to the AWS ecosystem
3. AWS Service availability: As with any cloud service, service availability and downtime can impact your application
4. Customization: While AppSync offers a lot of functionality out of the box, there might be scenarios where you need more customization or control over certain aspects of the GraphQL server

### Cognito

Cognito provides a fully managed service for user authentication, registration, and user profile management. It supports multiple identity providers, including social identity providers, and integrates with existing corporate directories. Cognito scales automatically to handle a large number of users, making it suitable for applications with varying user bases. It also includes features such as multi-factor authentication (MFA) and adaptive authentication to enhance security (for example, blocking suspicious activity). Cognito User Pools manage user identities, while Identity Pools provide temporary AWS credentials for access to other AWS services (e.g., S3). This separation of concerns enhances security and flexibility. As mentioned previously, Cognito and AppSync integrate very well together for authentication and authorization purposes.

Combining Cognito and AppSync gives many benefits:

1. Unified authentication and authorization
2. Secure GraphQL API
3. Integration with other AWS services
4. Simplified user management
5. Automatic scaling (cost-effective as scales on demand)

#### Pros

1. Fully managed user identity service with many features
2. Scalability
3. Flexibility in authentication methods (e.g., social identity providers)
4. MFA - very important from a security perspective
5. Integration with other AWS services
6. User pools and identity pools allow for more secure and modular approach
7. Supports custom authentication flows
8. Compliance: Cognito is designed to meet various compliance regulations, and it provides features and capabilities to help customers build applications that adhere to regulatory requirements

#### Cons

1. Learning curve
2. Limited customization for default UI - may require custom UI development
3. Dependency on AWS ecosystem (i.e. vendor lock-in)
4. Cognito's pricing model can be complex, considering factors such as the number of users, active users, and the usage of certain features
5. While Cognito provides extensive features, there might be scenarios where advanced customization is needed, and the service may have limitations in accommodating highly specific use cases
6. User migration challenges
7. Some Cognito configurations at the user pool level cannot be changed later, which could create friction

## Cognito vs. Alternatives

### Cognito

#### Licensing

AWS Cognito is part of the AWS ecosystem and follows a pay-as-you-go pricing model. Users pay for the specific features they use, such as authentication, user pool, and identity pool usage.

#### Support

AWS provides various support plans, including Basic, Developer, Business, and Enterprise support. AWS Support covers a wide range of services, including Cognito, and offers different levels of assistance. AWS Support plans vary in cost, and the level of support needed will impact pricing. As with Auth0, enterprise-level support tends to be more expensive.

#### Pricing model

AWS Cognito pricing is based on factors like monthly active users, data storage, and data transfer. Users are billed separately for the various components they use, such as user pools, identity pools, and multi-factor authentication.

#### Estimated Cost

For 1 million MAU, the total cost per month might be in the tens of thousands of dollars, depending on usage patterns and support level.

### Auth0

#### Licensing

Auth0 typically operates on a subscription-based pricing model. The cost depends on factors such as the number of active users, features used, and the level of support required. For 1 million MAU, you would fall into a higher pricing tier. Exact pricing depends on features used.

#### Support

Auth0 provides various support plans, including community support, developer support, and enterprise support. The support plans come with different service level agreements (SLAs) and response times. Auth0's support plans vary in cost, and the level of support needed will affect pricing. Enterprise-level support tends to be more expensive.

#### Pricing model

Auth0's pricing model is based on monthly or annual subscriptions. Pricing tiers often include features such as social logins, multi-factor authentication, and custom domains.

#### Estimated Cost

For 1 million MAU: currently unknown. Auth0 does not provide a pricing calculator. To get an estimate, it would require contacting their sales team.

### In-House Solution

#### Licensing

Licensing for an in-house solution depends on the specific software used. Many open-source solutions are released under licenses such as MIT, Apache, or GPL, allowing for flexibility in usage. Open-source solutions often have no licensing costs, but there are development and maintenance costs associated with building and maintaining an in-house solution.

#### Support

Support for an in-house solution will often involve an internal development team. Support for open-source software typically relies on community forums, user groups, and documentation. Costs could vary widely based on the level of support needed.

#### Pricing model

The implementation cost for an in-house solution involves factors like development time, personnel costs, and infrastructure expenses. It doesn't have a per-user licensing cost but might incur ongoing maintenance and support costs.

#### Estimated Cost

For 1 million MAU, development and infrastructure costs for an in-house solution can vary significantly based on the complexity of the project, features required, and ongoing maintenance needs.

Detailed cost comparisons would depend on the specific requirements, user base, and features needed for a particular project.


