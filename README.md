# rust-testapi

This is a scratchpad project where I'm testing a bunch of things concurrently.

**Building image and running locally**
```
docker build -t rust-testapi .
docker run --rm -p 8080:8080 rust-testapi
```

Note: To debug failing Docker builds, I had to use `DOCKER_BUILDKIT=0` (see [here](https://stackoverflow.com/a/66770818)).

**Pushing to ECR**
```
aws --profile robwilliams42-root ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 913913497794.dkr.ecr.us-east-1.amazonaws.com
docker tag rust-testapi 913913497794.dkr.ecr.us-east-1.amazonaws.com/rust-testapi
docker push 913913497794.dkr.ecr.us-east-1.amazonaws.com/rust-testapi
```

## AWS Elastic Kubernetes

Currently created manually by following [this guide](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html).

```
aws --profile <my aws profile> ec2 create-key-pair --region  us-east-1 --key-name k8skeypair2
eksctl create cluster -f aws/ClusterConfig.yaml --profile <my aws profile>
```

As a side note, I tried creating things manually by following the AWS console + CLI guide, and it wound up in a state where Fargate never provisioned any nodes and so I couldn't do anything.

I also ran into a bug where `eksctl` ignores basically all of your configuration parameters if you specify an AWS profile using `--profile`, so this is why I used the ClusterConfig.yaml file. I found their [example configurations](https://github.com/weaveworks/eksctl/tree/main/examples) more useful than the written documentation.

## Rust API

Rust-based web service API. This isn't a huge focus, but wanted to use Rust to continue my investment in that language. My first attempt with a Warp hello world ended prematurely when I couldn't get graceful shutdown working (CTRL+C against `docker run`) within 15 minutes. I switched to Actix-web which I am more familiar with, and that worked out of the box once I fixed the common `127.0.0.1` vs. `0.0.0.0` bind mistake.

## Authentications

Figure out how to set up authentication with EKS. It seems to have some OpenID Connect options, but I need to investigate this more.