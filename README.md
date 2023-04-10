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

Enable minikube's ingress addon:

```
minikube addons enable ingress
```

Start everything:

```
docker compose up -d
kubectl apply -f cert-manager.yml
kubectl apply -f ingress.yml
```

Install [sniproxy](https://github.com/dlundquist/sniproxy) and run it with:

```
sudo sniproxy -c sniproxy.conf
```

### Why SNIProxy?

Because we already have tech debt.

Minikube should not be used for what we want to do, but instead we should opt for another
solution like [k3s](https://k3s.io/) or [microk8s](https://microk8s.io). We cannot expose
the ingress we have built without incurring in a lot more problems and security risks.

Having said this, we can't route all the traffic through the nginx entrypoint either due to:
- For HTTPS, nginx needs the certificate files. However, these live inside K8S and are
managed by cert-manager.
- We could use [ngx_stream_core_module](https://nginx.org/en/docs/stream/ngx_stream_core_module.html)
but it's not enabled by default and nginx has to be built with a specific parameter. Also
there are no (up-to-date) Docker images available that have this.

Therefore, the easiest option we have is using SNIProxy, as seen
[here](https://serverfault.com/q/1083973).
