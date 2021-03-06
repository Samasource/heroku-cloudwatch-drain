# heroku-cloudwatch-drain

A Heroku HTTPS log drain that stores logs in CloudWatch Logs.

[![Build Status](https://travis-ci.org/kiskolabs/heroku-cloudwatch-drain.svg?branch=master)](https://travis-ci.org/kiskolabs/heroku-cloudwatch-drain)

## Getting started

### Heroku

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

### Locally

Build and install:

    $ go install

Run:

    $ heroku-cloudwatch-drain

## Configuration

See all available configuration flags:

    $ heroku-cloudwatch-drain -h

The AWS configuration is picked up from the environment. For a full list of
environment variables and other ways to configure the AWS region, credentials,
etc., see the [SDK
Configuration](http://docs.aws.amazon.com/sdk-for-go/v1/developer-guide/configuring-sdk.html)
page.

## Sending logs

Logs should be sent to this application, with the log group name as the URL
path. For example, if the heroku-cloudwatch-drain is available at
`https://drain.example.com/`, and you wish to collect logs under the log group
name `my-app`, the log drain URL should be `https://drain.example.com/my-app`.

HTTP Basic Auth is supported and can be configured via CLI flags.

Both the CloudWatch Logs log group and log streams are created automatically as
requests come in. A new and unique log stream is created for each process.

## AWS IAM permissions

The IAM policy containing the minimum required permissions to run this is:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents",
        "logs:PutRetentionPolicy"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```

## Contributing

The [govendor](https://github.com/kardianos/govendor) tool is used for managing
the `vendor` directory.

Run tests:

    $ govendor test +local
