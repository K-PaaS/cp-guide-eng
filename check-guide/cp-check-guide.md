## Table of Contents
1. [Document Overview](#1)    
   1.1 [Purpose](#1.1)    
   1.2 [Caution](#1.2)  

2. [CCE Diagnosis Items](#2)  
    2.1 [Restriction of Remote Access for the root Account](#2.1)    
    2.2 [Setting Password Complexity](#2.2)    
    2.3 [Setting Account Lock Threshold](#2.3)    
    2.4 [Setting File and Directory Ownerships](#2.4)    
    2.5 [/etc/shadow File Owner and Permission Settings](#2.5)    
    2.6 [Checking SUID, SGID, Sticky bit Settings for Files](#2.6)    
    2.7 [Checking world-writable Files](#2.7)    
    2.8 [Docker Daemon Audit Settings](#2.8)    
    2.9 [/usr/lib/docker Audit Settings](#2.9)    
    2.10 [/etc/docker Audit Settings](#2.10)    
    2.11 [docker.service Audit Settings](#2.11)    
    2.12 [docker.socket Audit Settings](#2.12)    
    2.13 [/etc/default/docker Audit Settings](#2.13)    
    2.14 [Limiting Network Traffic between Containers via Default Bridge](#2.14)         
    2.15 [Setting DOCKER_CONTENT_TRUST Value](#2.15)     
    2.16 [Setting Maximum Password Usage Period](#2.16)   
    2.17 [Restriction of Docker's Default Bridge docker0 Usage](#2.17)  

3. [CCE Diagnosis Items (Kubernetes Vulnerability Measures as Alternatives to Docker Vulnerabilities)](#3)      
    3.1 [API Server Authentication Control](#3.1)   
    3.2 [API Server Authorization Control](#3.2)   
    3.3 [Controller Manager Authentication Control](#3.3)   
    3.4 [Kubelet Authentication Control](#3.4)   
    3.5 [Kubelet Authorization Control](#3.5)   
    3.6 [Application of Security Profiles for Containers](#3.6)      

4. [CVE Diagnosis Items](#4)  
    4.1 [Deactivation of TCP Timestamp Responses](#4.1)           
    4.2 [Mismatch between Subject CN Field of X.509 Certificate and Entity Name](#4.2)         
    4.3 [Untrusted TLS/SSL Server X.509 Certificates](#4.3)         
    4.4 [Self-Signed TLS/SSL Certificates](#4.4)         

  

## <div id='1'/>1. Document Overview

### <div id='1.1'/>1.1. Purpose

This document provides a technical security guide for CVE and CCE vulnerabilities. Each section consists of item description, action target, diagnosis method, and corrective action. Using this guide, security measures can be taken concerning vulnerability issues.

### <div id='1.2'/>1.2. Caution

The criteria included in this document are used during cloud certification evaluations. The actual criteria for distinguishing between 'good' or 'vulnerable' depend on various environments, policies, etc., according to each cloud service operation and are finally determined by the reviewer.

## <div id='2'/>2. CCE Diagnosis Items

### <div id='2.1'/>2.1. Restriction of Remote Access for the root Account

- Item Description:
  + Unauthorized access to the root account on systems where remote access restriction is not applied can result in breaches such as leakage of system account information, file and directory tampering, etc., if an unauthorized person acquires the root account information through various attacks (random attempts, dictionary attacks, etc.).

- Action Target:

| <center>Environment</center> | <center>Category</center> | <center>Action Target</center> |
| :--- | :--- | :---: |
| Cluster | Master | O |
|| Worker | O |
| Bosh | MariaDB | X |
|| HAProxy | X |
|| Private-image-repository | X |

- Diagnosis Method:
  + Check Root login settings in the /etc/ssh/sshd_config file

```
# cat /etc/ssh/sshd_config | grep PermitRootLogin
```


- Corrective Action:
+ Change the settings in /etc/ssh/sshd_config

```
# vi /etc/ssh/sshd_config

[현황]
#PermitRootLogin prohibit-password

[조치]
PermitRootLogin no
```

---

### <div id='2.2'/>2.2. Setting Password Complexity

- Item Description:
+ If password complexity settings are not in place for user accounts, it poses a risk of unauthorized access to the system using weak passwords obtained through various attacks (random attempts, dictionary attacks, etc.).

- Action Target:

| <center>Environment</center> | <center>Category</center> | <center>Action Target</center> |
| :--- | :--- | :---: |
| Cluster | Master | O |
|| Worker | O |
| Bosh | MariaDB | X |
|| HAProxy | X |
|| Private-image-repository | X |

- Diagnosis Method:
+ Check the settings in the /etc/pam.d/common-password file

```
# cat /etc/pam.d/common-password
```


- Corrective Action:
+ Edit the /etc/pam.d/common-password file
```
# vi /etc/pam.d/common-password

[Current]
password requisite pam_deny.so

[Action]
password requisite pam_pwquality.so enforce_for_root retry=3 minlen=8 dcredit=-1 ucredit=-1 lcredit=-1 ocredit=-1

```

---

### <div id='2.3'/>2.3. Setting Account Lock Threshold

- Item Description:
+ If a login failure threshold is not set, it leaves the system vulnerable to repeated login attempts (random attempts, dictionary attacks, guessing attacks, etc.), risking password exposure due to unrestricted blocking, leading to unauthorized access.

- Action Target:

| <center>Environment</center> | <center>Category</center> | <center>Action Target</center> |
| :--- | :--- | :---: |
| Cluster | Master | O |
|| Worker | O |
| Bosh | MariaDB | X |
|| HAProxy | X |
|| Private-image-repository | X |

- Diagnosis Method:
+ Check threshold settings in the /etc/pam.d/common-auth file

```
# cat /etc/pam.d/common-auth
```


- Corrective Action:
+ Change the settings in the /etc/pam.d/common-auth file
```
# vi /etc/pam.d/common-auth
[Current]
auth requisite pam_deny.so

[Action]
auth required pam_tally2.so deny=5 no_magic_root   
```

---

### <div id='2.4'/>2.4. Setting File and Directory Ownerships

- Item Description:
+ When a user with a UID that matches a deleted owner's UID accesses a file or directory, it risks exposing sensitive information.

- Action Target:

| <center>Environment</center> | <center>Category</center> | <center>Action Target</center> |
| :--- | :--- | :---: |
| Cluster | Master | O |
|| Worker | O |
| Bosh | MariaDB | X |
|| HAProxy | X |
|| Private-image-repository | X |

- Diagnosis Method:
+ Search for files and directories on the system that have no owner or group

```
# find / -nouser -o -nogroup
```

- Corrective Action:
+ If unnecessary, delete files or directories with no owner using the 'rm' command
+ If necessary, change ownership and group using the 'chown' command
```
## Good: There are no files or directories without owners or groups
## Vulnerable: There are files or directories without owners or groups

Delete files or directories without owners if unnecessary using the rm command
# rm <file name>
# rm -rf <directory name>

Change ownership and group using the chown command if necessary
# chown <user name> <file name>
```
---

### <div id='2.5'/>2.5. /etc/shadow File Owner and Permission Settings

- Item Description
  + If permission management is not enforced for this file, ID and password information can be exposed externally.

- Action Targets

| <center>Target Environment</center> | <center>Classification</center> | <center>Action Required</center> |
| :--- | :--- | :---: |
| Cluster | Master | O |
|| Worker | O |
| Bosh | MariaDB | X |
|| HAProxy | X |
|| Private-image-repository | X |

- Diagnostic Method
  + Check the permissions and owner of the /etc/shadow file
```
# ls -l /etc/shadow
```


- Action Plan
  + Change the owner and permissions of the /etc/shadow file (owner: root, permissions: 400)
```
[Current Status]
e.g., File obtained from search
-rw-r----- 1 root shadow 863 Jan 28 06:18 /etc/shadow

[Action]
e.g., Change the owner and permissions of the file obtained from the search
# chown root /etc/shadow
# chmod 400 /etc/shadow
```

---

### <div id='2.6'/>2.6. SUID, SGID, Sticky Bit File Check

- Item Description
  + Improper access permissions for SUID, SGID files can lead to obtaining root privileges and causing disruptions in normal services.

- Action Targets

| <center>Target Environment</center> | <center>Classification</center> | <center>Action Required</center> |
| :--- | :--- | :---: |
| Cluster | Master | O |
|| Worker | O |
| Bosh | MariaDB | X |
|| HAProxy | X |
|| Private-image-repository | X |

- Diagnostic Method
  + Use the following command to search for SUID and SGID files and verify the permissions of important files:
```
# find / -user root -type f \( -perm -04000 -o -perm -02000 \) -xdev -exec ls -al {} \;
```


- Action Plan
  + Remove SUID, SGID settings
```
# chmod -s /usr/bin/newgrp
# chmod -s /sbin/unix_chkpwd
# chmod -s /usr/bin/at
```

---

### <div id='2.7'/>2.7. World Writable File Check

- Item Description
  + When system files or similarly critical files are set as world-writable, malicious users can modify or delete these files at will, potentially leading to unauthorized system access and system failures.

- Action Targets

| <center>Target Environment</center> | <center>Classification</center> | <center>Action Required</center> |
| :--- | :--- | :---: |
| Cluster | Master | O |
|| Worker | O |
| Bosh | MariaDB | O |
|| HAProxy | O |
|| Private-image-repository | O |

- Diagnostic Method
  + Check for the existence of world-writable files using the following command:
```
# Note: When diagnosing using the below command, applying the method of taking action only on problematic items when applying CCE diagnosis as too many items might be listed.
# find / -type f -perm -2 -exec ls -l {} \;
```


- Action Plan
  + Method to remove write permissions for regular users
```
# chmod o-w <file name>

1) For problematic items when applying CCE diagnosis on Kubernetes master and worker:
[Current Status]
  4039   4 drwxrwxrwt 10 root root 4096 Jan 5 07:46 /tmp
  256009 4 drwxrwxrwt 2 root root 4096 Jan 2 12:25  /tmp/systemd-private-aae1da4e104642589da1b983608b0bee-systemd-timesyncd.service-0nl8Q4/tmp
  256007 4 drwxrwxrwt 2 root root 4096 Jan 2 12:25  /tmp/.Test-unix
  256005 4 drwxrwxrwt 2 root root 4096 Jan 2 12:25  /tmp/.XIM-unix
  256003 4 drwxrwxrwt 2 root root 4096 Jan 2 12:25  /tmp/.X11-unix
  256004 4 drwxrwxrwt 2 root root 4096 Jan 2 12:25  /tmp/.ICE-unix
  256006 4 drwxrwxrwt 2 root root 4096 Jan 2 12:25  /tmp/.font-unix
  67692  4 drwxrwxrwt 2 root root 4096 Oct 26 17:27 /var/crash
  67674  4 drwxrwxrwt 5 root root 4096 Jan 2 13:01  /var/tmp
  256061 4 drwxrwxrwt 2 root root 4096 Jan 2 12:25  /var/tmp/systemd-private-aae1da4e104642589da1b983608b0bee-systemd-timesyncd.service-jeqO9z/tmp
  256028 4 drwxrwxrwt 2 root root 4096 Jan 2 12:26 /var/tmp/cloud-init

[Action]
# chmod o-w /tmp
# chmod o-w /tmp/.Test-unix
# chmod o-w /tmp/.XIM-unix
# chmod o-w /tmp/.X11-unix
# chmod o-w /tmp/.ICE-unix
# chmod o-w /tmp/.font-unix
# chmod o-w /tmp/systemd*/tmp
# chmod o-w /var/crash
# chmod o-w /var/tmp
# chmod o-w /var/tmp/cloud-init
# chmod o-w /var/tmp/systemd*/tmp
-------------------------------------------------------------------------------------------

2) Items requiring action when applying CCE diagnosis on mariadb, haproxy, private-image-repository:

[Current Status]
 4039      4 drwxrwxrwt  10 root     root         4096 Jan 29 06:15 /tmp
67674      4 drwxrwxrwt   5 root     root         4096 Jan 28 04:52 /var/tmp

[Action]
# Access to BOSH Inception Environment Instance VM
e.g., {Instance VM Name} : mariadb (input the three VMs above)
$ bosh -e <bosh_name> -d container-platform ssh {Instance VM Name}

# chmod o-w /tmp
# chmod o-w /var/tmp
```
---

### <div id='2.8'/>2.8. Docker Daemon Audit Configuration

- From section 2.8 Docker daemon audit configuration to section 2.13 /etc/default/docker audit settings, modifications are made to the same file `audit.rules`, so the settings under the '## Add at the bottom' section should be continuously added below.

- Item Description
  + The Docker daemon runs with root privileges, necessitating the auditing of its activities and purposes.

- Target of Action

| Environment | Category | Target of Action |
| :--- | :--- | :---: |
| Cluster | Master | ✓ |
|           | Worker | ✓ |
| Bosh      | MariaDB | ❌ |
|           | HAProxy | ❌ |
|           | Private-image-repository | ❌ |

- Diagnostic Methods
  + Checking the audit configuration for `/usr/bin/docker` using the following command:
    ```
    # auditctl -l | grep /usr/bin/docker
    ```
  + Verifying the contents of the file `/etc/audit/audit.rules` for `/usr/bin/docker` using the command:
    ```
    # cat /etc/audit/audit.rules | grep /usr/bin/docker
    ```

- Remediation Steps
  + Applying the audit configuration:
    ```
    Apply the settings as follows:
    # apt install auditd -y
    # vi /etc/audit/rules.d/audit.rules

    ## Add at the bottom
    -w /usr/bin/docker -k docker

    # systemctl restart auditd.service
    ```

---

### <div id='2.9'/>2.9. /usr/lib/docker Audit Settings

- Item Description
  + The /var/lib/docker directory holds all information about containers, necessitating audit settings.

- Target of Action

| Environment | Category | Target of Action |
| :--- | :--- | :---: |
| Cluster | Master | ✓ |
|           | Worker | ✓ |
| Bosh      | MariaDB | ❌ |
|           | HAProxy | ❌ |
|           | Private-image-repository | ❌ |

- Diagnostic Methods
  + Checking the audit configuration for `/var/lib/docker` using the following command:
    ```
    # auditctl -l | grep /var/lib/docker
    ```
  + Verifying the contents of the file `/etc/audit/audit.rules` for `/var/lib/docker` using the command:
    ```
    # cat /etc/audit/audit.rules | grep /var/lib/docker
    ```

- Remediation Steps
  + Applying the audit configuration:
    ```
    Apply the settings as follows:
    # apt install auditd -y
    # vi /etc/audit/rules.d/audit.rules

    ## Add at the bottom
    -w /var/lib/docker -k docker

    # systemctl restart auditd.service
    ```


### <div id='2.10'/>2.10. /etc/docker Audit Settings

- Item Description
  + The /etc/docker directory contains various certificates and keys used for TLS communication between the Docker daemon and Docker clients, requiring audit settings.

- Target of Action

| Environment | Category | Target of Action |
| :--- | :--- | :---: |
| Cluster | Master | ✓ |
|           | Worker | ✓ |
| Bosh      | MariaDB | ❌ |
|           | HAProxy | ❌ |
|           | Private-image-repository | ❌ |

- Diagnostic Methods
  + Checking the audit configuration for `/etc/docker` using the following command:
    ```
    # auditctl -l | grep /etc/docker
    ```
  + Verifying the contents of the file `/etc/audit/audit.rules` for `/etc/docker` using the command:
    ```
    # cat /etc/audit/audit.rules | grep /etc/docker
    ```

- Remediation Steps
  + Applying the audit configuration:
    ```
    Apply the settings as follows:
    # apt install auditd -y
    # vi /etc/audit/rules.d/audit.rules

    ## Add at the bottom
    -w /etc/docker -k docker

    # systemctl restart auditd.service
    ```

---

### <div id='2.11'/>2.11. docker.service Audit Settings

- Item Description
  + If daemon parameters are altered by an administrator, the docker.service file exists. This file holds various parameters for the Docker daemon, necessitating audit settings.

- Target of Action

| Environment | Category | Target of Action |
| :--- | :--- | :---: |
| Cluster | Master | ✓ |
|           | Worker | ✓ |
| Bosh      | MariaDB | ❌ |
|           | HAProxy | ❌ |
|           | Private-image-repository | ❌ |

- Diagnostic Methods
  + Checking the path of the docker.service file:
    ```
    # systemctl show -p FragmentPath docker.service
    ```
  + Verifying the audit configuration for docker.service using the command:
    ```
    # auditctl -l | grep /lib/systemd/system/docker.service
    ```
  + Verifying the contents of the file `/etc/audit/audit.rules` for docker.service using the command:
    ```
    # cat /etc/audit/audit.rules | grep docker.service
    ```

- Remediation Steps
  + Applying the audit configuration:
    ```
    Apply the settings as follows:
    # apt install auditd -y
    # vi /etc/audit/rules.d/audit.rules

    ## Add at the bottom
    -w /lib/systemd/system/docker.service -k docker

    # systemctl restart auditd.service
    ```

---

### <div id='2.12'/>2.12. docker.socket Audit Settings

- Item Description
  + The docker.socket file holds various parameters for the Docker daemon socket, requiring audit settings.

- Target of Action

| Environment | Category | Target of Action |
| :--- | :--- | :---: |
| Cluster | Master | ✓ |
|           | Worker | ✓ |
| Bosh      | MariaDB | ❌ |
|           | HAProxy | ❌ |
|           | Private-image-repository | ❌ |  

- Diagnostic Methods
  + Checking the path of the docker.socket file:
    ```
    # systemctl show -p FragmentPath docker.socket
    ```
  + Verifying the audit configuration for docker.socket using the command:
    ```
    # auditctl -l | grep /lib/systemd/system/docker.socket
    ```
  + Verifying the contents of the file `/etc/audit/audit.rules` for docker.socket using the command:
    ```
    # cat /etc/audit/audit.rules | grep docker.socket
    ```

- Remediation Steps
  + Applying the audit configuration:
    ```
    Apply the settings as follows:
    # apt install auditd -y
    # vi /etc/audit/rules.d/audit.rules

    ## Add at the bottom
    -w /lib/systemd/system/docker.socket -k docker

    # systemctl restart auditd.service
    ```



### <div id='2.13'/>2.13. /etc/default/docker Audit Settings

- Item Description
  + The /etc/default/docker file contains various parameters for the Docker daemon, requiring audit settings.

- Target of Action

| Environment | Category | Target of Action |
| :--- | :--- | :---: |
| Cluster | Master | ✓ |
|           | Worker | ✓ |
| Bosh      | MariaDB | ❌ |
|           | HAProxy | ❌ |
|           | Private-image-repository | ❌ |

- Diagnostic Methods
  + Checking the audit configuration for `/etc/default/docker` using the following command:
    ```
    # auditctl -l | grep /etc/default/docker
    ```
  + Verifying the contents of the file `/etc/audit/audit.rules` for `/etc/default/docker` using the command:
    ```
    # cat /etc/audit/audit.rules | grep /etc/default/docker
    ```

- Remediation Steps
  + Applying the audit configuration:
    ```
    Apply the settings as follows:
    # apt install auditd -y
    # vi /etc/audit/rules.d/audit.rules

    ## Add at the bottom
    -w /etc/default/docker -k docker

    # systemctl restart auditd.service
    ```

---

### <div id='2.14'/>2.14. Restricting Network Traffic Among Containers via Default Bridge

- Item Description
  + Communication between containers on the Default Network Bridge on the same host is unrestricted. Therefore, each container can see all network packets of other containers through the host's network, potentially exposing unintended or unwanted information. To prevent this, restrict communication between containers on the Default Network Bridge.

- Target of Action

| Environment | Category | Target of Action |
| :--- | :--- | :---: |
| Cluster | Master (Cloudcore) | ✓ |
|           | Worker (EdgeCore) | ✓ |
| Bosh      | MariaDB | ❌ |
|           | HAProxy | ❌ |
|           | Private-image-repository | ❌ |   

- Diagnostic Methods
  + Check if the container restriction option is applied with the following command:
    ```
    $ docker network ls --quiet | xargs docker network inspect --format "{{ .Name}}: {{.Options }}"
    ```

- Remediation Steps (Standalone Deployment)
  + Add the --icc=false option
    ```
    # vi /etc/systemd/system/docker.service.d/docker-options.conf
    (Change)    --iptables=true
    (Add)    --icc=false

    [Service]
    Environment="DOCKER_OPTS= --iptables=true \
    --icc=false \
    --exec-opt native.cgroupdriver=systemd \
    \
    --data-root=/var/lib/docker \
    --log-opt max-size=50m --log-opt max-file=5 \
    # Docker daemon reload and restart
    # systemctl daemon-reload
    # systemctl restart docker
    ```

- Remediation Steps (Edge Deployment)
  + Add "icc": false
    ```
    # vi /etc/docker/daemon.json
    {
        "icc": false
    }
    # Docker daemon reload and restart
    # systemctl daemon-reload
    # systemctl restart docker
    ```

---

### <div id='2.15'/>2.15. Setting DOCKER_CONTENT_TRUST Value

- Item Description
  + Controls image tampering in Docker.

- Target of Action

| Environment | Category | Target of Action |
| :--- | :--- | :---: |
| Cluster | Master | ✓ |
|           | Worker | ✓ |
| Bosh      | MariaDB | ❌ |
|           | HAProxy | ❌ |
|           | Private-image-repository | ❌ |

- Remediation Steps
  + Modify and apply the settings in the /etc/bash.bashrc file.
  + Set the DOCKER_CONTENT_TRUST item to 1.
    ```
    # vi /etc/bash.bashrc
    ----------------------------------------
    ## Add at the bottom
    export DOCKER_CONTENT_TRUST=1
    ----------------------------------------
    # source /etc/bash.bashrc
    ```

### <div id='2.16'/>2.16. Setting Maximum Password Expiry

- Item Description
  + Without setting a maximum password expiry, unauthorized users can attempt various attacks (random attempts, dictionary attacks, etc.) without time restrictions. This leaves the door open for attackers to conduct long-term attacks, increasing the probability of password exposure proportionally to the duration.

- Target for Action

| <center>Target Environment</center> | <center>Classification</center> | <center>Action Required</center> |
| :--- | :--- | :---: |
| Cluster | Master | ✔ |
|| Worker | ✔ |
| Bosh | MariaDB | ❌ |
|| HAProxy | ❌ |
|| Private-image-repository | ❌ |

- Diagnostic Procedure
  + Check the setting value of the maximum password expiry in the /etc/login.defs file.
  + Check individual account settings.

```
# cat /etc/login.defs | grep PASS_MAX_DAYS

## <Account Name>: e.g., root, ubuntu
# chage -l <Account Name>
```
- Remediation Steps
  + Update settings in the /etc/login.defs file.
  + Modify settings for each account.
--------------------------
1) Update settings in the /etc/login.defs file
```
# vim /etc/login.defs

[Current]
PASS_MAX_DAYS 99999

[Action]
PASS_MAX_DAYS 90
```
--------------------------

2) Modify settings for each account
```
## <Account Name>: e.g., root, ubuntu
# chage -M 90 <Account Name>

[Current]
root
Maximum number of days between password change : 99999

ubuntu
Maximum number of days between password change : 99999

[Action]
root
Maximum number of days between password change : 90

ubuntu
Maximum number of days between password change : 90
```

#### <div id='2.17'/>2.17. Restricting the Use of Docker's Default Bridge 'docker0'

- Item Description
  + Docker connects virtual interfaces created in bridge mode to a common bridge named docker0.
  + This network model is vulnerable to attacks like ARP Spoofing and MAC Flooding due to the lack of applied filtering.

- Measures
  + Kubernetes configures networking between pods using a network overlay via CNI, thereby not utilizing the docker0 bridge (refer to item 1).
  + Considerations for removing the bridge for operational purposes (refer to item 2).

1. Example of Docker0 Bridge Network in Use (before deletion)
   ```bash
   ## Install bridge-utils package for checking bridge usage
   $ sudo apt install bridge-utils

   ## Check Network Bridge
   $ brctl show
   bridge name            bridge id                STP enabled        interfaces
   docker0                8000.024282e2a076        no

   ## Deploy Pods
   $ kubectl apply -f {POD_YAML_FILE}
   $ kubectl get pods
   NAME                                   READY   STATUS    RESTARTS   AGE
   nginx-559d75d76b-d6p84                 1/1     Running   0          11m

   ## Even after deploying pods, check interfaces showing no connection to docker0 bridge.
   $ brctl show
   bridge name            bridge id                STP enabled        interfaces
   docker0                8000.024282e2a076        no

   ## Using Calico CNI Plugin, when deploying Pods, interfaces are assigned to calixxxxxxxxxx.
   ## It confirms the use of the docker0 network bridge.
   
   $ ifconfig
   cali57a63d8e86c: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500 inet6 fe80::ecee:eeff:feee:eeee  prefixlen 64  scopeid 0x20<link>
          ether ee:ee:ee:ee:ee:ee  txqueuelen 0  (Ethernet)
          RX packets 960  bytes 81389 (81.3 KB)
          RX errors 0  dropped 0  overruns 0  frame 0
          TX packets 1035  bytes 536155 (536.1 KB)
          TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
   cali76e9dbd11dd: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
   inet6 fe80::ecee:eeff:feee:eeee  prefixlen 64  scopeid 0x20<link>
   ether ee:ee:ee:ee:ee:ee  txqueuelen 0  (Ethernet)
   RX packets 1858  bytes 159032 (159.0 KB)
   RX errors 0  dropped 0  overruns 0  frame 0
   TX packets 1899  bytes 291632 (291.6 KB)
   TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
   ...skipped...
   docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
   ether 02:42:25:66:af:b5  txqueuelen 0  (Ethernet)
   RX packets 0  bytes 0 (0.0 B)
   RX errors 0  dropped 0  overruns 0  frame 0
   TX packets 0  bytes 0 (0.0 B)
   TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
   ...skipped...
   
   ## Docker Network Verification
   $ sudo docker network ls
   NETWORK ID     NAME      DRIVER    SCOPE
   095c47839397   bridge    bridge    local
   ec24cb1c3ae4   host      host      local
   58219ab61fba   none      null      local
   ```

2. Deletion of Docker0 Bridge Network (Post-Deletion Example)

    ```bash
    ## Configure the removal of Docker0 Bridge Network
    $ sudo vi /etc/docker/daemon.json
    {
          "bridge": "none"
    }

    ## Reload Docker daemon and restart
    $ sudo systemctl daemon-reload
    $ sudo systemctl restart docker

    ## Check Network Bridge
    $ brctl show
    bridge name            bridge id                STP enabled        interfaces

    ## Deploy Pods
    $ kubectl apply -f {POD_YAML_FILE}
    $ kubectl get pods
    NAME                                   READY   STATUS    RESTARTS   AGE
    nginx-559d75d76b-d6p84                 1/1     Running   0          13m
    nginx2-559d75d76b-t24tg                1/1     Running   0          15s

    ## Recheck Network Bridge after Pod deployment 
    $ brctl show
    bridge name            bridge id                STP enabled        interfaces

    ## The docker0 bridge is no longer present.
    $ ifconfig
    cali57a63d8e86c: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
          inet6 fe80::ecee:eeff:feee:eeee  prefixlen 64  scopeid 0x20<link>
          ether ee:ee:ee:ee:ee:ee  txqueuelen 0  (Ethernet)
          RX packets 203  bytes 19527 (19.5 KB)
          RX errors 0  dropped 0  overruns 0  frame 0
          TX packets 244  bytes 245060 (245.0 KB)
          TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

    cali76e9dbd11dd: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
          inet6 fe80::ecee:eeff:feee:eeee  prefixlen 64  scopeid 0x20<link>
          ether ee:ee:ee:ee:ee:ee  txqueuelen 0  (Ethernet)
          RX packets 263  bytes 26006 (26.0 KB)
          RX errors 0  dropped 0  overruns 0  frame 0
          TX packets 264  bytes 92832 (92.8 KB)
          TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    ...skipped...

    ## Docker Network Check
    $ sudo docker network ls
    NETWORK ID     NAME      DRIVER    SCOPE
    ec24cb1c3ae4   host      host      local
    58219ab61fba   none      null      local
    ```

<br>

### <div id='3'/>3. CCE Diagnostic Items (Remediation for Docker Vulnerabilities Using Kubernetes Vulnerability Mitigation)

- Remediation Targets

| Target Environment | Category | Remediation Target | Scope of Application |
| :--- | :--- | :---: | :---: |
| Cluster | Master | O | O |
| | Worker | X | O |

#### <div id='3.1'/>3.1. API Server Authentication Control
- Item Description
  + The API Server serves as the hub for Kubernetes components, providing a REST API based on HTTP and HTTPS. It plays a role in connecting all other components for interaction.
  + Verify the authentication control for the API Server.

- Docker Inspection Target Items
  + Enable Docker client authentication.
  + Limit container execution from acquiring additional permissions.
  + Execute containers with a non-root user.

- Remediation Method
  + The kube-apiserver.yaml file is only present on the Master, so applying CCE diagnosis to the Master will apply security checks to the entire Cluster.
  + Modify the /etc/kubernetes/manifests/kube-apiserver.yaml file.
  + If the following items exist in the file, change the configuration values. If they do not exist, add the configuration values.
  
```yaml
 $ sudo vi /etc/kubernetes/manifests/kube-apiserver.yaml

 ## Disable unauthenticated access
 ----------------------------------------
 --anonymous-auth=false
 Remove --insecure-allow-any-token=true
 Remove --insecure-bind-address=X.X.X.X
 Remove --insecure-port=0
 ----------------------------------------

 ## Remove vulnerable authentication methods
 ----------------------------------------
 Remove --basic-auth-file={filename}
 Remove --token-auth-file={filename}
 ----------------------------------------
```

![image](https://user-images.githubusercontent.com/67575226/106992759-966a6600-67bc-11eb-8d7a-e8a218613d70.png)

#### <div id='3.2'/>3.2. API Server Authorization Control
- Item Description
  + Kubernetes performs approvals for requests based on policies through the API Server.
  + Verify the authorization control for the API Server.

- Docker Inspection Target Items
  + Enable Docker client authentication.
  + Limit container execution from acquiring additional permissions.
  + Execute containers with a non-root user.

- Remediation Method
  + The kube-apiserver.yaml file is only present on the Master, so applying CCE diagnosis to the Master will apply security checks to the entire Cluster.
  + Modify the /etc/kubernetes/manifests/kube-apiserver.yaml file.
  + If the following item exists in the file, change the configuration value. If it does not exist, add the configuration value.

```yaml
 $ sudo vi /etc/kubernetes/manifests/kube-apiserver.yaml

 ## Node permissions, Role-Based Access Control (RBAC) in use
 ----------------------------------------
 --authorization-mode=Node,RBAC
 ----------------------------------------
```
![image](https://user-images.githubusercontent.com/67575226/106992884-d03b6c80-67bc-11eb-9a76-b09b17fe57c9.png)

#### <div id='3.3'/>3.3. Controller Manager Authentication Control
- Item Description
  + In Kubernetes, Controllers monitor the shared state of the cluster via the API Server (control loops that aim to move the current state to the desired state).
  + Verify the Controller Manager authentication control.

- Docker Inspection Target Items
  + Enable Docker client authentication.
  + Limit container execution from acquiring additional permissions.
  + Execute containers with a non-root user.

- Remediation Method
  + The kube-controller-manager.yaml file is only present on the Master, so applying CCE diagnosis to the Master will apply security checks to the entire Cluster.
  + Modify the /etc/kubernetes/manifests/kube-controller-manager.yaml file.
  + If the following item exists in the file, change the configuration value. If it does not exist, add the configuration value.
```yaml
$ sudo vi /etc/kubernetes/manifests/kube-controller-manager.yaml

 ## Individual service account credentials for each controller
 ----------------------------------------
 --use-service-account-credentials=true
 ----------------------------------------
 
 ## Certificate management for controller account credentials
 ----------------------------------------
 --service-account-private-key-file=/etc/kubernetes/ssl/sa.key
 ----------------------------------------
```
![image](https://user-images.githubusercontent.com/67575226/106992932-f103c200-67bc-11eb-9b19-8b2ae91bacec.png)

#### <div id='3.4'/>3.4. Kubelet Authentication Control
- Item Description
  + Kubelet is an agent running on each node in Kubernetes. It executes and manages containers based on the PodSpec defined in YAML or JSON format for Pods.
  + Verify the Kubelet authentication control.

- Docker Inspection Target Items
  + Enable Docker client authentication.
  + Limit container execution from acquiring additional permissions.
  + Execute containers with a non-root user.

- Remediation Method
  + The config.yaml file is only present on the Master, so applying CCE diagnosis to the Master will apply security checks to the entire Cluster.
  + Modify the /var/lib/kubelet/config.yaml file.
  + If the following items exist in the file, change the configuration value. If they do not exist, add the configuration value.

```yaml
 $ sudo vi /var/lib/kubelet/config.yaml

 ## Kubelet Authentication Control
 ----------------------------------------
 authentication:
    anonymous:
        enabled: false
 
 readOnlyPort: 0
 ----------------------------------------
 ```
![image](https://user-images.githubusercontent.com/67575226/106992965-05e05580-67bd-11eb-8749-acb298c60a78.png)

#### <div id='3.5'/>3.5. Kubelet Authorization Control
- Item Description
  + By default, Kubelet allows all requests from the API Server of the Kubernetes Master without performing any permission checks.
  + Perform authorization validation through configuration changes.

- Docker Inspection Target Items
  + Enable Docker client authentication.
  + Limit container execution from acquiring additional permissions.
  + Execute containers with a non-root user.

- Remediation Method
  + The config.yaml file is only present on the Master, so applying CCE diagnosis to the Master will apply security checks to the entire Cluster.
  + Modify the /var/lib/kubelet/config.yaml file.
  + If the following item exists in the file, change the configuration value. If it does not exist, add the configuration value.

```yaml
 $ sudo vi /var/lib/kubelet/config.yaml

 ## Kubelet Authorization Control
 ----------------------------------------
 authorization:
     mode: Webhook
 ----------------------------------------
 ```
![image](https://user-images.githubusercontent.com/67575226/106993012-1d1f4300-67bd-11eb-8beb-a0ee6f53579f.png)

#### <div id='3.6'/### 3.6. Applying Security Profiles to Containers
- Item Description
  + AppArmor is an SELinux alternative framework created using LSM (Linux Security Module).
  + It's a Linux kernel security module that can restrict the capabilities of processes running on the host operating system.
  + Each process can have its own security profile, allowing or disallowing specific functionalities like network access, file read/write/execute permissions, etc.
  + It's a crucial security feature included by default in Ubuntu 7.10 and onwards. The docker-default AppArmor security profile is automatically applied when running containers.

- Docker Inspection Target Item
  + Setting container SELinux security options

```bash
 ## Checking AppArmor Status
 $ sudo apparmor_status

 ## Testing the docker-default Security Profile
 $ kubectl exec –it nginx-xxxxx -- /bin/bash
 $ cat proc/sysrq-trigger
 ```
![image](https://user-images.githubusercontent.com/67575226/106980702-03bdcd00-67a4-11eb-89d8-376e4a5a1bd1.png)
![image](https://user-images.githubusercontent.com/67575226/106980792-2a7c0380-67a4-11eb-8aca-bb79604a18de.png)

<br>

##  <div id='4'/>4. CVE  Diagnostic Items
### <div id='4.1'/>4.1. Disable TCP Timestamp Responses

- Item Description
  + Enabling TCP timestamp responses allows remote hosts to approximate the uptime of a system, which might assist in future attacks. Moreover, some operating systems can be fingerprinted based on the behavior of these TCP timestamps.

- Remediation Targets

| Target Environment | Category | Remediation Target |
| :--- | :--- | :---: |
| Cluster | Master | O |
| | Worker | O |
| Bosh | MariaDB | X |
| | HAProxy | X |
| | Private-image-repository | X |

- Remediation Steps
  + Modify the /etc/sysctl.conf file settings, add policies to iptables.

```bash
 $ sudo vi /etc/sysctl.conf
 ----------------------------------------
 ## Add at the bottom
 net.ipv4.tcp_timestamps=0
 ----------------------------------------
 $ sudo reboot

 # Add policies to iptables
 $ sudo iptables -A INPUT -p icmp --icmp-type timestamp-request -j DROP
 $ sudo iptables -A OUTPUT -p icmp --icmp-type timestamp-reply -j DROP
```

### <div id='4.2'/> 4.2.4.2. Mismatch between Entity Name and Subject CN Field in X.509 Certificates

- Item Description
  + Before issuing certificates, the certification authority (CA) must verify the entity's ID as specified in the CA's CPS (Certification Practice Statement). Hence, in the standard certificate validation process, it's necessary for the CN field in the presented certificate to match the actual name of the entity presenting the certificate.

- Remediation Targets

| Target Environment | Category | Remediation Target |
| :--- | :--- | :---: |
| Cluster | Master | O |
| | Worker | O |

- Remediation Steps
  + After purchasing the certificate, add the TLS secret in Kubernetes to apply the certificate to Ingress. The decision depends on how the certificate is applied to Ingress.
  + Apart from applying the certificate to Ingress, the certificate can be included in the WebServer as usual and placed in Pods behind Ingress, allowing usage. It's also possible to apply the certificate in front of Kubernetes, like L4Switch & CDN, depending on operational needs.


### <div id='4.3'/> 4.3. ntrusted TLS/SSL Server X.509 Certificates

- Item Description
  + The server's TLS/SSL certificate is signed by an untrusted certification authority. The use of self-signed certificates indicates potential TLS/SSL man-in-the-middle attacks and is not recommended.

| Target Environment | Category | Remediation Target |
| :--- | :--- | :---: |
| Cluster | Master | O |
| | Worker | O |

- Remediation Steps
  + After purchasing the certificate, add the TLS secret in Kubernetes to decide whether to apply the certificate to Ingress. The decision depends on how the certificate is applied to Ingress.
  + Apart from applying the certificate to Ingress, the certificate can be included in the WebServer as usual and placed in Pods behind Ingress, allowing usage. It's also possible to apply the certificate in front of Kubernetes, like L4Switch & CDN, depending on operational needs.


### <div id='4.4'/> 4.4. Self-Signed TLS/SSL Certificates

- Item Description
  + The server's TLS/SSL certificate is self-signed. Particularly, self-signed certificates are generally used to eavesdrop on TLS/SSL connections, making them inherently untrusted due to the potential for TLS/SSL man-in-the-middle attacks.

| Target Environment | Category | Remediation Target |
| :--- | :--- | :---: |
| Cluster | Master | O |
| | Worker | O |

- Remediation Steps
  + After purchasing the certificate, add the TLS secret in Kubernetes to decide whether to apply the certificate to Ingress. The decision depends on how the certificate is applied to Ingress.
  + Apart from applying the certificate to Ingress, the certificate can be included in the WebServer as usual and placed in Pods behind Ingress, allowing usage. It's also possible to apply the certificate in front of Kubernetes, like L4Switch & CDN, depending on operational needs.
This text reflects the description and recommended actions for handling self-signed 


