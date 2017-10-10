# conf-modern-devops-dc-os

Materials for presentation about modern DevOps with DC/OS.

## Local development

Pre-requisites:

    * [DC/OS at Vagrant](https://github.com/dcos/dcos-vagrant)
    * Maven
    * Java 8

## DevOps 

1) Deploy application

```
dcos marathon app add devops/service.json
```

2) Update application

```
dcos marathon app update /k8s-java-sample devops/service.json
```

##### Basic operations

1) Get services

```
dcos service
```

2) List deployed apps

```
dcos marathon app list
```

2) Uninstall app from universe

```
dcos package uninstall <package_name> --app-id=/<sub_package_name>
```

## DC/OS - set-up

ToDo: Comming soon

## Monitoring

ToDo: Comming soon