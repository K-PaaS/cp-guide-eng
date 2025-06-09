### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Use](/use-guide/README.md) > Configs Menu

<br>

## Table of Contents

1. [Configs Menu](#1)  
  1.1. [ConfigMaps](#1-1)  
    1.1.1. [Get the ConfigMap list](#1-1-1)  
    1.1.2. [View ConfigMap Details](#1-1-2)  
    1.1.3. [Create a ConfigMap](#1-1-3)  
    1.1.4. [Modify the ConfigMap](#1-1-4)  
    1.1.5. [Delete the ConfigMap](#1-1-5)  
  1.2. [Secrets](#1-2)  
    1.2.1. [Get the Secret list](#1-2-1)  
    1.2.2. [View Secret Details](#1-2-2)  
    1.2.3. [Create a Secret](#1-2-3)  
    1.2.3.1. [Opaque Type](#1-2-3-1)  
    1.2.3.2. [kubernetes.io/tls Type](#1-2-3-2)  
    1.2.3.3. [kubernetes.io/ssh-auth Type](#1-2-3-3)  
    1.2.3.4. [kubernetes.io/dockercfg Type](#1-2-3-4)  
    1.2.3.5. [bootstrap.kubernetes.io/token Type](#1-2-3-5)  
    1.2.3.6. [kubernetes.io/service-account-token Type](#1-2-3-6)  
    1.2.4. [Modify the Secret](#1-2-4)  
    1.2.5. [Delete the Secret](#1-2-5)  
  
<br> 

## <div id='1'/> 1. Configs Menu
### <div id='1-1'/> 1.1. ConfigMaps
#### <div id='1-1-1'/> 1.1.1. Get the ConfigMap list
- Click ConfigMaps in Configs to go to the ConfigMap list page.
  ![IMG_6_1_1]

<br>

#### <div id='1-1-2'/> 1.1.2. View the ConfigMap Details
- Click the name of a ConfigMap in the Configs list to go to the ConfigMap details page.
  ![IMG_6_1_2]

<br>

#### <div id='1-1-3'/> 1.1.3. Create the ConfigMap
- When you click the Create button in the Configs list, the Create ConfigMap popup window appears.
  ![IMG_6_1_3]

<br>

#### <div id='1-1-4'/> 1.1.4. Modify the ConfigMap
- When you click the Edit button in the Configs details, the Edit ConfigMap popup window appears.
  ![IMG_6_1_4]

<br>

#### <div id='1-1-5'/> 1.1.5. Delete the ConfigMap
- Clicking the Delete button in the Configs details completes the deletion of the ConfigMap.
  ![IMG_6_1_5]

<br>

### <div id='1-2'/> 1.2. Secrets
#### <div id='1-2-1'/> 1.2.1. Get the Secret list
- Click Secrets in Configs to go to the Secret list page.  
  ![IMG_6_2_1]

<br>

#### <div id='1-2-2'/> 1.2.2. View the Secret Details
- Click the name of a Secret in the Configs list to go to the Secret details page.  
  ![IMG_6_2_2]

<br>

#### <div id='1-2-3'/> 1.2.3. Create the Secret
##### <div id='1-2-3-1'/> 1.2.3.1. Opaque Type
- When you click the Create button in the Secret list, you go to the Create Secret page.
- The Opaque type is the default secret type when the secret type is not specified in the secret configuration file.    
  ![IMG_6_2_3]

  <table>
    <thead>
      <tr>
        <th>Item</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Storage Backend</td>
        <td>Select the storage in which encrypted Secret data will be stored.</td>
      </tr>     
      <tr>
        <td>Namespace</td>
        <td>Select the Namespace where the Secret resource is created.</td>
      </tr>
       <tr>
        <td>Name</td>
        <td>Enter the name of the secret.</td>
      </tr>
      <tr>
        <td>Specifying Data Types</td>
        <td>You can specify the type of Secret data.</td>
      </tr>
      <tr>
        <td>Key</td>
        <td>Enter the key value to store as Secret data.</td>
      </tr>
      <tr>
        <td>Value</td>
        <td>Enter the value to store as Secret data.</td>
      </tr>
      <tr>
        <td>Upload File</td>
        <td>You can upload files with Value values.</td>
      </tr>
      <tr>
        <td>Add Data</td>
        <td>You can add data to enter as a Key, Value pair.</td>
      </tr>
    </tbody>
  </table>

<br>

##### <div id='1-2-3-2'/> 1.2.3.2. kubernetes.io/tls Type
- The kubernetes.io/tls type is a secret type for storing certificates and related keys used for tls.A key value and a tls.crt value are required.  
  ![IMG_6_2_4]    

  <table>
    <thead>
      <tr>
        <th>Item</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Storage Backend</td>
        <td>Select the storage in which encrypted Secret data will be stored.</td>
      </tr>     
      <tr>
        <td>Namespace</td>
        <td>Select the Namespace where the Secret resource is created.</td>
      </tr>
       <tr>
        <td>Name</td>
        <td>Enter the name of the secret.</td>
      </tr>
      <tr>
        <td>Specifying Data Types</td>
        <td>You can specify the type of Secret data.</td>
      </tr>
      <tr>
        <td>Key</td>
        <td>Enter the key value to store as Secret data. If you type tls.key or tls.crt, the Data Type automatically changes to kubernetes.io/tls type.</td>
      </tr>
      <tr>
        <td>Value</td>
        <td>Enter the value to store as Secret data.</td>
      </tr>
      <tr>
        <td>Upload File</td>
        <td>You can upload files with Value values.</td>
      </tr>
      <tr>
        <td>Add Data</td>
        <td>You can add data to enter as a Key, Value pair. tls in Key value.If you click the Add data button with the key entered, tls.crt is automatically entered as the next data key value, and tls.crt is entered as the next data key value, if you click the Add data button with tls.crt entered as the next data key value.The key is entered automatically.</td>
      </tr>
    </tbody>
  </table>

<br>

##### <div id='1-2-3-3'/> 1.2.3.3. kubernetes.io/ssh-auth Type
- The kubernetes.io/ssh-auth type is a secret type for storing data used for ssh authentication and requires an ssh-privatekey value.  
  ![IMG_6_2_5]

  <table>
    <thead>
      <tr>
        <th>Item</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Storage Backend</td>
        <td>Select the storage in which encrypted Secret data will be stored.</td>
      </tr>     
      <tr>
        <td>Namespace</td>
        <td>Select the Namespace where the Secret resource is created.</td>
      </tr>
       <tr>
        <td>Name</td>
        <td>Enter the name of the secret.</td>
      </tr>
      <tr>
        <td>Specifying Data Types</td>
        <td>You can specify the type of Secret data.</td>
      </tr>
      <tr>
        <td>Key</td>
        <td>Enter the key value to store as Secret data. If you enter ssh-private key, the Data Type automatically changes to the kubernetes.io/ssh-auth type.</td>
      </tr>
      <tr>
        <td>Value</td>
        <td>Enter the value to store as Secret data.</td>
      </tr>
      <tr>
        <td>Upload File</td>
        <td>You can upload files with Value values.</td>
      </tr>
      <tr>
        <td>Add Data</td>
        <td>You can add data to enter as a Key, Value pair.</td>
      </tr>
    </tbody>
  </table>

<br>

##### <div id='1-2-3-4'/> 1.2.3.4. kubernetes.io/dockercfg Type
- The kubernetes.io/dockerconfigjson type is a secret type for storing docker registry access credentials for images and requires docker-server (required), docker-username (required), docker-password (required), and docker-email (optional).  
  ![IMG_6_2_6]

  <table>
    <thead>
      <tr>
        <th>Item</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Storage Backend</td>
        <td>Select the storage in which encrypted Secret data will be stored.</td>
      </tr>     
      <tr>
        <td>Namespace</td>
        <td>Select the Namespace where the Secret resource is created.</td>
      </tr>
       <tr>
        <td>Name</td>
        <td>Enter the name of the secret.</td>
      </tr>
      <tr>
        <td>Specifying Data Types</td>
        <td>You can specify the type of Secret data.</td>
      </tr>
      <tr>
        <td>Key</td>
        <td>Enter the key value to store as Secret data.</td>
      </tr>
      <tr>
        <td>Value</td>
        <td>Enter the value to store as Secret data.</td>
      </tr>
      <tr>
        <td>Upload File</td>
        <td>You can upload files with Value values.</td>
      </tr>      
    </tbody>
  </table>

<br>

##### <div id='1-2-3-5'/> 1.2.3.5. bootstrap.kubernetes.io/token Type
- The bootstrap.kubernetes.io/token type is a secret type for storing tokens used to sign a configuration map and requires values such as token-id (required), token-secret (required), usage-bootstrap-authentication (required), usage-bootstrap-signing (required), auth-extra-group (optional), expiration (optional), and description (optional).  

  ![IMG_6_2_7]

  <table>
    <thead>
      <tr>
        <th>Item</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Storage Backend</td>
        <td>Select the storage in which encrypted Secret data will be stored.</td>
      </tr>     
      <tr>
        <td>Namespace</td>
        <td>Select the Namespace where the Secret resource is created.</td>
      </tr>
       <tr>
        <td>Name</td>
        <td>Enter the name of the secret.</td>
      </tr>
      <tr>
        <td>Specifying Data Types</td>
        <td>You can specify the type of Secret data.</td>
      </tr>
      <tr>
        <td>Key</td>
        <td>Enter the key value to store as Secret data.</td>
      </tr>
      <tr>
        <td>Value</td>
        <td>Enter the value to store as Secret data.</td>
      </tr>
      <tr>
        <td>Upload File</td>
        <td>You can upload files with Value values.</td>
      </tr>      
    </tbody>
  </table>

<br>

##### <div id='1-2-3-6'/> 1.2.3.6. kubernetes.io/service-account-token Type
- The kubernetes.io/service-account-token type is a secret type for storing token credentials that check service accounts, and a service account name value is required. For the service account token type, a service account must be created in advance in the same name space.  

  ![IMG_6_2_8]

  <table>
    <thead>
      <tr>
        <th>Item</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Storage Backend</td>
        <td>Select the storage in which encrypted Secret data will be stored.</td>
      </tr>     
      <tr>
        <td>Namespace</td>
        <td>Select the Namespace where the Secret resource is created.</td>
      </tr>
       <tr>
        <td>Name</td>
        <td>Enter the name of the secret.</td>
      </tr>
      <tr>
        <td>Specifying Data Types</td>
        <td>You can specify the type of Secret data.</td>
      </tr>
      <tr>
        <td>Service Account Name</td>
        <td>Select the Service Account Name value.</td>
      </tr>      
    </tbody>
  </table>

<br>

#### <div id='1-2-4'/> 1.2.4. Modify the Secret
- In Secret Details, press the Edit button in the Data list to modify the Value value and click the Modify button to modify it.    
  ![IMG_6_2_9]

<br>

#### <div id='1-2-5'/> 1.2.5. Delete the Secret
- Click the Delete button to delete button in the Secret details.  
  ![IMG_6_2_10]

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng/blob/master/README.md) > [CP Use](/use-guide/README.md) > Configs Menu

[IMG_6_1_1]:../images/portal/IMG_6_1_1.png
[IMG_6_1_2]:../images/portal/IMG_6_1_2.png
[IMG_6_1_3]:../images/portal/IMG_6_1_3.png
[IMG_6_1_4]:../images/portal/IMG_6_1_4.png
[IMG_6_1_5]:../images/portal/IMG_6_1_5.png
[IMG_6_2_1]:../images/portal/IMG_6_2_1.png
[IMG_6_2_2]:../images/portal/IMG_6_2_2.png
[IMG_6_2_3]:../images/portal/IMG_6_2_3.png
[IMG_6_2_4]:../images/portal/IMG_6_2_4.png
[IMG_6_2_5]:../images/portal/IMG_6_2_5.png
[IMG_6_2_6]:../images/portal/IMG_6_2_6.png
[IMG_6_2_7]:../images/portal/IMG_6_2_7.png
[IMG_6_2_8]:../images/portal/IMG_6_2_8.png
[IMG_6_2_9]:../images/portal/IMG_6_2_9.png
[IMG_6_2_10]:../images/portal/IMG_6_2_10.png