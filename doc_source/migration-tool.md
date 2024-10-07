## Note
* [Website Link](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/migration-tool.html)
* NO existing | open source repo

* Pre-release version
  * == NOT ALL features are migrated
    * -> rest must be done manually
* allows
  * automatizing the migration for AWS SDK Java v1 -> v2
* how does it work?
  * use [OpenRewrite](https://docs.openrewrite.org/)
    * == source code refactoring tool / open source
* how to use it?
  * Maven
    * -- based on -- [OpenRewrite Maven Plugin](https://docs.openrewrite.org/reference/rewrite-maven-plugin)
    * run | root directory of your Maven project
      * `dryRun` mode

        ```
        mvn org.openrewrite.maven:rewrite-maven-plugin:dryRun \
          -Drewrite.recipeArtifactCoordinates=software.amazon.awssdk:v2-migration:<sdkversion>*-PREVIEW \
          -Drewrite.activeRecipes=software.amazon.awssdk.v2migration.AwsSdkJavaV1ToV2
        ```

        * -- generate -- 
          * diff logs | console output
          * "rewrite.patch" | "target/rewrite"
        * 👁️ONLY preview the changes == NO changes made | source code files 👁️
      * `run` mode
    
      ```
      mvn org.openrewrite.maven:rewrite-maven-plugin:run \
        -Drewrite.recipeArtifactCoordinates=software.amazon.awssdk:v2-migration:<sdkversion>*-PREVIEW \
        -Drewrite.activeRecipes=software.amazon.awssdk.v2migration.AwsSdkJavaV1ToV2
      ```  

        * modify the source code
        * afterward, you should run `mvn clean verify`
  * Gradle
    * TODO:
* current limitations
  * NOT ALL features are migrated
    * [S3 Client (AmazonS3Client)](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/migration-clients.html)
    * [S3 Transfer Manager (TransferManager)](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/migration-transfer-manager.html)
    * [S3 URI Parsing (AmazonS3URI)](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/migration-s3-uri-parser.html)
    * [DynamoDB object mapping (DynamoDBMapper)](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/migration-ddb-mapper.html)
    * [EC2 metadata utility (EC2MetadataUtils)](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/migration-imds.html)
    * [Waiters (AmazonDynamoDBWaiters)](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/migration-waiters.html)
    * [IAM Policy Builder (Policy)](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/migration-iam-policy-builder.html)
    * [CloudFront presigning (CloudFrontUrlSigner, CloudFrontCookieSigner)](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/migration-cloudfront-presigning.html)
    * [S3 Event Notifications (S3EventNotification)](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/migration-s3-event-notification.html)
    * SDK metric publishing ([1.x documentation](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/generating-sdk-metrics.html), [2.x documentation](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/metrics.html))