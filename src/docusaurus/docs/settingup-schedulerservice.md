import PartialExample from './_elevate.mdx';

# Scheduler Service

<PartialExample elevate /> scheduler services can be set up in local in one of the following ways:

* As a [dockerized service with local dependencies (Intermediate)](#a-dockerized-service-with-local-dependencies)

* As a [local Service with local dependencies (Hardest)](#b-local-service-with-local-dependencies)

## A. Dockerized Service With Local Dependencies

**Expectation**: Run single docker containerized service with existing local (in host) or remote dependencies.

### Local Dependencies Steps

1. Update dependency (Mongo, Kafka etc) IP addresses in .env with "**host.docker.internal**".

    For example:

    ```
     #MongoDb Connectivity Url
     MONGODB_URL = mongodb://host.docker.internal:27017/elevate-scheduler

     #Kafka Host Server URL
     KAFKA_URL = host.docker.external:9092
    ```

2. Find **host.docker.internal** IP address and added it to **mongod.conf** file in host.

    For example: If **host.docker.internal** is **172.17.0.1**,
    **mongod.conf:**

    ```
    # network interfaces
    net:
        port: 27017
        bindIp: "127.0.0.1,172.17.0.1"
    ```

    > :::note
    > Steps to find **host.docker.internal** IP address and location of **mongod.conf** is operating system specific. Refer this [Stack Overflow discussion thread](https://stackoverflow.com/questions/22944631/how-to-get-the-ip-address-of-the-docker-host-from-inside-a-docker-container) for more information.

3. Build the docker image.
    ```
    /ELEVATE/scheduler$ docker build -t elevate/scheduler:1.0 .
    ```
4. Run the docker container.

    - For Mac and Windows with docker v18.03+:

        ```
        $ docker run --name user elevate/scheduler:1.0
        ```

    - For Linux:
        ```
        $ docker run --name user --add-host=host.docker.internal:host-gateway elevate/scheduler:1.0`
        ```
        Refer this [Stack Overflow discussion thread](https://stackoverflow.com/a/24326540) for more information.

### Remote Dependencies Steps

1. Update dependency (such as Mongo, Kafka etc) Ip addresses in .env with respective remote server IPs.

    For Example:

    ```
     #MongoDb Connectivity Url
     MONGODB_URL = mongodb://10.1.2.34:27017/elevate-scheduler

     #Kafka Host Server URL
     KAFKA_URL = 11.2.3.45:9092
    ```

2. Add Bind IP to **mongod.conf** in host:

    Follow instructions given [here](https://www.digitalocean.com/community/tutorials/how-to-configure-remote-access-for-mongodb-on-ubuntu-20-04).

    > :::note
    > Instructions might differ based on MongoDB version and operating system.

3. Build the docker image.
    ```
    /ELEVATE/scheduler$ docker build -t elevate/scheduler:1.0 .
    ```
4. Run the docker container.

    ```
    $ docker run --name scheduler elevate/scheduler:1.0
    ```

## B. Local Service With Local Dependencies

**Expectation**: Run single service with existing local dependencies in host (**Non-Docker Implementation**).

### Steps

1. Install the required tools and dependencies such as:

    * Install any IDE (for example: Visual Studio Code)

    * [Install Nodejs](https://nodejs.org/en/download/)

    * [Install MongoDB](https://docs.mongodb.com/manual/installation/)
    
    * [Install Robo-3T](https://robomongo.org/)

2. Clone the **Scheduler service** repository.

    ```
    git clone https://github.com/ELEVATE-Project/scheduler.git
    ```

3. Add **.env** file to the project directory.

    Create a **.env** file in **src** directory of the project and copy these environment variables into it.

    ```
    #Scheduler Service Config

    #Application Base url
    APPLICATION_BASE_URL = /scheduler/

    # Kafka hosted server url
    KAFKA_URL = localhost:9092

    # Kafka topic to push notification data
    NOTIFICATION_KAFKA_TOPIC = 'notificationtopic'

    # MONGODB_URL
    MONGODB_URL = mongodb://localhost:27017/tl-cron-rest

    # App running port
    APPLICATION_PORT = 4000

    # Api doc url
    API_DOC_URL = '/api-doc'
    ```

4. Start MongoDB locally.

    Based on your host operating system and method used, start MongoDB.

5. Install Npm packages.

    ```
    ELEVATE/scheduler/src$ npm install
    ```

6. Start Scheduler server.

    ```
    ELEVATE/scheduler/src$ npm start
    ```

## API Documentation link

https://elevate-apis.shikshalokam.org/scheduler/api-doc

## Mentoring Services

https://github.com/ELEVATE-Project/mentoring.git

## User Services

https://github.com/ELEVATE-Project/user.git

## Notification Services

https://github.com/ELEVATE-Project/notification.git