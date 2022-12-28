# Certs Maker

[![CodeQL](https://github.com/linjicong/certs-maker/actions/workflows/codeql.yml/badge.svg)](https://github.com/linjicong/certs-maker/actions/workflows/codeql.yml) [![Docker Image](https://img.shields.io/docker/pulls/linjicong/certs-maker.svg)](https://hub.docker.com/r/linjicong/certs-maker)

<p style="text-align: center;">
  <a href="README.md" target="_blank">ENGLISH</a> | <a href="README_CN.md">中文文档</a>
</p>

<img src="logo.png">

**Tiny self-signed tool, ~ 3MB Size.**

Generate a self-hosted / dev certificate through configuration.

<img src="screenshots/docker.png">

## Quick Start

Generate self-signed certificate supporting `*.lab.com` and `*.data.lab.com`, just "One Click":

```bash
docker run --rm -it -v `pwd`/ssl:/ssl linjicong/certs-maker "--CERT_DNS=lab.com,*.lab.com,*.data.lab.com"
# OR use environment:
# docker run --rm -it -v `pwd`/ssl:/ssl -e "CERT_DNS=lab.com,*.lab.com,*.data.lab.com" linjicong/certs-maker
```

Check in the `ssl` directory of the execution command directory:

```bash
ssl
├── lab.com.conf
├── lab.com.crt
└── lab.com.key
```

If you prefer to use file configuration, you can use `docker-compose.yml` like this:

```yaml
version: '2'
services:

certs-maker:
    image: linjicong/certs-maker
    environment:
      - CERT_DNS=lab.com,*.lab.com,*.data.lab.com
    volumes:
      - ./ssl:/ssl
```

Then execute the following command:

```bash
docker-compose up
# OR
# docker compose up
```

If you want the certificate to be more friendly to K8s, you can add the `FOR_K8S` parameter:

```bash
docker run --rm -it -v `pwd`/ssl:/ssl linjicong/certs-maker "--CERT_DNS=lab.com,*.lab.com,*.data.lab.com --FOR_K8S=ON"
# OR
# docker run --rm -it -v `pwd`/ssl:/ssl -e "CERT_DNS=lab.com,*.lab.com,*.data.lab.com" -e "FOR_K8S=ON" linjicong/certs-maker
```

And K8S friendly compose file:

```yaml
version: '2'
services:

certs-maker:
    image: linjicong/certs-maker
    environment:
      - CERT_DNS=lab.com,*.lab.com,*.data.lab.com
      - FOR_K8S=ON
    volumes:
      - ./ssl:/ssl
```

If you want to further define the information content of the certificate, including the issuing country, province, street, organization name, etc., you can refer to the following document to manually add parameters.

## SSL certificate parameters

You can customize the generated certificate by declaring the environment variables or cli args of docker.

Use in environment variables:

| Parameter | Name | Use in environment variables |
| ------ | ------ | ------ |
| Country Name | CERT_C | `CERT_C=CN` |
| State Or Province Name | CERT_ST | `CERT_ST=BJ` |
| Locality Name | CERT_L | `CERT_L=HD` |
| Organization Name | CERT_O | `CERT_O=Lab` |
| Organizational Unit Name | CERT_OU | `CERT_OU=Dev` |
| Common Name | CERT_CN | `CERT_CN=Hello World` |
| Domains | CERT_DNS | `CERT_DNS=lab.com,*.lab.com,*.data.lab.com` |
| Issue for K8s | FOR_K8S | `FOR_K8S=ON` |
| File Owner User | USER | `USER=ubuntu` |
| File Owner UID | UID | `UID=1234` |
| File Owner GID | GID | `GID=2345` |

Use in Program CLI arguments:

| Parameter | Name | Use in CLI arguments |
| ------ | ------ | ------ |
| Country Name | CERT_C | `--CERT_C=CN` |
| State Or Province Name | CERT_ST | `--CERT_ST=BJ` |
| Locality Name | CERT_L | `--CERT_L=HD` |
| Organization Name | CERT_O | `--CERT_O=Lab` |
| Organizational Unit Name | CERT_OU | `--CERT_OU=Dev` |
| Common Name | CERT_CN | `--CERT_CN=Hello World` |
| Domains | CERT_DNS | `--CERT_DNS=lab.com,*.lab.com,*.data.lab.com` |
| Issue for K8s | FOR_K8S | `--FOR_K8S=ON` |
| File Owner User | USER | `--USER=ubuntu` |
| File Owner UID | UID | `--UID=1234` |
| File Owner GID | GID | `--GID=2345` |

## Docker Image

[linjicong/certs-maker](https://hub.docker.com/r/linjicong/certs-maker)
