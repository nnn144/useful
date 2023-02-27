# Getting VPN
## 0. Prerequisites
FortiClient VPN requires DUO two-factor authentication. Please refer to [this KB article](https://help.fit.edu/TDClient/39/Portal/KB/ArticleDet?ID=797) to setup DUO.

## 1. Introduction
FIT asks for using VPN to remotely use some of the resources.

## 2. Get VPN software
For Windows - Click [HERE](https://tsc.fit.edu/forticlient/FortiClientVPNSetup_6.2.6.0951_x64.exe.zip)

For Mac - Click [HERE](https://tsc.fit.edu/forticlient/FortiClientVPNSetup6.2.6.737macosx.dmg.zip)

## 3. How to configure FortiClient VPN
Once the application is installed, you will need to select to add a new connection. Follow the settings in the image below.
![VPN Setting 1](https://help.fit.edu/TDPortal/Images/Viewer?fileName=b2e52746-2dcd-46e1-822f-10a24e743555.png&beidInt=5)

After the connection is configured type in your TRACKS username and password and click the CONNECT button.
![VPN Setting 2](https://help.fit.edu/TDPortal/Images/Viewer?fileName=4ce94657-3caf-46bc-8894-e48618a4d2ac.png&beidInt=5)<br>
**You will receive a DUO approval request on your DUO mobile app.**  
The VPN connection status will remain at 45% until you select Approve in the mobile app.

NOTE: If the connection status stops at 40%, please look at your task bar in the bottom and check if there is a second instance of FortiClient asking you to accept a certificate, if it is, please accept the certificate.<br>
![VPN Setting 3](https://help.fit.edu/TDPortal/Images/Viewer?fileName=9a40497b-8093-4465-8b34-19bb643c39c7.png&beidInt=5)

Once the connection has been established your application should look like the image below:
![VPN Setting 4](https://help.fit.edu/TDPortal/Images/Viewer?fileName=02dfdb61-496c-4a2d-b252-9c77d6c58c8c.png&beidInt=5)

## 4. Update FortiClient connection settings
*NOTE: This step is used to change the VPN connection settings, because FIT changed the server once.*<br>
*NOTE 2: Normally you don't need this step, unless you want to use it for something else (not FIT).*

Click on the **three lines** to the right of your selected "VPN Name", and click **Edit The Selected Connection**.
![VPN Setting optional](https://help.fit.edu/TDPortal/Images/Viewer?fileName=6aa9bce3-e919-41fc-9fbc-24d803a7f363.png&beidInt=5)

Remove roar.fit.edu from the Remote Gateway field and enter **FITVPN.fit.edu**. Click **Save**.
![VPN Setting optional 2](https://help.fit.edu/TDPortal/Images/Viewer?fileName=ba5f0ffa-0cff-435d-9ca7-ad0c8405a2a1.png&beidInt=5)

