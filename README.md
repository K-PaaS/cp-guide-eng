# K-PaaS container platform

<table>
  <tr>
    <td colspan=2 align=center>Platform</td>
    <td colspan=2 align=center><a href="https://github.com/PaaS-TA/paasta-deployment">Application platform</a></td>
    <td colspan=2 align=center><a href="https://github.com/PaaS-TA/paas-ta-container-platform">🚩 Container Platform</a></td>
  </tr>
  <tr>
    <td colspan=2 rowspan=2 align=center>Portal</td>
    <td colspan=2 align=center><a href="https://github.com/PaaS-TA/portal-deployment">AP Portal</a></td>
    <td colspan=2 align=center><a href="./install-guide/container-platform-portal/cp-portal-deployment-standalone-guide.md">CP Portal</a></td>
  </tr>
  <tr align=center>
    <td colspan=4><a href="https://github.com/PaaS-TA/PaaS-TA-Monitoring">Monitoring Dashboard</a></td>
  </tr>
  <tr align=center>
    <td rowspan=2 colspan=2><a href="https://github.com/PaaS-TA/monitoring-deployment">Monitoring</a></td>
    <td><a href="https://github.com/PaaS-TA/PaaS-TA-Monitoring-Release">Monitoring</a></td>
    <td><a href="https://github.com/PaaS-TA/paas-ta-monitoring-logsearch-release">Logsearch</a></td>
    <td><a href="https://github.com/PaaS-TA/paas-ta-monitoring-influxdb-release">InfluxDB</a></td>
    <td><a href="https://github.com/PaaS-TA/paas-ta-monitoring-redis-release">Redis</a></td>
  </tr>
  <tr align=center>
    <td><a href="https://github.com/PaaS-TA/PAAS-TA-PINPOINT-MONITORING-RELEASE">Pinpoint</td>
    <td><a href="https://github.com/PaaS-TA/PAAS-TA-PINPOINT-MONITORING-BUILDPACK">Pinpoint Buildpack</td>
    <td></td>
    <td></td>
  </tr>
  </tr>
  <tr align=center>
    <td rowspan=4 colspan=2><a href="https://github.com/PaaS-TA/service-deployment">AP Service</a></td>
    <td><a href="https://github.com/PaaS-TA/PAAS-TA-CUBRID-RELEASE">Cubrid</a></td>
    <td><a href="https://github.com/PaaS-TA/PAAS-TA-API-GATEWAY-SERVICE-RELEASE">Gateway</a></td>
    <td><a href="https://github.com/PaaS-TA/PAAS-TA-GLUSTERFS-RELEASE">GlusterFS</a></td>
    <td><a href="https://github.com/PaaS-TA/PAAS-TA-APP-LIFECYCLE-SERVICE-RELEASE">Lifecycle</a></td>
  </tr>
  <tr align=center>
    <td><a href="https://github.com/PaaS-TA/PAAS-TA-LOGGING-SERVICE-RELEASE">Logging</a></td>
    <td><a href="https://github.com/PaaS-TA/PAAS-TA-MONGODB-SHARD-RELEASE">MongoDB</a></td>
    <td><a href="https://github.com/PaaS-TA/PAAS-TA-MYSQL-RELEASE">MySQL</a></td>
    <td><a href="https://github.com/PaaS-TA/PAAS-TA-PINPOINT-RELEASE">Pinpoint APM</a></td>
  </tr>
  <tr align=center>
    <td><a href="https://github.com/PaaS-TA/PAAS-TA-DELIVERY-PIPELINE-RELEASE">Pipeline</a></td>
    <td align=center><a href="https://github.com/PaaS-TA/rabbitmq-release">RabbitMQ</a></td>
    <td><a href="https://github.com/PaaS-TA/PAAS-TA-ON-DEMAND-REDIS-RELEASE">Redis</a></td>
    <td><a href="https://github.com/PaaS-TA/PAAS-TA-SOURCE-CONTROL-RELEASE">Source Control</a></td>
  </tr>
  <tr align=center>
    <td><a href="https://github.com/PaaS-TA/PAAS-TA-WEB-IDE-RELEASE-NEW">WEB-IDE</a></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr align=center>
    <td rowspan=1 colspan=2><a href="https://github.com/K-PaaS/container-platform">CP Service</a></td>
    <td><a href="./install-guide/pipeline/cp-pipeline-service-guide.md">Pipeline</a></td>
    <td><a href="./install-guide/source-control/cp-source-control-service-guide.md">Source Control</a></td>
    <td></td>
    <td></td>
  </tr>
