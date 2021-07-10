# rust-testapi

This is a scratchpad project where I'm testing a bunch of things concurrently.

```
docker build -t rust-testapi .
docker run --rm -p 8080:8080 rust-testapi
```

To debug failed build, I had to use `DOCKER_BUILDKIT=0` (see [here](https://stackoverflow.com/a/66770818)).

## AWS Elastic Kubernetes

Currently created manually in AWS console with basically all default settings.

TODO: do this with Terraform

## Rust API

Rust-based web service API. This isn't a huge focus, but wanted to use Rust to continue my investment in that language.

## Authentications

Figure out how to set up authentication with EKS. It seems to have some OpenID Connect options, but I need to investigate this more.