Add repo:
```
helm repo add web-cardinality https://szuster6.github.io/web-cardinality/
```

Install helm chart:
```
helm install my-web-cardinality web-cardinality/web-cardinality
```

Example deployment with ssl and basic auth enabled:
1. use "createPV: true" and place your static content on node host path /data/web-cardinality or "createPV: false" and use your own PV.
```
mkdir -p /data/web-cardinality
echo "Hello from web-cardinality" > /data/web-cardinality/index.html
```

2. Generate self signed certificate
```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout web-cardinality.com.key -out web-cardinality.com.crt
commonName=web-cardinality.com
```

3. Generate auth file for basic auth
```
htpasswd -c auth webmaster
```

4. Deploy helm chart without ingress controller installed
```
helm install web-cardinality web-cardinality/web-cardinality --set ssl.tlscrt="$(cat ../certs/web-cardinality.com.crt)",ssl.tlskey="$(cat ../certs/web-cardinality.com.key)",basicAuth.htpasswd="$(cat ../certs/auth)",service.type="LoadBalancer",ssl.enabled="true",basicAuth.enabled="true"
```

5. Deploy helm chart with ingress controller installed
```
helm install web-cardinality web-cardinality/web-cardinality --set ssl.tlscrt="$(cat ../certs/web-cardinality.com.crt)",ssl.tlskey="$(cat ../certs/web-cardinality.com.key)",basicAuth.htpasswd="$(cat ../certs/auth)",service.type="ClusterIP",ingress.enabled="true"
```


