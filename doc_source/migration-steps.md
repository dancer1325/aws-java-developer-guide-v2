## Note
* [Website Link](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/migration-steps.html)
* NO existing | open source repo

* goal
  * manually migrate application / use AWS SDK for Java v1 -> v2
* steps
  * add | "pom.xml"

    ```
    <dependencyManagement>
      <dependencies>
        <dependency>
          <groupId>software.amazon.awssdk</groupId>
          <artifactId>bom</artifactId>
          <version>2.X.Y</version>
          <type>pom</type>
          <scope>import</scope>
        </dependency>
      </dependencies>
    </dependencyManagement>
    ```
    * Reason: 🧠 ALL v2 dependency needed -> come from the SAME version & v1 & v2 can be used | SAME time 🧠
  * search files for `SERVICE_IDs` / used | v1 
    * find `import com.amazonaws.services`
    * _Example:_ `cognitoidentity` is the `SERVICE_ID` for Amazon Cognito Identity
  * determine v2 Maven dependency / those `SERVICE_IDs`
  * add v2 Maven dependencies | "pom.xml"
  * remove v1 dependencies | "pom.xml" & v1 imports | source code
  * refactor the code / use v2 API enhancements
* _Example:_ TODO: