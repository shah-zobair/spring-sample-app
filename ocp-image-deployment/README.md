<h3>Deploying a simple spring app from Image</h3>

**Build the image locally:**
```
docker build -t spring-web .
```

**Expose OpenShift image-registry:**
```
oc patch configs.imageregistry.operator.openshift.io/cluster --patch '{"spec":{"defaultRoute":true}}' --type=merge
```

**Login to OpenShift Image Registry and push the image:**
```
docker whoami -t
docker login default-route-openshift-image-registry.apps.example.com -u quicklab -p sha256~nxcPZVIPZJfEeN0KCLj2Mu1-ISUxa6jiTrXEBELXVIQ

docker tag spring-web default-route-openshift-image-registry.apps.example.com/openshift/spring-web
docker push default-route-openshift-image-registry.apps.example.com/openshift/spring-web
```

**Deploy the application with Service and Route:**
```
oc new-project spring-test-1

oc apply -f deployment-spring-web.yaml
oc apply -f service-spring-web.yaml
oc apply -f route-spring-web.yaml
```

Now, access the application using the dynamically created URL from route object:  https://<URL>/greeting?name=Shah
