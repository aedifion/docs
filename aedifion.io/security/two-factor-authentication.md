---
description: Secure access to aedifion.io
---

# Authentication via SSO

## Authentication via Single Sign On

After your company administrator created your personal user account at the aedifion platform, please use the Login via **Single Sign On** **\(SSO\)** or rather **OAuth2.0** to access the aedifion services.

### Initial Login

At your initial attempt to access the services, please use the `Forgot Password?`-functionality below the login information:

![](../../.gitbook/assets/image%20%2825%29.png)

You will get forwarded to a page, where you have to provide your mail address. 

![](../../.gitbook/assets/image%20%2831%29.png)

![](../../.gitbook/assets/image%20%285%29.png)

After that you will receive an email to update your personal password. Please check your Spam Folder in case the mail is not in your inbox. The email is only valid for five minutes. 

### Change Password

There are two ways to change your password if you know your credentials: Either do it in the security-section of your user details in the frontend: 

![](../../.gitbook/assets/bildschirmfoto-2020-02-10-um-13.22.32.png)

or via login to the authentication server: [https://auth.aedifion.io/auth/realms/aedifion/account](https://auth.aedifion.io/auth/realms/aedifion/account). 

If you don't know your credentials please use the `Forgot Password?`-functionality, as described [above](two-factor-authentication.md#initial-login).

### Two factor authentication

To increase security many services offer or even require a second factor for authentication \(called 2FA or TFA\), e.g. a mobile phone or a specialized USB key. The reasoning behind it is, that if your computer is compromised due to malware, or that somehow an attacker has acquired your login credentials, he still cannot access the service, because he does not have your second factor.

aedifion's authentication method allows an easy setup of a second factor via a mobile phone. Recommended mobile applications to use are FreeOTP+ or Google Authenticator. This tutorial will show you how to setup two-factor authentication with FreeOTP.

#### **Login**

Login to the authentication server: [https://auth.aedifion.io/auth/realms/aedifion/account](https://auth.aedifion.io/auth/realms/aedifion/account) with your known credentials.

![](../../.gitbook/assets/image%20%2821%29.png)

#### Navigate to Authenticator

Click on the left bar on _Authenticator._ The new page will show a QR code.

![](../../.gitbook/assets/image%20%2814%29.png)

#### Scan the QR Code

Open _"Scan this QR Code"_ with your application and point your phone's camera at the screen. A new account will be shown in the account list with the name _Aedifion_. When you click on the account, it will generate a six-digit number.

![](../../.gitbook/assets/image%20%2818%29.png)

#### Enter this number

Enter this number in the form-field and click save. The code refreshes every 30 seconds. If the time ran out, just generate a new code by clicking on the account in the mobile application.

![](../../.gitbook/assets/image%20%2834%29.png)

#### Done

Congratulations, you now have enabled two-factor authentication for your account. Every time you log in, you will be requested to enter the current six-digit code from your mobile application.

![](../../.gitbook/assets/image%20%2816%29.png)

