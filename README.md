# aws-lambda-test-utils

Testing your AWS Lambda Functions does not need to be difficult or complex.

[![Build Status](https://travis-ci.org/dwyl/aws-lambda-test-utils.svg?branch=master)](https://travis-ci.org/dwyl/aws-lambda-test-utils)
[![codecov.io](https://codecov.io/github/dwyl/aws-lambda-test-utils/coverage.svg?branch=master)](https://codecov.io/github/dwyl/aws-lambda-test-utils?branch=master)
[![Dependency Status](https://david-dm.org/dwyl/aws-lambda-test-utils.svg)](https://david-dm.org/dwyl/aws-lambda-test-utils)
[![devDependency Status](https://david-dm.org/dwyl/aws-lambda-test-utils/dev-status.svg)](https://david-dm.org/dwyl/aws-lambda-test-utils#info=devDependencies)

[![NPM](https://nodei.co/npm-dl/aws-lambda-test-utils.png?months=3)](https://nodei.co/npm/aws-lambda-test-utils/)

If you are *new* to Amazon WebServices Lambda
(*or need a refresher*),
please checkout our our  
***Beginners Guide to AWS Lambda***:
https://github.com/dwyl/learn-aws-lambda

## Why?

Testing your code is *essential* everywhere you need *reliability*.

AWS Lambda has a **Testing _Console_** which is a *web-based*
way of invoking your function(s) with a given input and
monitoring the result. But this quite slow and *cannot be automated* (*yet*).

## What?

The simplest *possilbe* way we could think of for Testing
our AWS Lambda functions.

## *How*? (*Usage*)

### *install* `aws-lambda-test-utils` from NPM

```sh
npm install aws-lambda-test-utils --save-dev
```

### Use in your Tests

### Mock Context

An example of using the `mockContextCreator` in an example test.

```js
'use strict';
var test               = require('tape');
var utils              = require('aws-lambda-test-utils')
var mockContextCreator = utils.mockContextCreator;
var index              = require('./index.js'); // lambda function

var ctxOpts = {
  functionName: 'LambdaTest',
  functionVersion: '1',
  invokedFunctionArn: 'arn:aws:lambda:eu-west-1:655240711487:function:LambdaTest:ci'
};
var testEvent = { key1: 'value1' };

test('LambdaTest', function(t){
  t.test("LambdaTest: returns value when given event with key1 property", function(st) {
    function test(result){
      st.equals(result, "value1")
      st.end();
    };
    var context = mockContextCreator({}, test); // no options and test as the callback
    index.handler(testEvent, context);
  });
  t.test("LambdaTest: returns value when given event with key1 property", function(st) {
    function test(result){
      st.equals(result, "value1")
      st.end();
    };
    var context = mockContextCreator(ctxOpts, test); // context options specified and test as the callback
    index.handler(testEvent, context);
  });
  t.end();
});
```

### Mock Events
This helper can be used to mock event objects created by AWS services like S3, SNS, or DynamoDB.

```js
var utils = require('aws-lambda-test-utils');

// has 3 methods on it: createDynamoDBEvent, createSNSEvent, createS3Event
var mockEventCreator = utils.mockEventCreator;
```



#### `createDynamoDBEvent(options)`

Creates a mock DynamoDB event.
```js
var utils = require('aws-lambda-test-utils');
var dynamoEvent = utils.mockEventCreator.createDynamoDBEvent();

// Default options (which can be overridden):
{
  awsRegion: "eu-west-1",
  eventSourceARN: "arn:aws:dynamodb:us-west-2:account-id:table/ExampleTableWithStream/stream/2015-06-27T00:48:05.899",
  events: [{type: "INSERT", number: 1}]
};

```

#### `createSNSEvent(options)`

```js
var utils = require('aws-lambda-test-utils');
var dynamoEvent = utils.mockEventCreator.createSNSEvent();

// Default options (which can be overridden):
{
  message: "default test message"
};

```

#### `createS3Event()`

```js
var utils = require('aws-lambda-test-utils');
var dynamoEvent = utils.mockEventCreator.createS3Event();

```
#### `createAPIGatewayEvent(options)`

```js
var utils = require('aws-lambda-test-utils');
var APIGatewayEvent = utils.mockEventCreator.createAPIGatewayEvent();

// Default options (which can be overridden):
{
  path: "default/path",
  method: "GET",
  headers: {
    "default-header": 'default'
  },
  queryStringParameters: {
    query: "default"
  },
  pathParameters: {
    uuid: '1234'
  },
  stageVariables: {
    ENV: "test"
  },
  body: "default body"
}

```


## Documentation

### Lambda Function `event` & `context`

Every AWS Lambda function take two parameters `event` & `context`

### `context`

The 'context' object has the following form:

```js
{
  //methods
  success,
  done,
  fail,
  getRemainingTimeInMillis,

  //properties
  functionName,
  functionVersion,
  invokedFunctionArn,
  memoryLimitInMB,
  awsRequestId,
  logGroupName,
  logStreamName,
  identity: {
    cognito_identity_id,
    cognito_identity_pool_id
  },
  clientContext: {
    client: {
      installation_id,
      app_title,
      app_version_name,
      app_version_code,
      app_package_name,
      Custom,
    },
    env: {
      platform_version
      platform,
      make,
      model,
      locale,
    }
  }
}
```

The properties of the context object can be specified in an options parameter to the `mockContextCreator` function.  

## Background Reading

+

## TODO

* Add stubs for AWS SDK methods
