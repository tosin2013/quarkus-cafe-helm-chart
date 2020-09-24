# Deploy Quarkus Cafe Using a helm chart
![Lint and Test Charts](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-helm/workflows/Lint%20and%20Test%20Charts/badge.svg?branch=gh-pages)
![Release Charts](https://github.com/quarkuscoffeeshop/quarkuscoffeeshop-helm/workflows/Release%20Charts/badge.svg?branch=gh-pages)


## Administrator Tasks 
    
**Install Helm**

```shell script
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```

### OpenShiftt 4.x  DeploymenInstructions 

**Login to OpenShift and create project**
```
oc new-project <<NAMESPACE>
```

**Run ansible playbook to install Red Hat AMQ and mongodb on target cluster**
* [admin-tasks](admin-tasks/README.md)


**Add the Quarkus Coffee Shop  Helm Chart repository**
```
helm repo add quarkuscoffeeshop https://quarkuscoffeeshop.github.io/quarkuscoffeeshop-helm/
```

**Check helm repo**
```
$ helm repo list
NAME             	URL
quarkuscoffeeshop	https://quarkuscoffeeshop.github.io/quarkuscoffeeshop-helm/
```

**Configure values.yml**
```
cat >values.yml<<EOF
# Quarkus Cafe Application Variables
projectnamespace: <<NAMESPACE>>
domain: <<DOMAIN>>
kafka_cluster_name: cafe-cluster
version:
  barista: 3.0.0
  core: 3.0.0
  customermocker: 3.0.1
  kitchen: 3.0.0
  web: 3.0.0

# Helm chart variables
Release:
  Name: quarkuscoffeeshop-deployment
  release-namespace: <<NAMESPACE>>
EOF

**Deploy the Cluster Operator using the Helm command line tool**
```
helm install strimzi/strimzi-kafka-operator
```



### Kubernertes Cluster Instructions - WIP
**Add the Strimzi Helm Chart repository**
```
helm repo add strimzi https://strimzi.io/charts/
```

**Deploy the Cluster Operator using the Helm command line tool**
```
helm install strimzi/strimzi-kafka-operator
```

**Deploy monogodb using helm chart**  
work in progress

## Developer Tasks 
**cd into support/helm-deployment**

**Update the default values found in quarkuscoffeeshop-charts/values.yaml** 

**Test Install quarkus application**
```
helm install quarkus-cafe-deployment quarkuscoffeeshop-charts/ --values quarkuscoffeeshop-charts/values.yaml --dry-run
```

**Install quarkus application**
```
helm install quarkus-cafe-deployment quarkuscoffeeshop-charts/ --values quarkuscoffeeshop-charts/values.yaml 
```
**Uninstall quarkus application**
```
helm uninstall quarkus-cafe-deployment
```

**Expose routes for Web and Customermocker**

The Helm chart does not currently expose the routes for our Web UI or the Customermocker service.  You will need to log in and expose them using oc:

```shell script
oc expose svc/quarkuscoffeeshop-web
oc expose svc/quarkuscoffeeshop-customermocker
```