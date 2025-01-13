### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](../README.md) > Edge Sample Guide

<br>

## Table of Contents

1. [Document Outline](#1)  
  1.1. [Purpose](#1.1)  
  1.2. [Range](#1.2)  
  1.3. [System Configuration Diagram](#1.3)  
  1.4. [References](#1.4)  

2. [KubeEdge Sample Deployment](#2)  
  2.1. [Web Based KubeEdge Counter Sample](#2.1)  
  2.2. [Edge Based KubeEdge Temperature Collect Sample](#2.2)  

<br>

## <div id='1'> 1. Document Outline

### <div id='1.1'> 1.1. Purpose
This document (Kube Edge Sample Deployment Guide) describes how to deploy and verify samples by configuring the actual Edge environment and device using Raspberry Pi and Thermo-Humidity Sensor (DHT11).

<br>

### <div id='1.2'> 1.2. Range
The installation range was prepared based on the Kube Edge Sample deployment guide to configure and verify the actual Kube Edge environment.

<br>

### <div id='1.3'> 1.3. System Configuration Diagram
The system configuration consists of a Kubernetes Cluster Kubernetes Cluster(Master, Worker)와 Raspberry Pi(Edge), DHT11 Sensor(Device) environment.
Install the Kubernetes Cluster (Master, Worker) through Kubespray and install KubeEdge in the Kubernetes environment.
For the total required VM environment,  **1 or more Master VM, 1 or more Worker VM, 1 or more Raspberry Pi** are required.
This document is about the configuration and verification of the Edge environment for configuring the actual Edge environment.

![image 001]

<br>

### <div id='1.4'> 1.4. References
> https://kubeedge.io/en/docs/developer/device_crd/
> https://github.com/kubeedge/examples
> https://github.com/kubeedge/examples/blob/master/kubeedge-counter-demo/README.md
> https://github.com/kubeedge/examples/blob/master/temperature-demo/README.md

<br>

## <div id='2'> 2. KubeEdge Sample Environment Setting
In this guide, data communication between Cloud and Edge is checked using two samples.
Kubernetes Cluster was configured in the Cloud environment, and Edge Node was added using Raspberry Pi.
A temperature and humidity sensor DHT11 was connected to Raspberry Pi.

Before installing the Container Platform portal, install Podman separately to deploy the KubeEdge Sample. For Podman installation, see **3.1. CRI-O secure-registry Setting** in the portal installation guide.
> [CRI-O secure-registry Setting guide](../container-platform-portal/cp-portal-deployment-standalone-guide-v1.2.md#31-cri-o-insecure-registry-setting)
<br>

### <div id='2.1'> 2.1. Web Based KubeEdge Counter Sample
Deploy the Web Application from **Master Node** and then proceed to Deploy the Counter Application from **Edge Node**.

- Download the files required for sample deployment from **Master Node** and **Edge Node**.
```
$ wget --content-disposition https://nextcloud.paas-ta.org/index.php/s/Bb8diHCZr7wNbcj/download

$ tar zxvf kubeedge-sample.tar.gz
```

<br>

- Load Container Image file from **Master Node**.
```
$ cd ~/kubeedge-sample/kubeedge-counter/amd64

$ sudo podman load -i kubeedge-counter-app.tar
```

<br>

- Load Container Image file from **Edge Node**.
```
$ cd ~/kubeedge-sample/kubeedge-counter/arm64

$ sudo podman load -i kubeedge-pi-counter.tar
```

<br>

- Deploy DeviceModel and DeviceInstance from **Master Node**.
```
$ cd ~/kubeedge-sample/kubeedge-counter/

## DeviceModel Deployment
$ kubectl apply -f kubeedge-counter-model.yaml

## Change hostname in DeviceInstance ()
$ sed -i "s/{EDGE_NODE_NAME}/{{Actual Edge Node Hostname}}/g" kubeedge-counter-instance.yaml

ex) sed -i "s/{EDGE_NODE_NAME}/cp-edge-1/g" kubeedge-counter-instance.yaml

## DeviceInstance Deployment
$ kubectl apply -f kubeedge-counter-instance.yaml
```

<br>

- Deploy  Web Application and Counter Application from **Master Node**.
```
$ kubectl apply -f kubeedge-web-controller-app.yaml

$ kubectl apply -f kubeedge-pi-counter-app.yaml
```

<br>

- Access the deployment web from a browser to control the Counter function. Counter values can be obtained by controlling the counter application deployed in the Edge area through the web deployment in the Cloud area.
![image 002]

<br>

- Check the device information in **Master Node** and check the counter information being collected. The values value update is checked below.
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

- Subscribe the '$hw/events/device/counter/twin/update' topic on **Edge Node** to view the datas being transferred.
```
## Install the package below to use the mosquitto_sub command.
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


### <div id='2.2'> 2.2. Edge Based KubeEdge Temperature Collect Sample
The DHT11 temperature sensor was configured using GPIO at the **Edge Node (Raspberry Pi)**. Temperature Application was deployed.

- Download the file necessary for Sample Deployment in **Master Node** and **Edge Node**.
```
$ wget --content-disposition https://nextcloud.paas-ta.org/index.php/s/Bb8diHCZr7wNbcj/download

$ tar zxvf kubeedge-sample.tar.gz
```

<br>

- Load Container Image file from **Edge Node**.
```
$ cd ~/kubeedge-sample/kubeedge-temperature/arm64

$ sudo podman load -i kubeedge-temperature.tar
```

<br>

- Deploy DeviceModel and DeviceInstance from **Master Node**.
```
$ cd ~/kubeedge-sample/kubeedge-temperature/

## DeviceModel Deployment
$ kubectl apply -f model.yaml

## Change hotname in DeviceInstance ()
$ sed -i "s/{EDGE_NODE_NAME}/{{Actual Edge Node Hostname}}/g" instance.yaml

ex) sed -i "s/{EDGE_NODE_NAME}/cp-edge-1/g" instance.yaml

## DeviceInstance Deployment
$ kubectl apply -f instance.yaml
```

<br>

- Deploy Web Application and Counter Application from **Master Node**.
```
## Change nodeSelector in deployment
$ vi deployment.yaml

...
nodeSelector:
  kubernetes.io/hostname: {{EDGE_NODE_NAME}} (Modified)
...

$ kubectl apply -f deployment.yaml
```

<br>

- Check the device information on the **Master Node** check the temperature information being collected.
```
$ kubectl get device temperature -oyaml -w
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

- Subscribe the '$hw/events/device/temperature/twin/update' topic on **Edge Node** to view the datas being transferred.
```
## Install the package below to use the mosquitto_sub command.
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

### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Install](../README.md) > Edge Sample Guide
