### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > K-PaaS Container Platform Edge Sample Guide


<br>

## Table of Contents

1. [Document Outline](#1)  
  1.1. [Purpose](#1.1)  
  1.2. [Scope](#1.2)  
  1.3. [System Configuration Diagram](#1.3)  
  1.4. [References](#1.4)  

2. [K-PaaS Container Platform Edge Sample Deployment](#2)  
  2.1. [Sample web-based counters](#2.1)  
  2.2. [Edge-based temperature collection sample](#2.2)  

<br>

## <div id='1'> 1. Document Outline

### <div id='1.1'> 1.1. Purpose
This document (Kube Edge Sample Deployment Guide) describes how to deploy and verify samples by configuring the actual Edge environment and device using Raspberry Pi and Thermo-Humidity Sensor (DHT11).

<br>

### <div id='1.2'> 1.2. Scope
The installation range was prepared based on the Kube Edge Sample deployment guide to configure and verify the actual Kube Edge environment.

<br>

### <div id='1.3'> 1.3. System Configuration Diagram
The system configuration consists of a Kubernetes `Single Cluster` (Control Plane, Worker), a Raspberry Pi (Edge), and a DHT11 sensor (Device) environment.

Refer to the configuration below for additional instance environments required for the K-PaaS Container Platform Edge sample deployment.

|Instance Type|Number of Instances|Note|
|---|---|---|
|Edge|1| Raspberry Pi<br>ARM64 Architecture |

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
This document (K-PaaS Container Platform Edge Sample Guide) uses two sample applications to demonstrate data communication between cloud and edge environments.<br>
We configured a K-PaaS container platform cluster in the cloud environment and configured the edge nodes using Raspberry Pi.<br>
We connected and configured a temperature and humidity sensor (DHT11) to the Raspberry Pi.

To deploy the K-PaaS Container Platform Edge sample application, you need to install Podman separately on the Edge node.

Install  Ubuntu 20.04 arm64 Podman
```
$ echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_22.04/ /" | sudo tee etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list

$ curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_20.04/Release.key | sudo apt-key add -

$ sudo apt-get update 
$ sudo apt-get -y install podman
```

<br>

Install Ubuntu 22.04 arm64 Podman
```
$ sudo apt-get update
$ sudo apt-get -y install podman
```

<br>

### <div id='2.1'> 2.1. Sample web-based counters
After deploying the web application on the Control Plane node, proceed to deploy the counter application on the Edge node.

Download the files required to deploy the sample application from the Control Plane node and the Edge node, respectively.
```
$ wget --content-disposition https://nextcloud.k-paas.org/index.php/s/fkiZoZkB9oCAcnB/download

$ tar zxvf kubeedge-sample-v1.5.tar.gz
```

<br>

Load the container image file from the `Control Plane node`.
```
$ cd ~/kubeedge-sample/kubeedge-counter/amd64

$ sudo podman load -i kubeedge-counter-app.tar
```

<br>

Load the container image file from the `Edge node`.
```
$ cd ~/kubeedge-sample/kubeedge-counter/arm64

$ sudo podman load -i kubeedge-pi-counter.tar
```

<br>

Deploy DeviceModel, DeviceInstance from the `Control Plane node`.
```
$ cd ~/kubeedge-sample/kubeedge-counter/

## Deploy DeviceModel
$ kubectl apply -f kubeedge-counter-model.yaml

## Change the hostname within a DeviceInstance ()
$ sed -i "s/{EDGE_NODE_NAME}/{{Physical edge node hostname}}/g" kubeedge-counter-instance.yaml

ex) sed -i "s/{EDGE_NODE_NAME}/paasta-cp-edge-1/g" kubeedge-counter-instance.yaml

## Deploy DeviceInstance
$ kubectl apply -f kubeedge-counter-instance.yaml
```

<br>

`Deploy the web application, counter application in the `Control Plane node`.
```
$ kubectl apply -f kubeedge-web-controller-app.yaml

$ kubectl apply -f kubeedge-pi-counter-app.yaml
```

<br>

Access the deployed web in a browser to control counter functions. Through the web deployed in the cloud environment, you can control the counter application deployed in the edge environment to obtain incremental count values.

![image 002]

<br>

Check the information of the Device in the `Control Plane node` to check the counter information being collected. The value of status is checked for updating.
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

Subscribe to the topic '$hw/events/device/counter/twin/update' on the `Edge node' to see the data being delivered.
```
## To use the mosquitto_sub command, install the following packages
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


### <div id='2.2'> 2.2. Edge-based temperature collection sample
We configured the DHT11 temperature sensor using GPIO on the Raspberry Pi (Edge node) and deployed the temperature collection application.

Download the files required to deploy the sample application from the Control Plane node and the Edge node.
```
$ wget --content-disposition https://nextcloud.k-paas.org/index.php/s/fkiZoZkB9oCAcnB/download

$ tar zxvf kubeedge-sample-v1.5.tar.gz
```

<br>

Load the container image file from the `Edge node`.
```
$ cd ~/kubeedge-sample/kubeedge-temperature/arm64

$ sudo podman load -i kubeedge-temperature.tar
```

<br>

Deploy DeviceModel, DeviceInstance from the `Control Plane node`.
```
$ cd ~/kubeedge-sample/kubeedge-temperature/

## Deploy DeviceModel
$ kubectl apply -f model.yaml

## Changing the hostname in a DeviceInstance ()
$ sed -i "s/{EDGE_NODE_NAME}/{{실제 엣지노드 호스트명}}/g" instance.yaml

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

Check the Device's information in the Control Plane node to see the temperature information being collected.
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

Subscribe to the topic '$hw/events/device/temperature/twin/update' on the `Edge node' to see the data being passed.
```
## To use the mosquitto_sub command, install the following packages
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

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > K-PaaS Container Platform Edge Sample Guide
