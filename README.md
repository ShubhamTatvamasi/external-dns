# external-dns


```bash
kubectl apply -f - << EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: external-dns
spec:
  strategy:
    type: Recreate
  selector:
    matchLabels:
      app: external-dns
  template:
    metadata:
      labels:
        app: external-dns
    spec:
      containers:
      - name: external-dns
        image: k8s.gcr.io/external-dns/external-dns:v0.7.7
        args:
        - --source=ingress
        - --provider=godaddy
        - --godaddy-api-key=<Your API Key>
        - --godaddy-api-secret=<Your API secret>
EOF
```

```bash
git clone https://github.com/kubernetes-sigs/external-dns.git --depth 1
docker build -t shubhamtatvamasi/external-dns --build-arg ARCH=amd64 .
```
