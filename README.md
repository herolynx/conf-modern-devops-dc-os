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

4) Check current marathon configuration

```
dcos package describe --config marathon
```

5) Checking logs

```
dcos service log --follow <scheduler-service-name>
```

6) Checking deployments

```
dcos marathon deployment list
```

## DC/OS - set-up

Overall installation description can be found [here](https://dcos.io/docs/1.7/administration/installing/).

##### Localhost

Follow instruction for [DC/OS at Vagrant](https://github.com/dcos/dcos-vagrant).

##### AWS

Template for [AWS CloudFromattion](https://downloads.dcos.io/dcos/EarlyAccess/commit/14509fe1e7899f439527fb39867194c7a425c771/aws.html?_ga=2.51727017.1886369744.1507529557-2142486960.1500623023)

## Monitoring

ToDo: Comming soon