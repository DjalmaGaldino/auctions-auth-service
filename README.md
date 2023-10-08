
## Bonus: Cross-stack authorization

This is very useful in a microservices setup. For example, you have an Auth Service (this service) which owns anything auth/user-related, and a bunch of other services that require user authorization.
Fear not, it is very easy to make your authorizer work anywhere else in your AWS account.

When defining your Lambdas in other services, simply define the `authorizer` as well and provide the ARN of your `auth` function (can be found in the AWS Console or via `sls info`).

#### Example:

```yaml
functions:
  someFunction:
    handler: src/handlers/someFunction.handler
    events:
      - http:
          method: POST
          path: /something
          authorizer: arn:aws:lambda:#{AWS::Region}:#{AWS::AccountId}:function:sls-auth-service-draft-dev-auth
```

If everything was set up correctly, all incoming requests to your `someFunction` Lambda will first be authorized. You can find the JWT claims at `event.requestContext.authorizer`.

## Info:

I needed to install babel/core and downgrade my NodeJs version to v14.x to work my deployment
