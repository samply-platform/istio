# Istio

## Installation

```bash
helm repo add istio.io https://storage.googleapis.com/istio-release/releases/1.3.0/charts/

helm install --name istio-init --namespace istio-system istio.io/istio-init --wait

helm install --name istio --namespace istio-system \
    --set grafana.enabled=true \
    --set kiali.enabled=true \
    --set tracing.enabled=true \
    --set kiali.createDemoSecret=true \
    --set "kiali.dashboard.jaegerURL=http://jaeger-query:16686" \
    --set "kiali.dashboard.grafanaURL=http://grafana:3000" \
    --set global.proxy.accessLogFile="/dev/stdout" \
    --set global.disablePolicyChecks=false \
    istio.io/istio --wait

kubectl label namespace default istio-injection=enabled

kubectl apply -f k8s/
```

## Auth

1. Create load balancer for service that can be used as host for gateway and virtual service
1. Enable global policies (https://istio.io/docs/tasks/policy-enforcement/enabling-policy/)
2. Apply gateway and virtual service [yaml](./k8s/catalog-api-gateway.yaml)
3. Apply auth policy [yaml](./k8s/catalog-api-enduser-auth.yaml)

## Links

* https://github.com/istio/istio/wiki/Route-directive-adapter-development-guide
* https://istio.io/docs/tasks/policy-enforcement/control-headers/


## DNS

Insert istioingress ip after using `minikube tunnel` into

`/etc/NetworkManager/dnsmasq.d/kube.conf`

```
address=/.devlocal/10.100.87.68
```