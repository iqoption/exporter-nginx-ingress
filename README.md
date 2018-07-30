# Kubernetes NginxIngress Exporter for Prometheus
## Before installation

Version 0.9.0 and above of NGINX ingress have built-in support for exporting Prometheus metrics.

### How enable?
1) In nginx-ingress daemonset need added annotation:

```YAML
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  labels:
    name: nginx-ingress-lb
  name: nginx-ingress-lb
  namespace: kube-system
spec:
  selector:
    matchLabels:
      name: nginx-ingress-lb
  template:
    metadata:
+      annotations:
+        prometheus.io/port: "10254"
+        prometheus.io/scrape: "true"
```
`and need restart nginx-ingress-lb on each node.`

2) In nginx-ingress configmap need added data:
```bash
$ kubectl edit cm/nginx-configuration -n kube-system
```

```YAML
apiVersion: v1
+ data:
+  enable-vts-status: "true"
kind: ConfigMap
metadata:
  labels:
    name: ...
  name: ...
  namespace: ...
```
[More information](https://docs.gitlab.com/ee/user/project/integrations/prometheus_library/nginx_ingress.html#manually-setting-up-nginx-ingress-for-prometheus-monitoring)

## Installation

```bash
$ helm repo add iqoption https://iqoption.github.io/exporter-nginx-ingress
$ helm upgrade --install exportner-nginx-ingress iqoption/exporter-nginx-ingress --tiller-namespace kube-system
```

[More information](https://github.com/coreos/prometheus-operator/blob/master/Documentation/user-guides/running-exporters.md)
