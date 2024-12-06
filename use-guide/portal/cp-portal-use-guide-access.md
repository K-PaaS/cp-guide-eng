### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Use](../Readme.md) >  [Portal Use Guide](./cp-portal-use-guide.md) > Access

<br>

## Table of Contents
1. [Access the Container Platform Portal](#1)  

<br>

## <div id='1'>1. Access the Container Platform Portal
Access the Container Platform portal.<br><br>
**Container Platform Portal URL** : `https://portal.${HOST_DOMAIN}`

<br>

### <div id='1-1'/>1.1. Log in to your Container Platform Portal administrator account
Administrator accounts require a password reset setting, so preprocess them as described below.
<details>
<summary><h4> :key: Set Container Platform Portal administrator account password </h4></summary>

<h1></h1>  

### 1. Get Keycloak Admin account information
To check the Keycloak Admin account information, execute the command below.
```bash
# Get Keycloak Admin account
$ kubectl get cm cp-portal-configmap -n cp-portal -o yaml | grep KEYCLOAK_ADMIN
KEYCLOAK_ADMIN_USERNAME: ********* (Username)
KEYCLOAK_ADMIN_PASSWORD: ********* (Password)
```

<br>

### 2. Access and login to the Keycloak Admin Console
Access the Keycloak Admin Console and log in to the Keycloak Admin account you found.<br><br>

**Keycloak Admin Console URL** : `https://keycloak.${HOST_DOMAIN}`

![image 011]

<br>

### 1. Reset the container platform administrator account password
- Change the Realm information in the top left corner to `Cp-realm`.

![image 012]

- Select the menu [Users] and click on the account with the username `admin`.

![image 013]

- Select the menu [Credentials] and click the button [Reset password] to reset the password.
  + **Password** : `Enter a password value to initialize`
  + **Temporary** : Select `Off
    - If 'On' is selected, the user will need to change the password again at the next login.

![image 014]
![image 015]

<h1></h1>

</details>

- Log in to the Container Platform portal with an administrator account.
  + **Username** : `admin`
  + **Password** : `initialized password value`

![image 002]

<br>

### <div id='1-2'/>1.2. Container Platform Portal User Account Login
#### Register as a user at
- Click the 'Register' button at the bottom.

![image 003]

- Enter the user account information to register, and click 'Register' button to sign up for the Container Platform Portal.

![image 004]

- You cannot access the portal immediately after registering, but you can use the portal after the administrator assigns a Namespace and Role to the user.
  To assign Namespace and Role to a user, see [[Modify User]](./cp-portal-use-guide-managements.md#-115-modify-user)


#### User login
- Log in to the Container Platform portal with the account registered through registration.

![image 006]

<br>

### [Index](https://github.com/K-PaaS/cp-guide-eng) > [CP Use](../Readme.md) >  [Portal Use Guide](./cp-portal-use-guide.md) > Access


[image 002]:..//images/portal/cp-002.png
[image 003]:..//images/portal/cp-003.png
[image 004]:..//images/portal/cp-004.png
[image 005]:..//images/portal/cp-005.png
[image 006]:..//images/portal/cp-006.png
[image 011]:..//images/portal/cp-011.png
[image 012]:..//images/portal/cp-012.png
[image 013]:..//images/portal/cp-013.png
[image 014]:..//images/portal/cp-014.png
[image 015]:..//images/portal/cp-015.png
