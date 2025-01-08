### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Use](../Readme.md) >  [Portal Use Guide](./cp-portal-use-guide.md) >  Chaos Menu

<br>

## Table of Contents

1. [Chaos Menu](#1)  
   1.1. [Experiments](#1-1)  
   1.1.1. [Get Experiment List](#1-1-1)  
   1.1.2. [Create Experiment](#1-1-2)  
   1.1.3. [Experiment Details](#1-1-3)  
   1.1.4. [Delete Experiment](#1-1-4)  
   1.2. [Events](#1-2)   
   1.2.1. [Get Event](#1-2-1)  
<br>

## <div id='1'/> 1. Chaos menu
Chaos provides different types of experiments to simulate anomalies that may actually occur in production (pod termination, network latency, etc.) and monitor the status of the experiment.
The types of experiments include 'Pod Fault', 'Network Attack', and 'Stress Test'.

### <div id='1-1'/> 1.1. Experiments
#### <div id='1-1-1'/> 1.1.1. Get Experiment List
- Get a list of Chaos Experiments.
  ![IMG_10_1_1_1]

<br>

#### <div id='1-1-2'/> 1.1.2. Create Experiment
- Click the Create button in the Experiment list to go to the Create Chaos Experiment page.
- On the Create Chaos Experiment page, you can create different types of chaos experiments according to the 'Pod Fault', 'Network Attack', and 'Stress Test' tabs.

<br>

- Pod Fault
    - This is an experiment to see if a Pod can be recovered after being deleted.
      ![IMG_10_1_1_2_1]

      <table>
      <thead>
        <tr>
          <th>Item</th>
          <th>Description</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Name</td>
          <td>Name of the Pod Fault experiment. Can be any combination of lowercase letters and numbers or only lowercase letters.</td>
        </tr>
        <tr>
          <td>Namespace</td>
          <td>Select the Namespace where the chaos resources for the Pod Fault experiment will be created.</td>
        </tr>
        <tr>
          <td>Grace period</td>
          <td>Optional, the number of seconds given for the pod to shut down normally. Defaults to 0, and can only be a number greater than or equal to 0.</td>
        </tr>
        <tr>
          <td>Namespace Selector</td>
          <td>Pod Fault Specifies the Namespace that the Pod to be experimented with belongs to.</td>
        </tr>
        <tr>
          <td>Label Selector</td>
          <td>Pod Fault Specifies the Label Selector that the Pod under experimentation has.</td>
        </tr>
        <tr>
          <td>Preview of Pods to be injected</td>
          <td>Pod Fault You can select up to six Pods to test.</td>
        </tr>
      </tbody>
      </table>

<br>

- Network Attack
    - An experiment that introduces network delays.
      ![IMG_10_1_1_2_2]

      <table>
      <thead>
        <tr>
          <th>Item</th>
          <th>Description</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Name</td>
          <td>Network Attack experiment name. Can be any combination of lowercase letters and numbers or only lowercase letters.</td>
        </tr>
        <tr>
          <td>Namespace</td>
          <td>Select the Namespace where the chaos resources for the Network Attack experiment will be created.</td>
        </tr>
        <tr>
          <td>Latency</td>
          <td>Enter a number for how long you want the network delay to be and select a time unit.</td>
        </tr>
        <tr>
          <td>Duration</td>
          <td>Enter a number for how long you want the chaos experiment to last and select the time units. You can experiment for up to 12 hours.</td>
        </tr>
        <tr>
          <td>Namespace Selector</td>
          <td>Specify the Namespace to which the Network Attack test pod belongs.</td>
        </tr>
        <tr>
          <td>Label Selector</td>
          <td>Specify the Label Selector that the Network Attack experiment target pod has.</td>
        </tr>
        <tr>
          <td>Preview of Pods to be injected</td>
          <td>You can select up to six pods to experiment with Network Attack.</td>
        </tr>
      </tbody>
      </table>

<br>

-  Stress Test
- This is an experiment to load selected Pods by allocating CPU and Memory to them.
  ![img_10_1_1_1_2_3]
- You can load both CPU and Memory, or select one or the other to load.
- If the duration is longer than 30 seconds, metrics are collected and can be viewed in a chart.
- Metrics may not be collected smoothly if the resource is not in a healthy state.

    <table>
    <thead>
      <tr>
        <th>Item</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Name</td>
        <td>Stress Test experiment name. Can be a combination of lowercase letters and numbers or just lowercase letters.</td>
      </tr>
      <tr>
        <td>Namespace</td>
        <td>Stress Test 실험을 위한 카오스 리소스가 만들어지는 Namespace를 선택한다.</td>
      </tr>
      <tr>
        <td>CPU Workers</td>
        <td>Specifies the number of threads to support the CPU load.</td>
      </tr>
      <tr>
        <td>CPU Load</td>
        <td>Specifies the percentage of the CPU to be occupied. CPU load final total = workers * load</td>
      </tr>
      <tr>
        <td>Memory Workers</td>
        <td>Specifies the number of threads to support memory load.</td>
      </tr>
      <tr>
        <td>Memory Size</td>
        <td>1 The amount of memory to be consumed per worker. Defaults to the total available memory size.</td>
      </tr>
      <tr>
        <td>Duration</td>
        <td>Enter a number for how long you want the chaos experiment to last, and then select a time unit. You can experiment for up to 12 hours. If it's less than 30 seconds, the chaos experiment will still run, but you won't be able to collect metrics.</td>
      </tr>
      <tr>
        <td>Namespace Selector</td>
        <td>Specifies the Namespace to which the Pod under stress test belongs.</td>
      </tr>
      <tr>
        <td>Label Selector</td>
        <td>Specifies the Label Selector that the Stress Test Pod belongs to.</td>
      </tr>
      <tr>
        <td>Preview of Pods to be injected</td>
        <td>You can select up to six Pods to inject into the Stress Test experiment.</td>
      </tr>
    </tbody>
    </table>

<br>

#### <div id='1-1-3'/> 1.1.3. Experiment Details
- Click the experiment name in the Experiments list to go to the experiment details page.
  ![IMG_10_1_1_3_1]

<br>

- If it's a Stress Test Experiment, you can see additional charts that you can monitor.
    - Up to six resources in the chart are shown.
    - If any of the target resources have HPA enabled, the first metric, "Resource usage by selected Pods during chaos", increases the number of pods for which the metric is collected by HPA and might not show a chart of all selected pods in the Chaos Experiment. (The chart shows a maximum of six resources.)
    - If a Pod loaded by the Chaos Experiment is not accessible, it will be blank in APP Status, and if it is accessible, it will be colored.
      ![IMG_10_1_1_3_2]

<br>

#### <div id='1-1-4'/> 1.1.4. Delete Experiment
- Click the Delete button at the bottom of the Experiment details page to delete the experiment.
  ![IMG_10_1_1_4_1]

<br>

### <div id='1-2'/> 1.2. Events
#### <div id='1-2-1'/> 1.2.1. Get Event
- Get a Chaos Event.
  ![IMG_10_1_2_1]


<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Use](../Readme.md) >  [Portal Use Guide](./cp-portal-use-guide.md) >  Chaos Menu
[IMG_10_1_1_1]:../images/portal/IMG_10_1_1_1.png
[IMG_10_1_1_2_1]:../images/portal/IMG_10_1_1_2_1.png
[IMG_10_1_1_2_2]:../images/portal/IMG_10_1_1_2_2.png
[IMG_10_1_1_2_3]:../images/portal/IMG_10_1_1_2_3.png
[IMG_10_1_1_3_1]:../images/portal/IMG_10_1_1_3_1.png
[IMG_10_1_1_3_2]:../images/portal/IMG_10_1_1_3_2.png
[IMG_10_1_1_4_1]:../images/portal/IMG_10_1_1_4_1.png
[IMG_10_1_2_1]:../images/portal/IMG_10_1_2_1.png
