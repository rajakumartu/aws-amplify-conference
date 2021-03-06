# AWS Amplify & AppSync Conference - Reference App

[![amplifybutton](https://oneclick.amplifyapp.com/button.svg)](https://console.aws.amazon.com/amplify/home#/deploy?repo=https://github.com/ksivamuthu/aws-amplify-conference)

This sample reference application shows how to use GraphQL to build an application that a user can login to the system, see the conference, sessions and speaker details. The sample is written in React and uses AWS AppSync, Amazon Cognito, Amazon DynamoDB, Amazon PinPoint and Amazon S3 as well as the Amplify CLI.

This tweet is my reaction when I started exploring AWS AppSync. I'm impressed on how AWS Amplify & Appsync evolved over an year. Integration with backend and client libraries are awesome.

<a href="https://twitter.com/ksivamuthu/status/1021208992377425920" ><img src="docs/twitter_1021208992377425920.png"><img></a>


## Table of Contents <!-- omit in toc -->
- [AWS Amplify & AppSync Conference - Reference App](#AWS-Amplify--AppSync-Conference---Reference-App)
  - [Slides](#Slides)
  - [Topics](#Topics)
  - [Demo Features](#Demo-Features)
  - [Prerequisites](#Prerequisites)
  - [Getting Started](#Getting-Started)
  - [GraphQL Schema & Transforms](#GraphQL-Schema--Transforms)
  - [How to run React Web App Project?](#How-to-run-React-Web-App-Project)
  - [Contributing](#Contributing)
  - [License](#License)


## Slides

Slides are here - https://slides.com/sivamuthukumar/building-serverless-graphql-using-aws-amplify/fullscreen

## Topics

* [AWS AppSync](https://aws.amazon.com/appsync/)
* [AWS Amplify CLI](https://aws-amplify.github.io)
* [AWS Amplify Client Libraries](https://aws-amplify.github.io/docs/js/start?ref=amplify-js-btn&platform=purejs)
* [AWS Amplify GraphQL Transforms](https://aws-amplify.github.io/docs/cli/graphql?sdk=js)

## Demo Features

- [x] Authentication
- [ ] Authorization
- [x] GraphQL Query
- [x] GraphQL Mutation
- [x] GraphQL Subscription
- [x] Elastic Search 
- [ ] Search Functionality in Client
- [x] Lambda Functions Resources
- [x] DynamoDB Resource
- [ ] Chatbot


## Prerequisites

+ [AWS Account](https://aws.amazon.com/mobile/details/)

+ [NodeJS](https://nodejs.org/en/download/) with [NPM](https://docs.npmjs.com/getting-started/installing-node)

+ [AWS Ampify CLI](https://aws-amplify.github.io/)
  - `npm install -g @aws-amplify/cli`
  - `amplify configure` 

## Getting Started

1. Clone this repo locally.

```
$ git clone https://github.com/ksivamuthu/aws-amplify-conference.git
cd aws-amplify-conference
```

2. Initialize the amplify project.

```
$ amplify init
```

3. Push the amplify resources into cloud

```bash
$ amplify push

Current Environment: dev

| Category  | Resource name                | Operation | Provider plugin   |
| --------- | ---------------------------- | --------- | ----------------- |
| Auth      | awsamplifyconferenceee829563 | Create    | awscloudformation |
| Hosting   | S3AndCloudFront              | Create    | awscloudformation |
| Api       | amplifyconferencegraphql     | Create    | awscloudformation |
| Analytics | amplifyconferenceanalytics   | Create    | awscloudformation |
| Storage   | amplifyconferences3          | Create    | awscloudformation |
| Function  | speakerfacts                 | Create    | awscloudformation |
? Are you sure you want to continue? Yes
```

This command will create the all backend resources required for projects.

| Category  | Resource name                | Operation | Provider plugin   |
| --------- | ---------------------------- | --------- | ----------------- |
| Auth      | awsamplifyconferenceee829563 | Create    | awscloudformation |
| Hosting   | S3AndCloudFront              | Create    | awscloudformation |
| Api       | amplifyconferencegraphql     | Create    | awscloudformation |
| Analytics | amplifyconferenceanalytics   | Create    | awscloudformation |
| Storage   | amplifyconferences3          | Create    | awscloudformation |
| Function  | speakerfacts                 | Create    | awscloudformation |

## GraphQL Schema & Transforms

The GraphQL Schema file is located at - [amplify/backend/api/amplifyconferencegraphql/schema.graphql](amplify/backend/api/amplifyconferencegraphql/schema.graphql)

Read about [GraphQL Transforms](https://aws-amplify.github.io/docs/cli/graphql) on @model, @searchable, @function directives

```graphql
type Conference @model {
  id: ID!
  title: String!
  year: Int!
  websiteUrl: String!
  location: String!
  contact: String!
  sessions: [Session]! @connection(name: "ConferenceSessions")
}

type Session @model @searchable {
  id: ID!
  title: String!
  abstract: String!
  level: Level!
  category: Category!
  keywords: [String]!
  favorites: [UserSessionVote]! @connection(name: "UserSessionVotes")
  conference: Conference! @connection(name: "ConferenceSessions")
  speaker: Speaker! @connection(name: "SessionSpeakers")
}

type Speaker @model {
  id: ID!
  name: String!
  bio: String!
  facts: String @function(name: "speakerfacts-${env}")
  jobtitle: String!
  phonenumber: String!
  companyname: String!
  companywebsite: String!
  blog: String!
  website: String!
  twitter: String!
  facebook: String!
  linkedin: String!
  sessions: [Session]! @connection(name: "SessionSpeakers")
}

type UserSessionVote @model {
  id: ID!
  userId: String!
  session: Session! @connection(name: "UserSessionVotes")
}

enum Level {
  BEGINNER
  INTERMEDIATE
  ADVANCED
}

enum Category {
  GeneralDiscussion
  ClientDevelopment
  WebDevelopment
  DatabaseDevelopment
  CloudDevelopment
  Design
  ProfessionalDevelopment
  CareerAdvancement
  ITTopics
}
```

## How to run React Web App Project?

1. Navigate into react-web-app project
  ```
  cd react-web-app
  ```

2. Install client dependencies.
  ```
  $ npm install
  ```

3. Run the react application
  ```
  $ npm run start
  ```

4. To deploy into S3 hosting
  ```
  $ amplify publish
  ```

The sample uses [AWS Amplify](https://github.com/aws/aws-amplify) to perform the Sign-Up and Sign-In flows with a Higher Order Component and its GraphQL calls.

## Contributing

Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are greatly appreciated.

* Fork the Project
* Create your Feature Branch (git checkout -b feature/AmazingFeature)
* Commit your Changes (git commit -m 'Add some AmazingFeature)
* Push to the Branch (git push origin feature/AmazingFeature)
* Open a Pull Request
  
## License

Distributed under the MIT License. See LICENSE for more information.
