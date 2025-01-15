# K-PaaS container platform

## Related Repositories

<table>
<thead>
  <tr>
    <th width="100">Platforms</th>
    <th width="250"><a href="https://github.com/K-PaaS/cp-deployment">Container platform</a></th>
    <th width="250">&nbsp;&nbsp;&nbsp;<a href="https://github.com/K-PaaS/sidecar-deployment.git">Sidecar</a></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td align="center">Portal</td>
    <td align="center"><a href="https://github.com/K-PaaS/cp-portal-release">CP Portal</a></td>
    <td align="center"><a href="https://github.com/K-PaaS/sidecar-deployment/tree/master/install-scripts/portal">Sidecar Portal</a></td>
  </tr>
  <tr>
    <td rowspan="8">Component <br>/Services</td>
    <td align="center"><a href="https://github.com/K-PaaS/cp-portal-ui">Portal UI</a></td>
    <td align="center"><a href="https://github.com/K-PaaS/sidecar-portal-ui">Portal UI</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/K-PaaS/cp-portal-api">Portal API</a></td>
    <td align="center"><a href="https://github.com/K-PaaS/sidecar-portal-api">Portal API</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/K-PaaS/cp-portal-common-api">Common API</a></td>
    <td align="center"></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/K-PaaS/cp-metrics-api">Metric API</a></td>
    <td align="center"></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/K-PaaS/cp-terraman">Terraman API</a></td>
    <td align="center"></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/K-PaaS/cp-catalog-api">Catalog API</a></td>
    <td align="center"></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/K-PaaS/cp-chaos-api">Chaos API</a></td>
    <td align="center"></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/K-PaaS/cp-chaos-collector">Chaos Collector API</a></td>
    <td align="center"></td>
  </tr>
</tbody></table>
<i>🚩 You are here.</i>

## Notice
#### The path of the release has changed from https://nextcloud.paas-ta.org/ to https://nextcloud.k-paas.org/


<br>

## Introduction
It covers guides to installing native Kubernetes (installing Kubespray, installing KubeEdge), as well as guides to various installation methods and utilization guides on how you can deploy and use container platforms on Kubernetes.

<br>

## Install

### Single Cloud Deployment
- Installing a cluster
  + [Cluster Installation Guide](install-guide/standalone/cp-cluster-install-single.md)
