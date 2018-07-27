# Kubernetes NginxIngress Exporter for Prometheus
## Installation

First you need enable metrics:
In nginx-ingress daemonset need added annotation:

```bash
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
and restart nginx-ingress-lb on each node.

```bash
helm install --upgrade exporter-nginx-ingress . --tiller-namespace kube-system
```
[More information](https://github.com/coreos/prometheus-operator/blob/master/Documentation/user-guides/running-exporters.md)
