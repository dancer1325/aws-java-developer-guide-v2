# What's different between the AWS SDK for Java 1\.x and 2\.x<a name="migration-whats-different"></a>

## Package name change<a name="mig-diff-package-name-change"></a>

* package name change
  * `com.amazonaws` -- must be replaced with -- `software.amazon.awssdk`
    * except to. work with server\-side AWS Lambda -- check [Set up an Apache Maven project](setup-project-maven.md#modules-dependencies)   
  * -> Maven's groupId has changed
* package names | SDK 1\.x / name `*.v2*` == code can work with SDK 2\.x 
  * _Example:_ `com.amazonaws.services.dynamodbv2`, `com.amazonaws.retry.v2`, `com.amazonaws.services.apigatewayv2`, `com.amazonaws.services.simpleemailv2`

## High\-level libraries<a name="highlevel-libraries"></a>

* 👁️SOME high\-level libraries are NOT yet available | version 2\.x\ 👁️
  * [changelog to check libraries release](https://github.com/aws/aws-sdk-java-v2/blob/master/docs/LaunchChangelog.md#7-high-level-libraries)
  * _Example:_ Amazon SQS Client\-side Buffering
  * if your application -- depends on -- these libraries -> configure your `pom.xml` to use both 1\.x and 2\.x\
    * [Using both SDKs side\-by\-side](migration-side-by-side.md)

## Adding version 2\.x to your project<a name="adding-v2"></a>

* Maven is the recommended way
  * _Example:_  

    ```
    <dependencyManagement>
        <dependencies>
            <dependency>
              <groupId>software.amazon.awssdk</groupId>
              <artifactId>bom</artifactId>
              <version>2.16.1</version>
              <type>pom</type>
              <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    
    <dependencies>
        <dependency>
          <groupId>software.amazon.awssdk</groupId>
          <artifactId>dynamodb</artifactId>
        </dependency>
    </dependencies>
    ```

## Client builders<a name="client-builder"></a>

* way to create ALL clients
  * Reason: 🧠Constructors are NO longer available 🧠

* _Example:_ 
  * | version 1\.x  

    ```
    AmazonDynamoDB ddbClient = AmazonDynamoDBClientBuilder.defaultClient();
    AmazonDynamoDBClient ddbClient = new AmazonDynamoDBClient();
    ```

  * | version 2\.x  

    ```
    DynamoDbClient ddbClient = DynamoDbClient.create();
    DynamoDbClient ddbClient = DynamoDbClient.builder().build();
    ```

## Client Configuration<a name="client-configuration"></a>

* | 1\.x
  * `ClientConfiguration`
* | 2\.x
  * client configuration -- is split into -- SEPARATE configuration classes
    * -> different HTTP clients (_Example:_ async, synchronous clients) can use SAME `ClientOverrideConfiguration` class
* _Examples:_
  * | 1\.x  

    ```
    AmazonDynamoDBClientBuilder.standard()
    .withClientConfiguration(clientConfiguration)
    .build()
    ```

  * synchronous client configuration | version 2\.x  

    ```
    ProxyConfiguration.Builder proxyConfig = ProxyConfiguration.builder();
    
    ApacheHttpClient.Builder httpClientBuilder =
            ApacheHttpClient.builder()
                            .proxyConfiguration(proxyConfig.build());
    
    ClientOverrideConfiguration.Builder overrideConfig =
            ClientOverrideConfiguration.builder();
    
    DynamoDbClient client =
            DynamoDbClient.builder()
                          .httpClientBuilder(httpClientBuilder)
                          .overrideConfiguration(overrideConfig.build())
                          .build();
    ```

  * asynchronous client configuration | version 2\.x  

    ```
    NettyNioAsyncHttpClient.Builder httpClientBuilder =
            NettyNioAsyncHttpClient.builder();
    
    ClientOverrideConfiguration.Builder overrideConfig =
            ClientOverrideConfiguration.builder();
    
    ClientAsyncConfiguration.Builder asyncConfig =
            ClientAsyncConfiguration.builder();
    
    DynamoDbAsyncClient client =
            DynamoDbAsyncClient.builder()
                               .httpClientBuilder(httpClientBuilder)
                               .overrideConfiguration(overrideConfig.build())
                               .asyncConfiguration(asyncConfig.build())
                               .build();
    ```
* check [changelog with the mapping](https://github.com/aws/aws-sdk-java-v2/blob/master/docs/LaunchChangelog.md#13-sdk-client-configuration)

## Setter Methods<a name="setter-methods"></a>

* | 2\.x, setter method does NOT include prefixes
  * `set`
  * `with`
    * _Example:_ `*.withEndpoint()` -- is replaced with -- `*.endpoint()`
* _Example:_
  * | 1\.x  

    ```
    AmazonDynamoDB client = AmazonDynamoDBClientBuilder.standard()
                    .withRegion("us-east-1")
                    .build();
    ```

  * | 2\.x  

    ```
    DynamoDbClient client = DynamoDbClient.builder()
                    .region(Region.US_EAST_1)
                    .build();
    ```

## Class Names<a name="class-names"></a>

* ALL client class names -- are NOW aligned with -- names / used | AWS CLI
  * fully camel cased
  * NO longer prefixed by `Amazon`
  * [full list of client name changes](https://github.com/aws/aws-sdk-java-v2/blob/master/docs/LaunchChangelog.md#63-client-names)
* _Examples:_
  * | 1\.x  

    ```
    AmazonDynamoDB
    AWSACMPCAAsyncClient
    ```

  * | 2\.x**  

    ```
    DynamoDbClient
    AcmAsyncClient
    ```

## Region class<a name="region-class"></a>

* TODO: The AWS SDK for Java version 1\.x had multiple `Region` and `Regions` classes, both in the core package and in many of the service packages\. `Region` and `Regions` classes in version 2\.x are now collapsed into one core class, `Region`\.

**Example Region and Regions classes in 1\.x**  

```
com.amazonaws.regions.Region
com.amazonaws.regions.Regions
com.amazonaws.services.ec2.model.Region
```

**Example Region class in 2\.x**  

```
software.amazon.awssdk.regions.Region
```

For more details about changes related to using the `Region` class, see [Region class name changes](migration-client-region.md)\.

## Immutable POJOs<a name="immutable-classes"></a>

Clients and operation request and response objects are now immutable and cannot be changed after creation\. To reuse a request or response variable, you must build a new object to assign to it\.

**Example of updating a request object in 1\.x**  

```
DescribeAlarmsRequest request = new DescribeAlarmsRequest();
DescribeAlarmsResult response = cw.describeAlarms(request);

request.setNextToken(response.getNextToken());
```

**Example of updating a request object in 2\.x**  

```
DescribeAlarmsRequest request = DescribeAlarmsRequest.builder().build();
DescribeAlarmsResponse response = cw.describeAlarms(request);

request = DescribeAlarmsRequest.builder()
        .nextToken(response.nextToken())
        .build();
```

## Streaming operations<a name="streaming-operations"></a>

Streaming operations such as the Amazon S3 `getObject` and `putObject` methods now support non\-blocking I/O\. As a result, the request and response POJOs no longer take `InputStream` as a parameter\. Instead, the request object accepts `RequestBody`, which is a stream of bytes\. The asynchronous client accepts `AsyncRequestBody`\.

**Example of Amazon S3`putObject` operation in 1\.x**  

```
s3client.putObject(BUCKET, KEY, new File(file_path));
```

**Example of Amazon S3`putObject` operation in 2\.x**  

```
s3client.putObject(PutObjectRequest.builder()
                                 .bucket(BUCKET)
                                 .key(KEY)
                                 .build(),
                 RequestBody.of(Paths.get("myfile.in")));
```

In parallel, the response object accepts `ResponseTransformer` for synchronous clients and `AsyncResponseTransformer` for asynchronous clients\.

**Example of Amazon S3`getObject` operation in 1\.x**  

```
S3Object o = s3.getObject(bucket, key);
S3ObjectInputStream s3is = o.getObjectContent();
FileOutputStream fos = new FileOutputStream(new File(key));
```

**Example of Amazon S3`getObject` operation in 2\.x**  

```
s3client.getObject(GetObjectRequest.builder().bucket(bucket).key(key).build(),
		ResponseTransformer.toFile(Paths.get("key")));
```

## Exception changes<a name="exceptions-changes"></a>

Exception class names, their structures, and their relationships have also changed\. `software.amazon.awssdk.core.exception.SdkException` is the new base `Exception` class that all the other exceptions extend\.

For a full list of the 2\.x exception class names mapped to the 1\.x exceptions, see [Exception class name changes](migration-exception-changes.md)\.

## Service\-specific changes<a name="service-changes"></a>

### Amazon S3 operation name changes<a name="s3-operations-name"></a>

Many of the operation names for the Amazon S3 client have changed in the AWS SDK for Java 2\.x\. In version 1\.x, the Amazon S3 client is not generated directly from the service API\. This results in inconsistency between the SDK operations and the service API\. In version 2\.x, we now generate the Amazon S3 client to be more consistent with the service API\.

**Example of Amazon S3 client operation in 1\.x**  

```
changeObjectStorageClass
```

**Example of Amazon S3 client operation in 2\.x**  

```
copyObject
```

**Example of Amazon S3 client operation in the Amazon S3 service API**  

```
CopyObject
```

For a full list of the operation name mappings, see the AWS SDK for Java 2\.x [changelog](https://github.com/aws/aws-sdk-java-v2/blob/master/docs/LaunchChangelog.md#4-service-changes)\.

### Cross\-Region access<a name="cross-region-access"></a>

For security best practices, cross\-Region access is no longer supported for single clients\.

In version 1\.x, services such as Amazon S3, Amazon SNS, and Amazon SQS are allowed to access resources across Region boundaries\. This is no longer allowed in version 2\.x using the same client\. If you need to access a resource in a different Region, you must create a client in that Region and retrieve the resource using the appropriate client\.

**Topics**