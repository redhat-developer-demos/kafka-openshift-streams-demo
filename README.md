# kafka-openshift-streams-demo
A project that demonstrates how to run the Apache Kafka Client against a Kafka broker running under Red Hat OpenShift Streams for Apache Kafka

## What you need to know before you start

* Java programming with Maven
* Access to the Red Hat OpenShift Console
* Basic understanding of message production and consumption under Kafka

## Setting up a Kafka instance and a Kafka topic in Red Hat Open Shift
TO BE provided

## Configuring the .env file
This project uses [`java-dotenv`](https://github.com/cdimascio/java-dotenv) to emulate environment variables and their values a runtime.

`java-dotenv` is already installed in the project's `pom.xml` file.

However, you will need to add a file named `.env` to the root of the project.

The structure of the `.env` file is as follows:

```
DOTENV_TEST_MESSAGE=PING
KAFKA_STREAM_TEST_TOPIC=<TOPIC_NAME_ON_KAFKA_STREAMS>
KAFKA_STREAM_USER_NAME=<SERVICE_ACCOUNT_CLIENT_ID>
KAFKA_STREAM_PWD=<SERVICE_ACCOUNT_CLIENT_SECRET>
KAFKA_STREAM_BOOTSTRAP_SERVER=<KAFKA_STREAMS_BOOTSTRAP_SERVER_URL>
```

**WHERE**

* `<TOPIC_NAME_ON_KAFKA_STREAMS>` is the name of topic where messages will be produced to and consumed from. The topic must be created within in Red Hat OpenShift Kafka Streams.
* `<SERVICE_ACCOUNT_CLIENT_ID>` is the Client ID created when you created the OpenShift Service Account bound to the stream.
* `<SERVICE_ACCOUNT_CLIENT_SECRET>` is the Client Secret created when you created the OpenShift Service Account bound to the stream.
* `<KAFKA_STREAMS_BOOTSTRAP_SERVER_URL>` is the URL of the Kafka Bootstrap which is created when you create the instance of OpenShift Kafka Streams.

**EXAMPLE:**

```properties
DOTENV_TEST_MESSAGE=PING
KAFKA_STREAM_TEST_TOPIC=mytopic
KAFKA_STREAM_USER_NAME=srvc-acct-7axxx530-e8fe-471c-bb3d-aa005020d911
KAFKA_STREAM_PWD=6823f7bd-649b-4029-8a1c-8713axxx918b
KAFKA_STREAM_BOOTSTRAP_SERVER=my-first-k-c-skxxxt--m--nhc-fna.bf2.kafka.rhcloud.com:443
```

You will substitute the values between the `<...>` brackets with values particular to your instance of OpenShift Kafka Streams.

## Building the code
TO BE PROVIDED

## Running the tests

Run the following command from the top level of the directory in which you installed the demonstration code.

`mvn test`

Upon success, you'll get output similar to the following:

```console
Results :

Tests run: 4, Failures: 0, Errors: 0, Skipped: 0

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  19.510 s
[INFO] Finished at: 2022-05-09T19:08:02Z
[INFO] ------------------------------------------------------------------------
```

