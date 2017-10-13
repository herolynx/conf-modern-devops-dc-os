# conf-modern-devops-dc-os

Materials for presentation about modern DevOps with DC/OS.

Similar presentation for **Kubernetes** can be found [here](https://github.com/herolynx/conf-modern-devops-k8s).

Project structure:

* `src`: code of microservice with docker build file
* `devops`: files related with DC/OS
* `others`: other samples for demo purposes

**Demo agenda:**

The purposes of this demo & presentation is to show how micro-services can be managed using DC/OS, thus following scenarios are covered:

* deployment 
* publishing & LB 
* rollback & rolling update
* config management
* web-socket support (with TCP connections load balancing)
* monitoring & log aggregation

## API

* **GET** `/hello`: printing sample message using config
* **GET** `/secrets`: printing sample message using secrets
* **GET** `/probe/health`: doing health-check
* **POST** `/probe/health`: changing status of health-check
* **GET** `/probe/ready`: doing readiness check
* **POST** `/probe/ready`: changing status of readiness
* `/web-socket`: establish web-socket connection (use `test_web_socket.sh` script, see `web-sockets` section)

## Build

1) Build project

```
mvn clean build
```

or

```
mvn clean istall
```

2) Build and push docker image

```
mvn clean package docker:build -DpushImage -DpushImageTags -DdockerImageTags=<VERSION>
```

## Local development

Pre-requisites:

* [DC/OS at Vagrant](https://github.com/dcos/dcos-vagrant)
* Maven
* Java 8

1) Set-up desired version (optional)

```
export DCOS_GENERATE_CONFIG_PATH=installers/dcos/dcos_generate_config-{version}-dev.sh
export DCOS_CONFIG_PATH=etc/config-{version}.yaml
```

2) Set cluster configuration

```
cp VagrantConfig-1m-1a-1p.yaml VagrantConfig.yaml
```

3) Start the cluster

```
vagrant up
```

4) Access the UI using URL: `http://m1.dcos`


Docs:

* [Getting Started with DC/OS on Vagrant](https://oliverveits.wordpress.com/2017/04/15/getting-started-with-dcos-on-vagrant/)

##### Disable authentication. 

DC/OS cluster uses external OAuth by default but it is more convenient to have it disabled for local testing.

```
echo "oauth_enabled: 'false'" >> etc/config-{version}.yaml
```

## DevOps 

1) Create application

```
dcos marathon app add devops/service.json
```

2) Update application's property

```
dcos marathon app update /dcos-java-sample <property>=<value>
```

or using whole file:

```
dcos marathon app update /dcos-java-sample < devops/service.json
```

##### [Basic operations](https://docs.mesosphere.com/1.10/cli/command-reference/)

1) List deployed apps

```
dcos marathon app list
```

2) Scale application

```
dcos marathon app update /dcos-java-sample instances=3
```

3) Check logs

```
dcos task log --follow dcos-java-sample
```

4) Watch progress of deployment

Check on-going deployments

```
dcos marathon deployment list
```

and then:

```
dcos marathon deployment watch --interval=1 <deployment_id>
```

5) Reach the service (on `Vagrant`) using URL: `http://192.168.65.60:10010/hello`

6) Make deployment & config update

```
dcos marathon app update /dcos-java-sample < devops/service.json
```

7) Make rollback

Check on-going deployments

```
dcos marathon deployment list
```

and then:

```
dcos marathon deployment rollback <deployment_id>
```

8) Update config

```
dcos marathon app update /dcos-java-sample < devops/config.json
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

## [Load Balancing and VIPs](https://dcos.io/docs/1.8/usage/service-discovery/load-balancing-vips/)

0) Check options of marathon load-balancer

```
 dcos package describe --config marathon-lb
```

1) Install LB

```
dcos package install marathon-lb --yes
```

2) Deploy sample `nginx` from `others/nginx-hostname-app.json`

```
dcos marathon app add others/nginx-hostname-app.json
```

and check sample output (for `Vagrant`) with `auto-refresh` option using browser:

```
http://192.168.65.60:10006/
```

or you can check internal address, from within your cluster:

```
curl http://marathon-lb.marathon.mesos:10006
```

Docs:

* [Service discovery and load balancing with DCOS](https://mesosphere.com/blog/dcos-marathon-lb/)

## DC/OS - set-up

Overall installation description can be found [here](https://dcos.io/docs/1.7/administration/installing/).

##### Localhost

Follow instruction for [DC/OS at Vagrant](https://github.com/dcos/dcos-vagrant).

##### AWS

Template for [AWS CloudFromattion](https://downloads.dcos.io/dcos/EarlyAccess/commit/14509fe1e7899f439527fb39867194c7a425c771/aws.html?_ga=2.51727017.1886369744.1507529557-2142486960.1500623023)

## Monitoring

##### DataDog

```
dcos package install datadog --options=monitoring/datadog.json --yes
```

Docs:

* [How to use Datadog with DC/OS](https://github.com/dcos/examples/tree/master/datadog/1.9)

## Other docs

* [DC/OS 101](https://dcos.io/docs/1.8/usage/tutorials/dcos-101/)

* [How to scale-test your DCOS cluster](https://mesosphere.com/blog/scale-test-dcos-cluster/)

* [Autoscaling](https://dcos.io/docs/1.7/usage/tutorials/autoscaling/)

* [DNS](https://dcos.io/docs/1.10/networking/mesos-dns/)