</table>
<i>🚩 You are here.</i>


<br>

## 소개
It covers native Kubernetes installation guides (Kubespray installation, KubeEdge installation) and installation and usage guides on how to deploy and use the container platform on Kubernetes.
<br>

## Install

### Stand-alone Deployment   
- Cluster Installation
  + [Cluster Installation Guide](./install-guide/standalone/cp-cluster-install.md)
  + [Installation and Deployment File](https://github.com/K-PaaS/cp-deployment/tree/master/standalone)
- Portal Installation
  + [Portal Installation Guide](./install-guide/container-platform-portal/cp-portal-deployment-standalone-guide.md)  
  + [Keycloak TLS Setting Guide](./install-guide/container-platform-portal/cp-portal-deployment-keycloak-tls-setting-guide.md)  
  + [Installation and Deployment File](https://github.com/K-PaaS/cp-portal-release)  
  + [Release File](https://github.com/K-PaaS/cp-portal-release/tree/master/portal)
- Service Installation
  + [Pipeline Installation Guide](./install-guide/pipeline/cp-pipeline-standalone-guide.md)
  + [SourceControl Installation Guide](./install-guide/source-control/cp-source-control-standalone-guide.md)

### Service Deployment
- Cluster Installation
  + [Cluster Installation Guide](./install-guide/standalone/cp-cluster-install.md)  
  + [Installation and Deployment File](https://github.com/K-PaaS/cp-deployment/tree/master/standalone)
- Portal Installation
  + [Portal Installation Guide](./install-guide/container-platform-portal/cp-portal-deployment-service-guide.md)
  + [Keycloak TLS Setting Guide](./install-guide/container-platform-portal/cp-portal-deployment-keycloak-tls-setting-guide.md)  
  + [Installation and Deployment File](https://github.com/K-PaaS/cp-portal-release)     
  + [Release File](https://github.com/K-PaaS/cp-portal-release)
- Service Installation
  + [Pipeline Installation Guide](./install-guide/pipeline/cp-pipeline-service-guide.md)
  + [SourceControl Installation Guide](./install-guide/source-control/cp-source-control-service-guide.md)

### Edge Deployment
- Edge Installation
  + [Edge Installation Guide](./install-guide/edge/cp-edge-install.md)
  + [Installation and Deployment File](https://github.com/K-PaaS/cp-deployment/tree/master/edge)
- Portal Installation
  + [Portal Installation Guide](./install-guide/container-platform-portal/cp-portal-deployment-standalone-guide.md)
  + [Keycloak TLS Setting Guide](./install-guide/container-platform-portal/cp-portal-deployment-keycloak-tls-setting-guide.md)  
  + [Installation and Deployment File](https://github.com/K-PaaS/cp-portal-release)     
  + [Release File](https://github.com/K-PaaS/cp-portal-release)
- Service Installation
  + [Pipeline Installation Guide](./install-guide/pipeline/cp-pipeline-service-guide.md)
  + [SourceControl Installation Guide](./install-guide/source-control/cp-source-control-service-guide.md)
- Sample Model
  + [Web Counting / Real-time Temperature Collect](./install-guide/edge/cp-edge-sample-guide.md)



<br>

## Use

### Portal Use Guide
- Operator Portal
  + [Operator Portal Use Guide](./use-guide/portal/cp-admin-portal-guide.md)
- User Portal
  + [User Portal Use Guide](./use-guide/portal/cp-user-portal-guide.md) 

### Service Use Guide
- Pipeline Service
  + [Pipeline Service Use Guide](./use-guide/pipeline/cp-pipeline-use-guide.md)
- Source Control Service
  + [Source Control Service Use Guide](./use-guide/source-control/cp-source-control-use-guide.md)


<br>

## Project

### portal project 
- [container-platform-api](https://github.com/K-PaaS/cp-portal-release/tree/v1.4.x/portal/paas-ta-container-platform-api)  
- [container-platform-common-api](https://github.com/K-PaaS/cp-portal-release/tree/v1.4.x/portal/paas-ta-container-platform-common-api)
- [container-platform-webadmin](https://github.com/K-PaaS/cp-portal-release/tree/v1.4.x/portal/paas-ta-container-platform-webadmin)
- [container-platform-webuser](https://github.com/K-PaaS/cp-portal-release/tree/v1.4.x/portal/paas-ta-container-platform-webuser)
- [container-platform-portal-service-broker](https://github.com/K-PaaS/cp-portal-release/tree/v1.4.x/service-broker/paas-ta-container-platform-admin-service-broker)


<br>

## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):
<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
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
