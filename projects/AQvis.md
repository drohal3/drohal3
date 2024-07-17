# AQvis
Data visualisation web app for IdealAQ.

## Frontend
[ [GitHub repository](https://github.com/drohal3/AQvis-frontend) ]

The frontend is implemented in ReactJS with MaterialUi. The data is visualised in plots provided by [Recharts](https://recharts.org/en-US/).

[//]: # (**Tools:**</br>)

[//]: # ([Vite]&#40;https://vitejs.dev/&#41; build tool </br>)


## Backend
[ [GitHub repository](https://github.com/drohal3/AQvis-backend) ]

The backend is implemented in FastAPI. It provides a simple API handling HTTP requests. Although, GraphQL can be added later if needed.
The data related to the website (user and organisation data) is stored in MongoDB or Amazon DocumentDB. The measurement data is queried from their dedicated storage in AWS. In the early stages, it is DynamoDB. For later, infrastructure utilizing Kinesis Data Streams, Kinesis Firehose and S3 or alternatively storage in time series specialised database Timestream is ready to be provisioned using Terraform and measurement data storage transferred there.

## Deployment
Both, frontend and backend apps are deployed using the same strategy with different steps in their deployment pipelines owing to the fact they use different technology stacks.
The deployment pipelines are defined using GitHub actions. In the pipelines, the linting is checked, tests are run, docker images are built and published to image repository (AWS ECR and/or Docker Hub). After a successful push to an image repository, a notification is sent to a Discord channel.

### AWS solution
[[Github repository](https://github.com/drohal3/AQvis-infra)]

The deployment pipeline also contains parts to trigger a new deployment in AWS ECS with the newest image version/tag.
The applications are running in the 

> **Note:** this solution was put on hold due to its unnecessary complexity and high operational costs. A simpler and more cost-efficient solution (see below) was chosen for time being.

### Dockerdeploy solution
This solution utilizes [dockerdeploy.cloud](https://dockerdeploy.cloud/) service to host the backend and frontend applications. 
It is used temporarily as a cost-efficient solution for the project demonstration purposes.

## Measurement data
[TODO: [Github repository]()]

The measurement data is published by devices as MQTT messages.
Usually, each device published data once a second. The data is handled by IoT Core AWS service, processed and forwarded to temporary or permanent storage.

## Measurement simulation
[[Github repository](https://github.com/drohal3/aws-timeseries-experiment/tree/main/device_simulation)]

For the demonstration and testing purposes a python script using [asyncio](https://docs.python.org/3/library/asyncio.html) to simulate X devices running and publishing data concurrently was created and used.

## Related projects
- **AQrpi**: (TODO: link) Hardware control software for Raspberry Pi. Uses YML config file tha describes used hardware components and based on it reads and calculates measurement data. The components usually use I2C, SPI and serial interfaces.
