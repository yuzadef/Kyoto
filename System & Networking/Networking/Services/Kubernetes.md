# Kubernetes

## Summary
- [Theory](#theory)
- [Basic usages](#basic-usages)
  - [Listing the pods](#listing-the-pods)
  - [Check privilege of current service account](#check-privilege-of-current-service-account)
  - [Listing resources](#listing-resources)
  - [Read resources](#read-resources)


## Theory
Kubernetes automates operational tasks of container management and includes built-in commands for deploying applications, rolling out changes to your applications, scaling your applications up and down to fit changing needs, monitoring your applications and making it easier to manage applications.

## Basic usages
### Listing the pods
```
kubectl --token=`cat token` --insecure-skip-tls-verify --namespace=default --server=https://10.0.0.1:8443/ get pods
```

### Check privilege of current service account
```
kubectl --token=`cat token` --insecure-skip-tls-verify --namespace=default --server=https://10.0.0.1:8443/ auth can-i --list
```

### Listing resources
```
kubectl --token=`cat token` --insecure-skip-tls-verify --namespace=default --server=https://10.0.0.1:8443/ get secrets
```

### Read resources
```
kubectl --token=`cat token` --insecure-skip-tls-verify --namespace=default --server=https://10.0.0.1:8443/ get [resource] [name] -o yaml
```
