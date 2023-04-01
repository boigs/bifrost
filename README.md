# bifrost

_boigs' API gateway_

## Description

nginx that acts as the API gateway for boigs' services ecosystem.


## Walkthrough

### Entrypoint

Using nginx's default configuration, which includes all the `*.conf` files found the `conf.d`
folder, the entrypoint of this service is `app.conf`, which itself includes
other files to maintain a hierarchical and pseudo-modularized structure.

### API Gateway

As seen in [this tutorial](https://galvarado.com.mx/post/desplegar-un-api-gateway-con-nginx/),
which is enough for the current needs of the system.