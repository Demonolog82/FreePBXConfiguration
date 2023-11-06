# FreePBXConfiguration
Simple and Fast FreePBX Configuration

This readme is to fast and simple FreePBX configuration without any additional settings
This setup will show how to configure FreePBX, softphone and physical phone

1. Download FreePBX from https://www.freepbx.org/
Select Download > Download FreePBX 16 (or newer if there is newer release)
Fill required fields and click Download FreePBX 16 - ISO image will start to download

2. Install FreePBX distibution to your PC or Virtual Environment (in my case it's Proxmox)
During installation set root password
After installation go through configuration wizard - in most cases it will be next apart from user creation where you need to provide new user and password for it

3. After installation log on through console using root account (please do not use this account normally, you should secure distro to not allow to log on through this user
You will see IP Address to which you can connect using browser

4. Open web browser, go to configuration panel - use ip from previous step

5. Select FreePBX Administration - use user created during setup (not root)

6. Go to Connectivity > Firewall and select tab Networks
Add networks you want to be allowed to connect to your FreePBX server (there should be network on which FreePBX is located), if your clients will use the same network then you do not need to do anything. Remember to select Assigned Zone to Trusted (Excluded from Firewall)
Click Save

7. Select Applications > Extensions
8. Select Add Extension > Add New SIP (chan_pjsip) Extension
9. Provide minimum required data:
- User Extension - this is phone number of user - in example is 900, if you do not change settings in User Manage Settings this will be also user used to log in
- Display Name - this will be displayed when calling
- Secret - Secret to be used for this number - in example it is 123456789
and on User Manage Settings set Password for New User - in example is 123456789

Click Submit
10. Create more than one extensions to be able to test it properly
11. Select Apply config (red button in top right corner) and wait for reloading service

You will use these Information to logon to FreePBX from yout Phone.

Now let configure softphone
For Windows MicroSIP will be used - download it from: https://www.microsip.org/, you can download portable version

Run MicroSIP:
1. From right menu (arrow down) select Add Account
2. Provide information from created account in FreePBX
AccountName - Name of Account - it can be anything
SIP Server - IP of you SIP server, the same you got after FreePBX installation
UserName - This is your User created during creation of Extension, if User was created automatically then it's extension number, if you changed it to different value then please use what you entered during extension creation
Domain - Same as SIP Server
Password - password created in User Manage Settings
Click Save
3. Application should login and you should see status Online in lower left corner

Configuration of Yealink T20P
URL: https://support.yealink.com/en/portal/search?keyword=T20P
Firmware URL (same is located in this repo): https://support.yealink.com/en/portal/docList?archiveType=software&productCode=a2d13025edcf388a
In this case i used phone with latest firmware available which is: 9.73.193.50
Please reset phone to factory defaults - it will be easier to configure
Default user/password is: admin/admin

1. Logon to phone using browser - IP from Phone you can check on phone:
- Use Menu button > Select Status (menu item 2) > you will IP Address
2. Go to tab Account and set your settings with previously created account
- Line Active - Enabled
- Label - it can be anything, like your name or number
- Register Name - enter your phone number created in extensions in FreePBX
- User Name - enter your user name created in FreePBX for extension which you are configuring
- Password - enter password for this user
SIP Server 1:
- Server Host: ip of your FreePBX host
Port should be 5060 and Transport UDP

Click Confirm
If everything has been set correctly then when you go to Status tab you should see Registered Status for this line.

Now you can try to call from one number to another and check if everything is working.


Troubleshooting:
IF there is any issue with Registering phone number please check what's inside logs and if you see connection tries in logs.

1. Go to Admin > Asterisk CLI
2. Type: pjsip set logger on, you should see message PJSIP logging enabled
3. Go to Reports > Asterisk Logfiles
4. You should see logs - please check if phone which cannot register is visible in this log, if not you will need to check network settings on device and check if network is added in Firewall from previous steps
5. After all go to Admin > Asterisk CLI
6. Type: pjsip set logger off to disable additional logging
