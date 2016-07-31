# Simple dockerized smtp relay server based on postfix

Project forked from [xadozuk/smtp-relay](https://github.com/xadozuk/docker-smtp-relay)

You must configure your domain accordingly (reverse DNS, SPF, ...) for mail not to be considered as spam.

## Security Consideration

This image has **not** been extensively tested for security. 

*Only* containers from docker networks and RELAY_NETWORKS can use the smtp relay.

You **shouldn't** expose the port 25 of this container directly on the Internet.

## Usage

1. First start the smtp relay container

  ```
  docker run -d --name smtp-relay -e SMTP_HOSTNAME=smtp.domain.tld pure/smtp-relay
  ```
  
2. Link the smtp relay to another container

  ```
  docker run -d --link smtp-relay:smtp <image>
  ```
  
3. Connect to the smtp relay with `smtp:25` inside your container.
4. Enjoy !

## Relay for networks

Set environment variable RELAY_NETWORKS to add subnets in postfix configuration (as example for two subnets **10.11.12.0/24, 10.22.32.0/24** and exposing port to host):

```
  docker run \
    -d \
    -p 25:25 \
    --name smtp-relay \
    -e SMTP_HOSTNAME=smtp.domain.tld \
    -e RELAY_NETWORKS="10.11.12.0/24 10.22.32.0/24" \
    pure/smtp-relay
```
