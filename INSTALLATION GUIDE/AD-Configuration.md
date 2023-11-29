## Active Directory Setup: A Step by Step Guide

- To set up an Active Directory on a Windows Server 2022 machine you need to execute the following steps.

### 1. Launch the Server Manager Program

Launch the Server Manager program, press the Windows Logo Key and search for “Server Manager”. An application should show up on the list. Click on it to launch the program.

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-1.png" style=" width:500px ; height:250px ">

### 2. Set up Roles & Features

1. Look for “Manage” on the top right of the menu bar. Click on it and then select “Add Roles and Features.” A pop-up window will open immediately. This pop-up window is the installer wizard that guides you with the roles and features setup.

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-2.png" style=" width:500px ; height:250px ">

2. On the left side of the window, you’ll see a list of all the checkpoints you encounter in this stage. Click “Next”.

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-3.png" style=" width:500px ; height:250px ">

### 3. Select Installation Type

3. At the “Installation Type” checkpoint select “Role-based or feature based installation” radio button and then click “Next”.

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-4.png" style=" width:500px ; height:250px ">

### 4. Configure your Server Selection and Roles

4. On the “Server Selection” checkpoint, select “Select a server from the server pool” radio button. This lists a server installed on your machine. Please, click on the desired server once to select it and click “Next.”

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-5.png" style=" width:500px ; height:250px ">

5. At the “Select Server Roles” checkpoint, select the role for the server. In the centre of the window, there is a list of all the roles that you can assign to your server machine. Search for “Active Directory Domain Services”. 

To quickly find the required role, simply click on the first role in the list once and quickly type the keyword “active”. This will bring the selection to the required role. Once you’ve found “Active Directory Server Roles”, click the square box to confirm the selection and click “Next.”

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-6.png" style=" width:500px ; height:250px ">

### 5. Add features

6. Next, a pop-up window will be displayed. This is the checkpoint for adding new features. Navigate on the “Add features” button at the bottom of the window and a list of available features will be displayed.

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-7.png" style=" width:500px ; height:250px ">

Next simply click “next” without making modifications to any other settings.

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-8.png" style=" width:500px ; height:250px ">

7. You will be redirected to the adding “Active Directory Domain Services” feature once the previous step is complete. On the installer wizard window, click “Next”.

### 6. Summary and confirmation

8. You’ll see a summary of your selected options here. Have a look at them carefully, and if you think you’ve made a mistake at any of the earlier checkpoints, you can go back and fix it by clicking “previous.” 

Then, click “Install” button, once you’re satisfied with your selections at the “Confirmation” checkpoint.

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-9.png" style=" width:500px ; height:250px ">

9. The wizard will then begin installation. The time of install depends on your machine’s hardware configuration and what features you’ve selected to be installed. Please make sure not to interrupt the installation. Once the installation is complete, click the “Close” button.

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-10.png" style=" width:500px ; height:250px ">

NOTE: Please don’t close the Server Manager application as you will be using the application for the next steps.


### 7. Promoting Your Server to a Domain Controller

So far, you’ve just added the required feature “Active Directory Domain Services.” The feature “Active Directory Domain Services” you’ve just added needs to be promoted to a DC (Domain Controller). Here are the following steps needed to do so:

- 1. Relaunch “Server Manager” if you have already closed it. On your Server Manager dashboard, you’ll should see a yellow triangle warning sign on the top right of the window near the menu bar. This sign appears only if Active Directory Domain Services was properly installed.

- 2. Click on the warning sign and a dropdown list will show you the required actions termed “post-deployment configuration.”

- 3. Look for the “Promote this server to a domain controller” option and click on it.

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-11.png" style=" width:500px ; height:250px ">

### 8. Add A Forest

At this checkpoint, a configuration wizard will open on your screen, which will guide you throughout your deployment configuration. The first step of the deployment configuration is to add a new forest.

### 9. Deployment Configuration

At the first checkpoint “Deployment Configuration”, please select the “add a new forest” radio button and enter your root domain name as desired. Then click “next”. (When adding a new forest, you’ll see multiple options. You don’t necessarily have to add a new forest and you choose any option from the given list.)

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-12.png" style=" width:500px ; height:250px ">

### 10. Setting Domain Controller options

2. At the “Domain Controller Options” checkpoint, leave all the settings untouched and enter your password and confirm it. Make sure to keep a note of this password as changing it later on is troublesome.

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-13.png" style=" width:500px ; height:250px ">

### 11. Configuring DNS Options

3. On the DNS Options page, you’l see an error message stating that there’s no parent zone found and no delegation for your DNS server could be created. Ignore this message and click the “next” button, leaving all the settings at this checkpoint unchanged.

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-14.png" style=" width:500px ; height:250px ">

### 12. Configure Additional Options

4. On the Additional Options page, enter your desired NetBIOS domain name in the given textbox. Click “Next”.

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-15.png" style=" width:500px ; height:250px ">

### 13. Confirm Preselected Paths

5. Three or more paths will be listed on your screen. Do not change these paths. You’re not required to keep a note of these paths either. Click “next”.

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-16.png" style=" width:500px ; height:250px ">

### 14. Review your Selections

6. Whatever options you’ve selected so far will listed on the configuration wizard at this checkpoint. Have a look at them and if needed, move to the previous checkpoints using the “previous” button and make the desired changes. Once you’re satisfied with the selected options, click “next” on the “Review Options”.

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-17.png" style=" width:500px ; height:250px ">

### 15. Run Prerequisites Check

7. Next, head to the “Prerequisites Check” checkpoint. At this stage you’ll see, if all the prerequisite checks were successfully completed. If not, then a list of errors will be displayed on the window. If there are any errors, you’ll need to go to the stated checkpoint and fix the errors. Once you’ve fixed all the errors, a green check mark with a success message will be displayed. Then click “Install” to begin the installation.

<img src="https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/INSTALLATION%20GUIDE/Images/ad-configuration/AD-18.png" style=" width:500px ; height:250px ">

Congratulations! You have successfully set up Active Directory on your Windows Server 2022. Next, your server machine will need to be restarted once the promotion is successfully complete.



## Important PowerShell Commands After Active Directory Setup

Verifing if the Active Directory installation was successfully completed:

```
Get-Service adws,kdc,netlogon,dns
```

Getting the details of your Domain Controller:

```
Get-ADDomainController
```

To get the details of your domain, use this command:

```
Get-ADDomain exampledomain.com
```