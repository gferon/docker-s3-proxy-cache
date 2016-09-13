# docker-s3-proxy-cache

[![Docker Repository on Quay.io](https://quay.io/repository/azavea/s3-proxy-cache/status "Docker Repository on Quay.io")](https://quay.io/repository/azavea/s3-proxy-cache)
[![Apache V2 License](http://img.shields.io/badge/license-Apache%20V2-blue.svg)](https://github.com/azavea/docker-s3-proxy-cache/blob/develop/LICENSE)

A proof of concept HTTP caching proxy for requests against Amazon S3 using the [AWS Signature v4](http://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-authenticating-requests.html). The proxy itself is powered by [OpenResty/Nginx](https://openresty.org/) and two Lua modules produced by the [Adobe API Platform](https://github.com/adobe-apiplatform) team for HMAC and generating a valid AWS signature:

- https://github.com/adobe-apiplatform/api-gateway-aws
- https://github.com/adobe-apiplatform/api-gateway-hmac

## Usage

First, build the container:

```bash
$ docker build -t quay.io/azavea/s3-proxy-cache:latest .
```

Then, run the container with an Amazon S3 region reference and credentials:

```bash
$ docker run --rm \
    -p 8000:80 \
    -e AWS_DEFAULT_REGION="us-east-1" \
    -e AWS_ACCESS_KEY_ID="AKI..." \
    -e AWS_SECRET_ACCESS_KEY="w9G..." \
    -v /var/cache/nginx:/var/cache/nginx \
    quay.io/azavea/s3-proxy-cache:latest
```

## Test

A quick way to test the setup above is with the AWS CLI. Below is an example of how to enable AWS Signature v4 for the `default` profile and overriding the AWS CLI endpoint URL to target the caching proxy:

```bash
$ aws configure set default.s3.signature_version s3v4
$ aws --endpoint-url http://localhost:8000 s3 cp s3://<BUCKET>/<OBJECT> .
```
