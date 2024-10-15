# Provide temporary credentials to the SDK<a name="credentials"></a>

* == temporary credentials / 
  * issued by AWS
  * uses
    * by SDK, to sign cryptographically 
* it happens | before making a request to Amazon Web Services
* how does it work?
  * SDK retrieves configuration values -- by checking -- several locations
    * == several ways to enable it

**Topics**
+ [Use temporary credentials](credentials-temporary.md)
+ [Default credentials provider chain](credentials-chain.md)
+ [Use a specific credentials provider or provider chain](credentials-specify.md)
+ [Use profiles](credentials-profiles.md)
+ [Load temporary credentials from an external process](credentials-process.md)
+ [Supply temporary credentials in code](credentials-explicit.md)
+ [Configuring IAM roles for Amazon EC2](ec2-iam-roles.md)