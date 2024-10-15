# Basic setup to work with AWS services<a name="setup-basics"></a>

## Overview<a name="setup-overview"></a>

* 👁️conditions required to successfully develop applications / access AWS services -- via -- AWS SDK for Java 👁️
  * [sign in to the AWS access portal](#setup-awsaccount) / available | AWS IAM Identity Center (==successor to AWS Single Sign\-On\)
  * [permissions of the IAM role](https://docs.aws.amazon.com/singlesignon/latest/userguide/permissionsetsconcept.html) / configured for the SDK -> MUST allow access to the AWS services / required by your application
    * permissions / associated with the **PowerUserAccess** AWS managed policy -> are sufficient for MOST development needs
  * development environment with
    * set >=1 [shared configuration files](https://docs.aws.amazon.com/sdkref/latest/guide/file-format.html)
      * `config` file contains [IAM Identity Center single sign\-on settings](#setup-credentials)
        * -> SDK -- can get -- AWS credentials
      * `credentials` file contains 👁️ temporary credentials 👁️
    * [installation of Java v8+](#setup-envtools)
    * [build automation tool ](#setup-envtools)
      * _Example:_ [Maven](https://maven.apache.org/download.cgi) or [Gradle](https://gradle.org/install/)
    * text editor
      * recommended an IDE 
        * _Example:_ [IntelliJ IDEA](https://www.jetbrains.com/idea/download/#section=windows), [Eclipse](https://www.eclipse.org/ide/), or [NetBeans](https://netbeans.org/downloads/)
        * Reason: 🧠 normally integrate AWS Toolkits 🧠
          * == more easily work with AWS services
          * _Example:_ [AWS Toolkit for IntelliJ](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html) and [AWS Toolkit for Eclipse](https://docs.aws.amazon.com/toolkit-for-eclipse/v1/user-guide/welcome.html)
  * active AWS access portal session | ready to run your application
    * -- via -- AWS CLI, you [initiate the sign\-in process](#setup-login-sso)

* TODO:
**Important**  
The instructions in this setup section assume that you or organization uses IAM Identity Center\. If your organization uses an external identity provider that works independently of IAM Identity Center, find out how you can get temporary credentials for the SDK for Java to use\. Follow [these instructions](credentials-temporary.md#credentials-temporary-from-portal) to add temporary credentials to the `~/.aws/credentials` file\.  
If your identity provider adds temporary credentials automatically to the `~/.aws/credentials` file, make sure that the profile name is `[default]` so that you do not need to provide a profile name to the SDK or AWS CLI\.

## Sign\-in ability to the AWS access portal<a name="setup-awsaccount"></a>

* AWS access portal
  * == web location / you manually sign in -- to the -- IAM Identity Center
    * format of the URL
      * `d-xxxxxxxxxx.awsapps.com/start` or
      * `your_subdomain.awsapps.com/start`
  * [IAM Identity Center authentication](https://docs.aws.amazon.com/sdkref/latest/guide/access-sso.html)

## Set up single sign\-on access for the SDK<a name="setup-credentials"></a>

* once you complete [Step 2](https://docs.aws.amazon.com/sdkref/latest/guide/access-sso.html#idcGettingStarted) -> your system should contain
  * AWS CLI
    * uses
      * start an [AWS access portal session](#setup-login-sso) | before you run your application
  * `~/.aws/config` / contains a [default profile](https://docs.aws.amazon.com/sdkref/latest/guide/file-format.html#file-format-profile)
    * SDK for Java -- via the profile's SSO token provider configuration, acquire -- credentials | before sending requests to AWS
      * `sso_role_name` value / is | IAM role -- connected to an -- IAM Identity Center permission set -> should allow access to the AWS services / used | your application
      * check [SSO token provider configuration](https://docs.aws.amazon.com/sdkref/latest/guide/feature-sso-credentials.html#sso-token-config)
    * _Example:_ sample `config` file shows a default profile + SSO token provider configuration 

  ```
  [default]
  sso_session = my-sso      # refers to the next section
  sso_account_id = 111122223333
  sso_role_name = SampleRole
  region = us-east-1
  output = json

  # section -- to initiate an -- AWS access portal session
  [sso-session my-sso]
  sso_region = us-east-1
  sso_start_url = https://provided-domain.awsapps.com/start
  sso_registration_scopes = sso:account:access
  ```

## Sign in using the AWS CLI<a name="setup-login-sso"></a>

* goal
  * active AWS access portal session
    * uses
      * 👁️SDK can use IAM Identity Center authentication -- to resolve -- credentials 👁️ 
* activate AWS access portal session
    ```
    aws sso login
    
    # if you need to use certain AWS profile
    aws sso login --profile profileName
    ```
  * if you ALREADY have an active AWS access portal session -> 
    * NOT required to provide credentials
    * displayed a dialog / -- requests permission for -- `botocore`
      * `botocore` == foundation for the AWS CLI
      * select **Allow**
* test if you ALREADY have an active session

    ```
    aws sts get-caller-identity
    # returns IAM Identity Center account & permission set / configured | "config" 
    ```

## Install Java and a build tool<a name="setup-envtools"></a>

Your development environment needs the following:
+ Java 8 or later\. The AWS SDK for Java works with the [Oracle Java SE Development Kit](https://www.oracle.com/java/technologies/javase-downloads.html) and with distributions of Open Java Development Kit \(OpenJDK\) such as [Amazon Corretto](http://aws.amazon.com/corretto/), [Red Hat OpenJDK](https://developers.redhat.com/products/openjdk), and [AdoptOpenJDK](https://adoptopenjdk.net/)\.
+ A build tool or IDE that supports Maven Central such as Apache Maven, Gradle, or IntelliJ\.
  + For information about how to install and use Maven, see [http://maven\.apache\.org/](http://maven.apache.org/)\.
  + For information about how to install and use Gradle, see [https://gradle\.org/](https://gradle.org/)\.
  + For information about how to install and use IntelliJ IDEA, see [https://www\.jetbrains\.com/idea/](https://www.jetbrains.com/idea/)\.

## Additional authentication options<a name="setup-additional"></a>

For more options on authentication for the SDK, such as the use of profiles and environment variables, see the [configuration](https://docs.aws.amazon.com/sdkref/latest/guide/creds-config-files.html) chapter in the AWS SDKs and Tools Reference Guide\.