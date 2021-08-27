# Automated Deployment of the Star Wars API towards Axway Amplify

This project allows all committed edits of the OAS3 specification to automatically trigger a webhook to the jenkins.axwaydemo.co.uk Jenkins server.

The Jenkinsfile included in this project defines two stages to occur within Axway API Manager:

1. The latest version of the Star Wars API OAS3 specification document is loaded into Axway API Manager as a Backend API
2. The Star Wars API is virtualized in API Manager as a front-end API, using version control of the url path format: /swapi/oas3-version-field, e.g. /swapi/v0.2.0

After this API is virtualized and deployed in API Manager, the Axway Discovery Agent running on axwaydemo.co.uk will discover the API and publish it towards Axway Amplify Unified Catalog.
