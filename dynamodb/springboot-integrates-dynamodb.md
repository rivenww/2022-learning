# Springboot Integrates  DynamoDB

P.S. My project: [https://github.com/wrw1997/DynamoDB-Demo](https://github.com/wrw1997/DynamoDB-Demo)

The AWS doc mentions the application of the SDK, so firstly we should introduce the SDK as a maven dependency.

```
<dependency>
    <groupId>com.amazonaws</groupId>
    <artifactId>aws-java-sdk-dynamodb</artifactId>
    <version>1.12.221</version>
</dependency>
```

Then write entity class and repository class. There isn't much difference with relational database, but we need to annotate the attribute with @DynamoDBAttribute.

## IAM and configuration in Springboot

Though we have already created table, we still cannot access the DynamoDB directly in spring.

Use IAM (in AWS console) to create a new group with the access to DB(AmazonDynamoDBFullAccess), and add a user into the group. Copy the keys to the method and annotate with @Bean then we can get a instantiated DynamoDBMapper in runtime.

P.S. It's better to store the keys in configuration file, not hardcore.

```
    @Bean
    public DynamoDBMapper dynamoDBMapper() {
        return new DynamoDBMapper(buildDynamoDB());
    }

    private AmazonDynamoDB buildDynamoDB() {
        return AmazonDynamoDBClientBuilder
                .standard()
                .withEndpointConfiguration(
                        new AwsClientBuilder.EndpointConfiguration(
                                "dynamodb.us-east-2.amazonaws.com",
                                "us-east-2"
                        )
                )
                .withCredentials(
                        new AWSStaticCredentialsProvider(
                                new BasicAWSCredentials(
                                        "key",
                                        "secret key"
                                )
                        )
                )
                .build();
    }
```

The rest is to implement the operation of accessing the table through the mapper's api and write the controller.
