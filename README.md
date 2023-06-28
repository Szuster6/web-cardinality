Example deployment:
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
helm upgrade web-cardinality ../web-cardinality/ --set ssl.tlscrt="$(cat web-cardinality.com.crt)",ssl.tlskey="$(cat web-cardinality.com.key)",basicAuth.htpasswd="$(cat auth)",service.type="LoadBalancer"
```
5. Deploy helm chart with ingress controller installed
```
helm upgrade web-cardinality ../web-cardinality/ --set ssl.tlscrt="$(cat web-cardinality.com.crt)",ssl.tlskey="$(cat web-cardinality.com.key)",basicAuth.htpasswd="$(cat auth)",service.type="ClusterIP",ingress.enabled="true"
```


