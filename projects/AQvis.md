# AQvis - Data visualisation web app for IdealAQ

## Frontend
[ [GitHub repository](https://github.com/drohal3/AQvis-frontend) ]

...

## Backend
[ [GitHub repository](https://github.com/drohal3/AQvis-backend) ]

The backend is implemented in FastAPI. It provides a simple API handling HTTP requests. Although, GraphQL can be added later if needed.
The data related to the website (user and organisation data) is stored in MongoDB or Amazon DocumentDB. The measurement data is queried from their dedicated storage in AWS. In the early stages, it is DynamoDB. For later, infrastructure utilizing Kinesis Data Streams, Kinesis Firehose and S3 or alternatively storage in time series specialised database Timestream is ready to be provisioned using Terraform and measurement data storage transferred there.

## Hosting and deployment
Both, frontend and backend apps are deployed using the same strategy with different steps in their CI/CD pipeline owing to the fact they use different technology stacks.

The deployment pipelines are defined using GitHub actions. In the pipeline, the linting is checked, tests are run, docker images are built and published to image repository (AWS ECR and/or Docker Hub). After a successful push to an image repository, a notification is sent to a Discord channel using a webhook.
The deployment pipeline also contains parts to trigger a new deployment in AWS ECS with the newest image version/tag. However, at the time of writing, the deployment in ECS is done manually to save the resources as the apps are not yet needed to be running all the time.

## Measurement data
The measurement data are published by devices as MQTT messages. The data are measured and sent in one second intervals. In the AWS, IoT Core service is used to subscribed to the topic to which the MQTT messages are published and data is passed to the relevant storage. 

