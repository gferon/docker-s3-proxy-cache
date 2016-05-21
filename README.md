# docker-s3-proxy-cache

This is a proof of concept HTTP caching proxy for interactions between Amazon SDKs and Amazon S3. It uses Nginx as the HTTP caching proxy and a slight fork of [ngx_aws_auth](https://github.com/anomalizer/ngx_aws_auth/compare/AuthV2...hectcastro:hmc/remove-date-string-to-sign) for Amazon S3 authenticated requests.

## Usage

First, populate the Nginx `server` configuration with the Amazon S3 bucket you're targeting:

```nginx
chop_prefix /<BUCKET>;
```

As well as your Amazon Web Services (AWS) credentials:

```nginx
aws_access_key <ACCESS_KEY>;
aws_secret_key <SECRET_KEY>;
s3_bucket <BUCKET>;
```

Lastly, use the supplied [Docker Compose](https://docs.docker.com/compose/) configuration to launch an instance of the caching proxy on port `8000`:

```bash
$ docker-compose up
```

## Test

A quick way to test the setup above is with the AWS CLI. Below is an example of overriding the endpoint URL to make use of the caching proxy:

```bash
$ aws --endpoint-url http://localhost:8000 s3 cp s3://<BUCKET>/<OBJECT> .
```
