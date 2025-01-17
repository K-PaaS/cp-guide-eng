### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > Kubeflow Pipeline Tutorial Guide

<br>

## Table of Contents

1. [Document Overview](#1)  
  1.1. [Purpose](#1.1)  
  1.2. [Scope](#1.2)  
  1.3. [System Configuration Diagram](#1.3)  
  1.4. [References](#1.4)  

2. [Kubeflow Pipeline Tutorial](#2)  
  2.1. [DSL - Control Structures Tutorial](#2.1)  
  2.2. [Data Passing in Python Components Tutorial](#2.2)  

<br>

## <div id='1'> 1. Document Overview

### <div id='1.1'> 1.1. Purpose
This document (Kubeflow Pipeline Tutorial Guide) describes how to execute and verify pipelines using the tutorials available in the Kubeflow Central Dashboard.

<br>

### <div id='1.2'> 1.2. Scope
This document is written based on the Kubeflow Pipeline Tutorial Guide to configure and validate the Kubeflow pipeline tutorial.

<br>

### <div id='1.3'> 1.3. System Configuration Diagram
The system architecture is composed of a Kubernetes Cluster (Master, Worker) environment.
Install the Kubernetes Cluster (Master, Worker) using Kubespray.
The required VM setup consists of **1 Master VM, at least 3 Worker VMs**.

![image 001]

<br>

### <div id='1.4'> 1.4. References
> https://www.kubeflow.org/  
> https://github.com/kubeflow/kubeflow  

<br>

## <div id='2'> 2. Kubeflow Pipeline Tutorial
This guide runs and verifies Kubeflow pipelines using two tutorial pipelines.

- Check the port information for the Kubeflow Central Dashboard.
```
$ kubectl get svc istio-ingressgateway -n istio-system
NAME                   TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)                                                                      AGE
istio-ingressgateway   NodePort   10.233.43.143   <none>        15021:31169/TCP,80:30856/TCP,443:32596/TCP,31400:31165/TCP,15443:30988/TCP   25d
```

- Log into the Kubeflow Central Dashboard. The default email is **user@example.com**, and the password is **12341234**.

![image 002]

- Access the Kubeflow Central Dashboard and go to the **Pipelines** menu.

<br>

### <div id='2.1'> 2.1. DSL - Control Structures Tutorial
This tutorial pipeline demonstrates how to use conditional execution and exit handlers in the DSL control structures tutorial.

- Click on "[Tutorial] DSL - Control structures" under the Pipeline name section.

![image 003]

- Click on the **Create experiment** button at the top right.

![image 004]

- Enter an **Experiment name** and click the Next button.

![image 005]

- Click the Start button at the bottom of the **Start a run** screen.

![image 006]

- In the **Runs** menu, click on the **Run name** field to check the pipeline execution status.

![image 007]

- Check the pod list during the pipeline execution.
```
$ kubectl get pods -n kubeflow-user-example-com
NAME                                                                READY   STATUS             RESTARTS   AGE
conditional-execution-pipeline-with-exit-handler-g2gml-1108718281   0/2     Completed          0          8m36s
conditional-execution-pipeline-with-exit-handler-g2gml-2196565435   0/1     Completed          0          5m1s
conditional-execution-pipeline-with-exit-handler-g2gml-2475178693   0/2     Completed          0          5m23s
conditional-execution-pipeline-with-exit-handler-g2gml-585293224    0/2     Completed          0          10m
conditional-execution-pipeline-with-exit-handler-g2gml-893365874    0/2     Completed          0          8m10s
```

<br>

### <div id='2.2'> 2.2. Data Passing in Python Components Tutorial
This tutorial pipeline demonstrates how to pass data between Python components.

- Click on "[Tutorial] Data passing in python components" under the Pipeline name section.

![image 003]

- Click on the **Create experiment** button at the top right.

![image 008]

- Enter an **Experiment name** and click the Next button.

![image 005]

- Click the Start button at the bottom of the **Start a run** screen.

![image 009]

- In the **Runs** menu, click on the **Run name** field to check the pipeline execution status.

![image 010]

- Check the pod list during the pipeline execution.
```
$ kubectl get pods -n kubeflow-user-example-com
NAME                                                                READY   STATUS             RESTARTS   AGE
file-passing-pipelines-xnnp6-1960832278                             0/2     Completed          0          20m   10.233.70.252   cp-dev-cluster-3   <none>           <none>
file-passing-pipelines-xnnp6-2194305122                             0/2     Completed          0          19m   10.233.70.207   cp-dev-cluster-3   <none>           <none>
file-passing-pipelines-xnnp6-2419874560                             0/2     Completed          0          20m   10.233.70.250   cp-dev-cluster-3   <none>           <none>
file-passing-pipelines-xnnp6-2436652179                             0/2     Completed          0          19m   10.233.70.242   cp-dev-cluster-3   <none>           <none>
file-passing-pipelines-xnnp6-2520540274                             0/2     Completed          0          19m   10.233.70.219   cp-dev-cluster-3   <none>           <none>
file-passing-pipelines-xnnp6-3053816171                             0/2     Completed          0          20m   10.233.70.241   cp-dev-cluster-3   <none>           <none>
file-passing-pipelines-xnnp6-3667305124                             0/2     Completed          0          20m   10.233.70.253   cp-dev-cluster-3   <none>           <none>
file-passing-pipelines-xnnp6-595339866                              0/2     Completed          0          20m   10.233.70.240   cp-dev-cluster-3   <none>           <none>
```

<br>

[image 001]:images/standalone-v1.2.png

[image 002]:images/kubeflow-login.png
[image 003]:images/kubeflow-pipelines.png

[image 004]:images/kubeflow-pipelines-dsl-001.png
[image 005]:images/kubeflow-pipelines-dsl-002.png
[image 006]:images/kubeflow-pipelines-dsl-003.png
[image 007]:images/kubeflow-pipelines-dsl-004.png

[image 008]:images/kubeflow-pipelines-python-001.png
[image 009]:images/kubeflow-pipelines-python-002.png
[image 010]:images/kubeflow-pipelines-python-003.png

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Install](/install-guide/README.md) > Kubeflow Pipeline Tutorial Guide