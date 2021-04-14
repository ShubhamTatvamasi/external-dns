# external-dns

create new namespace:
```bash
kubectl create ns external-dns
```

delete external-dns:
```bash
kubectl delete -f https://github.com/ShubhamTatvamasi/external-dns/raw/master/externaldns.yaml
```

create a deployment and service:
```bash
kubectl create deployment nginx --image=nginx:alpine
kubectl expose deployment nginx --port=80 --name=nginx
```

create ingress value:
```
kubectl apply -f - << EOF
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx
  annotations:
    external-dns.alpha.kubernetes.io/hostname: nginx.google.shubhamtatvamasi.com
    external-dns.alpha.kubernetes.io/ttl: "600"
    cert-manager.io/cluster-issuer: letsencrypt
spec:
  tls:
  - hosts:
      - nginx.google.shubhamtatvamasi.com
    secretName: nginx-tls
  rules:
  - host: nginx.google.shubhamtatvamasi.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: nginx
            port:
              number: 80
EOF
```

build new image:
```bash
git clone https://github.com/kubernetes-sigs/external-dns.git --depth 1
docker build -t shubhamtatvamasi/external-dns --build-arg ARCH=amd64 .
```
