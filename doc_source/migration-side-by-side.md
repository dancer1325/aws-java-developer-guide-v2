# Use the SDK for Java 1\.x and 2\.x side\-by\-side<a name="migration-side-by-side"></a>

* BOTH versions of the AWS SDK for Java | your project, can be used
* _Example:_ `pom.xml` for a project / uses Amazon S3 from version 1\.x & DynamoDB from version 2\.16\.1
  

```
<dependencyManagement>
    <dependencies>
        // v1 for aws-java-sdk-bom
        <dependency>
            <groupId>com.amazonaws</groupId>
            <artifactId>aws-java-sdk-bom</artifactId>
            <version>1.12.1</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        // v2 for bom
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
    // v1 for S3
    <dependency>
      <groupId>com.amazonaws</groupId>
      <artifactId>aws-java-sdk-s3</artifactId>
    </dependency>
    // v1 for DynamoDB
    <dependency>
      <groupId>software.amazon.awssdk</groupId>
      <artifactId>dynamodb</artifactId>
    </dependency>
</dependencies>
```