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

2) Update application's property

```
dcos marathon app update /k8s-java-sample <property>=<value>
```

##### [Basic operations](https://docs.mesosphere.com/1.10/cli/command-reference/)

1) List deployed apps

```
dcos marathon app list
```

2) Scale application

```
dcos marathon app update /k8s-java-sample instances=3
```

##### [Other operations](https://docs.mesosphere.com/1.10/cli/command-reference/)

1) Get services

```
dcos service
```

2) Install app form universe

```
dcos package install <package-name>
```

3) Uninstall app from universe

```
dcos package uninstall <package_name> --app-id=/<sub_package_name>
```

## DC/OS - set-up

ToDo: Comming soon

## Monitoring

ToDo: Comming soon