### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](https://github.com/K-PaaS/cp-guide-eng/blob/master/install-guide/Readme.md) > K-PaaS Container Platform Edge Sample Guide

<br>

## Table of Contents

1. [Document Overview](#1)<br>
  1.1. [Purpose](#1.1)<br>
  1.2. [Scope](#1.2)<br>
  1.3. [System Configuration Diagram](#1.3)<br>
  1.4. [References](#1.4)

2. [K-PaaS Container Platform Edge Sample Deployment](#2)<br>
  2.1. [Web-based Counter Sample](#2.1)<br>
  2.2. [Edge-based Temperature Collection Sample](#2.2)

<br>

## <div id='1'> 1. Document Overview

### <div id='1.1'> 1.1. Purpose
This document (K-PaaS Container Platform Edge Sample Guide) describes the method for deploying and verifying a sample using a Raspberry Pi and a temperature and humidity sensor (DHT11) to configure an actual Edge environment and device.

<br>

### <div id='1.2'> 1.2. Scope
The installation scope is based on the K-PaaS Container Platform Edge sample application to configure and validate the K-PaaS Container Platform Edge node environment.

<br>

### <div id='1.3'> 1.3. System Configuration Diagram
The system architecture consists of a Kubernetes `single cluster` (Control Plane, Worker), a Raspberry Pi (Edge), and a DHT11 sensor (Device) environment.

For additional instance environments required for K-PaaS Container Platform Edge sample deployment, refer to the configuration below.

|Instance Type|Number of Instances|Remarks|
|---|---|---|
|Edge|1|Raspberry Pi<br>ARM64 Architecture|

<br>

![image 001]

<br>

### <div id='1.4'> 1.4. References
> https://kubeedge.io/en/docs/developer/device_crd/
> https://github.com/kubeedge/examples
> https://github.com/kubeedge/examples/blob/master/kubeedge-counter-demo/README.md
> https://github.com/kubeedge/examples/blob/master/temperature-demo/README.md

<br>

## <div id='2'> 2. K-PaaS Container Platform Edge Sample Deployment
This document (K-PaaS Container Platform Edge Sample Guide) verifies data communication between the Cloud and Edge environments using two sample applications. <br>
A K-PaaS Container Platform cluster is set up in the Cloud environment, and a Raspberry Pi is used to configure the Edge node. <br>
A temperature and humidity sensor (DHT11) is connected and configured to the Raspberry Pi.

To deploy K-PaaS Container Platform Edge sample applications, you need to install Podman separately on the Edge node.

Ubuntu 20.04 arm64 Podman Installation
```
$ echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_22.04/ /" | sudo tee etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list

$ curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_20.04/Release.key | sudo apt-key add -

$ sudo apt-get update 
$ sudo apt-get -y install podman
```

<br>

Ubuntu 22.04 arm64 Podman Installation
```
$ sudo apt-get update
$ sudo apt-get -y install podman
```

<br>

### <div id='2.1'> 2.1. Web-based Counter Sample
After deploying the web application on the `Control Plane node`, the counter application is deployed on the `Edge node`.

Download the necessary files for deploying the sample applications on both the `Control Plane node` and the `Edge node`.
```
$ wget --content-disposition https://nextcloud.k-paas.org/index.php/s/fkiZoZkB9oCAcnB/download

$ tar zxvf kubeedge-sample-v1.5.tar.gz
```

<br>

Load the container image file on the `Control Plane node`.
```
$ cd ~/kubeedge-sample/kubeedge-counter/amd64

$ sudo podman load -i kubeedge-counter-app.tar
```

<br>

Load the container image file on the `Edge node`.
```
$ cd ~/kubeedge-sample/kubeedge-counter/arm64

$ sudo podman load -i kubeedge-pi-counter.tar
```

<br>

Deploy DeviceModel and DeviceInstance on the `Control Plane node`.
```
$ cd ~/kubeedge-sample/kubeedge-counter/

## Deploy DeviceModel
$ kubectl apply -f kubeedge-counter-model.yaml

## Change the hostname in DeviceInstance ()
$ sed -i "s/{EDGE_NODE_NAME}/{{Actual Edge Node Hostname}}/g" kubeedge-counter-instance.yaml

ex) sed -i "s/{EDGE_NODE_NAME}/paasta-cp-edge-1/g" kubeedge-counter-instance.yaml

## Deploy DeviceInstance
$ kubectl apply -f kubeedge-counter-instance.yaml
```

<br>

Deploy the web application and the counter application on the `Control Plane node`.
```
$ kubectl apply -f kubeedge-web-controller-app.yaml

$ kubectl apply -f kubeedge-pi-counter-app.yaml
```

<br>

Access the deployed web application through the browser and control the counter function. You can control the counter application deployed in the Edge environment through the web deployed in the Cloud environment, and observe the increasing count value.

![image 002]

<br>

Check the device information on the `Control Plane node` to verify the collected counter information. The update of the value in the status field can be confirmed.
```
$ kubectl get device counter -oyaml -w
```
```
...
status:
  twins:
  - desired:
      metadata:
        timestamp: "1640"
        type: string
      value: "ON"
    propertyName: status
    reported:
      metadata:
        timestamp: "1640253571427"
        type: string
      value: "99"
```

<br>

On the `Edge node`, subscribe to the topic '$hw/events/device/counter/twin/update' to check the transmitted data.
```
## To use the mosquitto_sub command, install the following package
$ sudo apt install mosquitto-clients

$ mosquitto_sub -h 127.0.0.1 -t '$hw/events/device/counter/twin/update' -p 1883
```
```
{"event_id":"","timestamp":0,"twin":{"status":{"actual":{"value":"1643"},"metadata":{"type":"Updated"}}}}
{"event_id":"","timestamp":0,"twin":{"status":{"actual":{"value":"1644"},"metadata":{"type":"Updated"}}}}
{"event_id":"","timestamp":0,"twin":{"status":{"actual":{"value":"1645"},"metadata":{"type":"Updated"}}}}
{"event_id":"","timestamp":0,"twin":{"status":{"actual":{"value":"1646"},"metadata":{"type":"Updated"}}}}
{"event_id":"","timestamp":0,"twin":{"status":{"actual":{"value":"1647"},"metadata":{"type":"Updated"}}}}
```
<br>

### <div id='2.2'> 2.2. Edge-based Temperature Collection Sample
The DHT11 temperature sensor is configured using GPIO on the `Raspberry Pi (Edge node)`, and a temperature collection application is deployed.

Download the necessary files for deploying the sample applications on both the `Control Plane node` and the `Edge node`.
```
$ wget --content-disposition https://nextcloud.k-paas.org/index.php/s/fkiZoZkB9oCAcnB/download

$ tar zxvf kubeedge-sample-v1.5.tar.gz
```

<br>

Load the container image file on the `Edge node`.
```
$ cd ~/kubeedge-sample/kubeedge-temperature/arm64

$ sudo podman load -i kubeedge-temperature.tar
```

<br>

Deploy DeviceModel and DeviceInstance on the `Control Plane node`.
```
$ cd ~/kubeedge-sample/kubeedge-temperature/

## Deploy DeviceModel
$ kubectl apply -f model.yaml

## Change the hostname in DeviceInstance ()
$ sed -i "s/{EDGE_NODE_NAME}/{{Actual Edge Node Hostname}}/g" instance.yaml

ex) sed -i "s/{EDGE_NODE_NAME}/paasta-cp-edge-1/g" instance.yaml

## Deploy DeviceInstance
$ kubectl apply -f instance.yaml
```

<br>

Deploy the temperature collection application on the `Control Plane node`.
```
## Change nodeSelector in deployment
$ vi deployment.yaml

...
nodeSelector:
  kubernetes.io/hostname: {{EDGE_NODE_NAME}} (Change)
...

$ kubectl apply -f deployment.yaml
```

<br>

Check the device information on the `Control Plane node` to verify the collected temperature information.
```
$ kubectl get device temperature -oyaml -w -n kubeedge
```
```
...
status:
  twins:
  - desired:
      metadata:
        type: string
      value: ""
    propertyName: temperature-status
    reported:
      metadata:
        timestamp: "1640253571427"
        type: string
      value: "24C"
```

<br>

On the `Edge node`, subscribe to the topic '$hw/events/device/temperature/twin/update' to check the transmitted data.
```
## To use the mosquitto_sub command, install the following package
$ sudo apt install mosquitto-clients

$ mosquitto_sub -h 127.0.0.1 -t '$hw/events/device/temperature/twin/update' -p 1883
```
```
{"event_id":"","timestamp":0,"twin":{"temperature-status":{"actual":{"value":"24C"},"metadata":{"type":"Updated"}}}}
{"event_id":"","timestamp":0,"twin":{"temperature-status":{"actual":{"value":"24C"},"metadata":{"type":"Updated"}}}}
{"event_id":"","timestamp":0,"twin":{"temperature-status":{"actual":{"value":"23C"},"metadata":{"type":"Updated"}}}}
{"event_id":"","timestamp":0,"twin":{"temperature-status":{"actual":{"value":"24C"},"metadata":{"type":"Updated"}}}}
{"event_id":"","timestamp":0,"twin":{"temperature-status":{"actual":{"value":"24C"},"metadata":{"type":"Updated"}}}}
```

<br>

[image 001]:images/edge-v1.2.png
[image 002]: images/kubeedge-counter-web.png

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](https://github.com/K-PaaS/cp-guide-eng/blob/master/install-guide/Readme.md) > K-PaaS Container Platform Edge Sample Guide