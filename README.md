# spring-kafka-streams-funtional-programming

This is the repo for learning the spring cloud stream for kafka streams. 

Behind the scenes, the kafka stream binder for spring cloud stream will convert this into a proper kafka streams application with a **StreamBuilder**, 

One of the prime tenets for spring cloud stream is to hide the complexity and boilerplate away from the user so that the application developer can focus on the 
bussiness logic. 

Binder will take care of creating the KAFKA STREAM topology, connecting to a kafka cluster, binding to a topic and consuming data from that 
topic which is bound as KStream in this case. 

Kafka-Stream requires 
1. Cluster information
    ```text
   
    By default the binder will try to connect a cluster that is running on localhost:9092.
    
   ```
1. Application id

```java
@Bean
    public java.util.function.Consumer<KStream<String, String>> process() {
        return input -> input.foreach((key, value) -> {
            System.out.println("Key::::" + key + "----> Value ::::" + value);
        });
    }
```

```java

In a Kafka Streams application, application.id is a mandatory field. Without it, you cannot start a Kafka Streams application. 

By default, the binder will generate an application ID and assign it to the processor.


It uses the function bean name as a prefix. For e.g, if you have a consumer as above, the binder will generate the application ID as process-applicationId. 
```

1. topic to consume

```text

spring.cloud.stream.bindings.process-in-0.destination: my-input-topic

```
 
1. Serde to use 

```java




Kafka Streams uses a special class called Serde to deal with data marshaling. It is essentially a wrapper around a deserializer on the inbound and a serializer on the outbound. 

Binder, however, infers this information by using the parametric types provided as part of Kafka Streams. For example, in the case of KStream<String, String>, the binder assumes that it needs to use String deserializers.










```



# Scenario - 1: Single input and output binding

From Spring perspective the binding is not mapped to a single TOPIC.

That means a single binding can be applied to multiple topics. 

##### topics could be multiplexed and attached to a single input binding (with comma-separated multiple topics configured on a single binding



Ex: spring.cloud.stream.bindings.wordcount-in-0.destination=words

the above setting or configuration can be modified to multiple topics as well.

**spring.cloud.stream.bindings.wordcount-in-0.destination=words1,words2,word3**

The output stream is configured as follows: spring.cloud.stream.bindings.wordcount-out-0.destination=counts




# Scenario 2: Multiple output bindings through Kafka Streams branching


