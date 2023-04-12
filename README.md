# bifrost

_boigs' API gateway_

![](https://norse-mythology.org/wp-content/uploads/2012/11/Bifrost.jpg)


## Description

This repository contains all the configuration for the API gateway. Continue reading
for more details.


## Design

The bifrost is made up of a couple components:
1. An nginx barebones entrypoint, running on Docker, which redirects all traffic to K8S.
1. K8S ingress, which then routes the traffic to the appropriate service.

![](media/diagram.png)

Implememtation details:
- Ingress controller is [`nginx`](https://kubernetes.github.io/ingress-nginx/deploy/#minikube).
- TLS certificates are managed with [`cert-manager`](https://github.com/cert-manager/cert-manager).


## Setup

Initialized with minikube ingress addon:

```
minikube addons enable ingress
```

Start the nginx entrypoint and the ingress:

```
docker compose up -d
kubectl apply -f ingress.yml
```
