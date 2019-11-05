---
description: Secure access to aedifion.io with a second factor
---

# Two factor authentication

To increase security many services offer or even require a second factor for authentication \(called 2FA or TFA\), e.g. a mobile phone or a specialized USB key. The reasoning behind it is, that if your computer is compromised due to malware, or that somehow an attacker has acquired your login credentials, he still cannot access the service, because he does not have your second factor.

The aedifion.io services allow access via Keycloak, which allows an easy setup of a second factor via a mobile phone. Recommended mobile applications to use are FreeOTP+ or Google Authenticator. This tutorial will show you how to setup two-factor authentication with FreeOTP.

#### **Login**

Login to the authentication server: [https://auth.aedifion.io/auth/realms/aedifion/account](https://auth.aedifion.io/auth/realms/aedifion/account) with your known credentials.

![](../../.gitbook/assets/image%20%2818%29.png)

#### Navigate to Authenticator

Click on the left bar on _Authenticator._ The new page will show a QR code.

![](../../.gitbook/assets/image%20%2812%29.png)

#### Scan the QR Code

Open _"Scan this QR Code"_ with your application and point your phone's camera at the screen. A new account will be shown in the account list with the name _Aedifion_. When you click on the account, it will generate a six-digit number.

![](../../.gitbook/assets/image%20%2815%29.png)

#### Enter this number

Enter this number in the form-field and click save. The code refreshes every 30 seconds. If the time ran out, just generate a new code by clicking on the account in the mobile application.

![](../../.gitbook/assets/image%20%2827%29.png)

#### Done

Congratulations, you now have enabled two-factor authentication for your account. Every time you log in, you will be requested to enter the current six-digit code from your mobile application.

![](../../.gitbook/assets/image%20%2813%29.png)