- Installing the portal
  + [Portal Installation Guide](install-guide/portal/cp-portal-standalone-guide.md)
  + [Installation and Deployment Files](https://github.com/K-PaaS/cp-helm-chart)
  + [Release Files](https://github.com/K-PaaS/cp-portal-release)
- Installing services
  + [Pipeline Installation Guide](install-guide/pipeline/cp-pipeline-standalone-guide.md)
  + [Source Control Installation Guide](install-guide/source-control/cp-source-control-standalone-guide.md)
- Kubeflow Tutorials
  + [Kubeflow Pipeline Tutorial Guide](install-guide/standalone/cp-kubeflow-sample-guide.md)

### Multi Cloud Deployment
- Installing a cluster
  + [Cluster Installation Guide](install-guide/standalone/cp-cluster-install-multi.md)
- Installing the portal
  + [Portal Installation Guide](install-guide/portal/cp-portal-standalone-guide-mc.md)
  + [Installation and Deployment Files](https://github.com/K-PaaS/cp-helm-chart)
  + [Release Files](https://github.com/K-PaaS/cp-portal-release)
- Installing services
  + [Pipeline Installation Guide](install-guide/pipeline/cp-pipeline-standalone-guide.md)
  + [Source Control Installation Guide](install-guide/source-control/cp-source-control-standalone-guide.md)
- Leveraging the container platform Kubernetes
  + [Linkerd Installation Guide](install-guide/multicluster/cp-linkerd-install.md)
  + [Karmada Installation Guide](install-guide/multicluster/cp-karmada-install.md)
- Utilize CSP Kubernetes Service
  + [Istio Multi-Cluster Installation Guide](install-guide/csp/cp-csp-istio-guide.md)
  + [Linkerd Multi-Cluster Installation Guide](install-guide/csp/cp-csp-linkerd-guide.md)

### Edge Deployment
- Install Edge
  + [Edge Installation Guide](install-guide/edge/cp-edge-install.md)
- Installing the portal
  + [Portal Installation Guide](install-guide/portal/cp-portal-standalone-guide.md)
  + [Installation and Deployment Files](https://github.com/K-PaaS/cp-helm-chart/tree/master)
  + [Release Files](https://github.com/K-PaaS/cp-portal-release/tree/master)
- Installing services
  + [Pipeline Installation Guide](install-guide/pipeline/cp-pipeline-standalone-guide.md)
  + [Source Control Installation Guide](install-guide/source-control/cp-source-control-standalone-guide.md)
- Sample models
  + [Web Counting / Real-Time Temperature Collection](install-guide/edge/cp-edge-sample-guide.md)


<br>

## Use

### Portal Use Guide
+ [Portal Use guide](use-guide/portal/cp-portal-use-guide.md)
+ [Terraman Use Guide](use-guide/terraman/cp-terraman-guide.md)

### Service Use Guide
- Pipeline services
  + [Pipeline Service Use Guide](use-guide/pipeline/cp-pipeline-use-guide.md)
- Source Control Services
  + [Source Control Service Use Guide](use-guide/source-control/cp-source-control-use-guide.md)


<br>

## Project

### Portal projects
- [cp-portal-ui](https://github.com/K-PaaS/cp-portal-ui)
- [cp-portal-api](https://github.com/K-PaaS/cp-portal-api)
- [cp-portal-common-api](https://github.com/K-PaaS/cp-portal-common-api)
- [cp-metrics-api](https://github.com/K-PaaS/cp-metrics-api)
- [cp-terraman](https://github.com/K-PaaS/cp-terraman)
- [cp-catalog-api](https://github.com/K-PaaS/cp-catalog-api)
- [cp-chaos-api](https://github.com/K-PaaS/cp-chaos-api)
- [cp-chaos-collector](https://github.com/K-PaaS/cp-chaos-collector)

<br>

## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):
<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="https://github.com/Luna77877"><img src="https://avatars.githubusercontent.com/u/107905603?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Luna</b></sub></a><br /><a href="#ideas-luna77877" title="Ideas, Planning, & Feedback">💻👀</a></td>
    <td align="center"><a href="https://github.com/jinyung0101java2"><img src="https://avatars.githubusercontent.com/u/67574725?v=4?s=100" width="100px;" alt=""/><br /><sub><b>JinYoung Jang</b></sub></a><br /><a href="https://github.com/PaaS-TA/paas-ta-container-platform/commits?author=jinyung0101java2" title="Code">💻</a> <a href="https://github.com/PaaS-TA/paas-ta-container-platform/pulls?q=is&Apr+reviewed-by&jinyung0101java2" title="Reviewed Pull Requests">👀</a></td>
    <td align="center"><a href="https://github.com/hoon77"><img src="https://avatars.githubusercontent.com/u/33216551?v=4?s=100" width="100px;" alt=""/><br /><sub><b>JiHoon Kang</b></sub></a><br /><a href="https://github.com/PaaS-TA/paas-ta-container-platform/commits?author=hoon77" title="Code">💻</a> <a href="https://github.com/PaaS-TA/paas-ta-container-platform/pulls?q=is&Apr+reviewed-by&hoon77" title="Reviewed Pull Requests">👀</a></td>
    <td align="center"><a href="https://github.com/suslmk-lee"><img src="https://avatars.githubusercontent.com/u/67575226?v=4?s=100" width="100px;" alt=""/><br /><sub><b>suslmk</b></sub></a><br /><a href="#maintenance-suslmk" title="Maintenance">🚧</a></td>
    <td align="center"><a href="https://github.com/dev-taewoo"><img src="https://avatars.githubusercontent.com/u/67407365?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Taewoo Kim</b></sub></a><br /><a href="https://github.com/PaaS-TA/paas-ta-container-platform/commits?author=dev-taewoo" title="Code">💻</a></td>
    <td align="center"><a href="https://github.com/rexx4314"><img src="https://avatars.githubusercontent.com/u/26153262?v=4?s=100" width="100px;" alt=""/><br /><sub><b>rexx4314</b></sub></a><br /><a href="#ideas-rexx4314" title="Ideas, Planning, & Feedback">🤔</a></td>
    <td align="center"><a href="https://github.com/opdc-minsu"><img src="https://avatars.githubusercontent.com/u/67140002?v=4?s=100" width="100px;" alt=""/><br /><sub><b>MinSu Kang</b></sub></a><br /><a href="https://github.com/PaaS-TA/paas-ta-container-platform/issues?q=author&opdc-minsu" title="Bug reports">🐛</a></td>
    <td align="center"><a href="https://github.com/jhuhm135"><img src="https://avatars.githubusercontent.com/u/70005316?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Juhyun Um</b></sub></a><br /><a href="#ideas-jhuhm135" title="Ideas, Planning, & Feedback">🤔</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/kyuminhan"><img src="https://avatars.githubusercontent.com/u/80228983?v=4?s=100" width="100px;" alt=""/><br /><sub><b>KyuMin Han</b></sub></a><br /><a href="#ideas-kyuminhan" title="Ideas, Planning, & Feedback">🤔</a></td>
  </tr>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

<br>

## License
Use [Apache-2.0 License](http://www.apache.org/licenses/LICENSE-2.0)
