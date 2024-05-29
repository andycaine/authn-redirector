# authy

Lamda@Edge function that redirects clients to a login endpoint if the origin returns a 401 response.

## Description

When building a web app, we normally want a user that fails authentication checks to be redirected to an authentication screen, rather than be served up a HTTP 401 response. `authn-redirector` enables this by redirecting an authentication user to an authentication URL if the underlying origin returns a 401 response.

## Getting started

`authn-redirector` is available via the AWS Serverless Application Repository. Include it in your SAM / CloudFormation template as a serverless application`, or deploy directly from the [AWS Serverless Application Repository](https://eu-west-2.console.aws.amazon.com/lambda/home?region=eu-west-2#/create/app?applicationId=arn:aws:serverlessrepo:eu-west-2:211125310871:applications/authn-redirector).

```yaml
Authy:
  Type: AWS::Serverless::Application
  Properties:
    Location:
      ApplicationId: 'arn:aws:serverlessrepo:eu-west-2:211125310871:applications/authn-redirector'
      SemanticVersion: <CURRENT_VERSION>
    Parameters:
      ...
```

Note that, as this is a Lambda@Edge resource, it must be deployed in the us-east-1 region.

You can then configure CloudFront to use `authn-redirector` to perform redirects on 401 responses:

```yaml
CloudFrontDistro:
  Type: AWS::CloudFront::Distribution
  Properties:
    DistributionConfig:
      DefaultCacheBehavior:
        LambdaFunctionAssociations:
          - EventType: origin-response
            LambdaFunctionARN: !Ref RedirectorFunctionVersion
```
