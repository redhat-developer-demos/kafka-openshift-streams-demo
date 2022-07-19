# kafka-openshift-streams-demo
A project that demonstrates how to run the Apache Kafka Client against a Kafka broker running under Red Hat OpenShift Streams for Apache Kafka

## What you need to know before you start

* Java programming with Maven
* Access to the Red Hat OpenShift Console
* Basic understanding of message production and consumption under Kafka

## Setting up a Kafka instance and a Kafka topic in Red Hat Open Shift
The first thing you will need to do is build a running instance of Kafka Streams on Red Hat OpenShift.

Follow the directions described in the interactive QuickStart document, [Getting started with Red Hat OpenShift Streams for Apache](https://console.redhat.com/beta/application-services/learning-resources?quickstart=getting-started) in order to get an instance of Kafka Streams running in OpenShift.

You will need retrieve four fields of information that will be created when you get OpenShift Kafka Streams up and running. The information you will need is:

* The Kafka **topic** name that you created in Open Shift
* The **URL** of the Kafka Bootstrap Server
* The **Client ID** associated with the Service Account you will bind to the Kafka instance you created in OpenShift
* The **Client Secret** associated with the Service Account you will bind to the Kafka instance you created in OpenShift.

The Quickstart instructions do a good job of making it very clear as to when these fields are created. You'll want to copy the fields into a text editor so that you can paste them into the `.env` file you'll create the emulate environment variables and their values.

## Configuring the .env file
This project uses [`java-dotenv`](https://github.com/cdimascio/java-dotenv) that emulates environment variables and their values a runtime.

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

## Configuring a secure connection to OpenShift Streams
Getting the Java client to connect to OpenShift Stream requires special configuration settings for creating a secure connection. These configuration settings, which use the Simple Authentication and Security Layer (SASL) standard, are as follows (each setting is linked to a web page that describes the details of the setting):

* [security.protocol](https://kafka.apache.org/24/javadoc/org/apache/kafka/common/security/auth/SecurityProtocol.html)
* [sasl.mechanism](https://www.iana.org/assignments/sasl-mechanisms/sasl-mechanisms.xhtml)
* [sasl.jass.config](https://www.iana.org/assignments/sasl-mechanisms/sasl-mechanisms.xhtml)

The demonstration application assign values to the settings as follows:

* `security.protocol="SASL_SSL"`
* `sasl.mechanism="PLAIN"`
* `sasl.jaas.config='org.apache.kafka.common.security.plain.PlainLoginModule required username="<USERNAME>" password="<PASSWORD>"'`

Essentially, the settings mean that the connection will use the SASL_SSL security protocol and that the SASL security mechanism will be PLAIN. The PLAIN security mechanism simplifies configuration for the purposes of this demonstration by letting you submit the username/password credentials as plain text. However, the connection string is submitted confidentially by lower level encryption as specified in [RFC4616](https://www.rfc-editor.org/rfc/rfc4616.html).

Since this article's demonstration application is dedicated to connecting to an instance of Kafka running remotely under OpenShift Streams, the required configuration settings are put directly into the code, in the addSecurityProperties method. The method is located in the `src/main/java/com/demo/kafka/PropertiesHelper.java` file of the demonstration application’s source code.

Take a look at the [code](./src/main/java/com/demo/kafka/PropertiesHelper.java) so see how configuration is implemented. Notice that the values for the username and password are stored in the `KAFKA_STREAM_USER_NAME` and `KAFKA_STREAM_PWD` environment variables, respectively. Hence, the connection information is protected from intrusions into the server’s file system:
## Running the tests

Make sure that you've properly configured the `.env` file with the values that are particular to your running instance of OpenShift Kafka Streams.

Then, run the following command from the top level of the directory in which you installed the demonstration code.

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

