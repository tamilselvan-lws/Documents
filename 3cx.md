## Install Debian for 3CX

To install Debian for 3CX:

1. [Download the latest 3CX ISO.](https://downloads-global.3cx.com/downloads/debian10iso/debian-amd64-netinst-3cx.iso)

2. If you are using a hypervisor/virtualized OS, set the CD option to boot from the ISO and ensure the CD drive is set to connect on startup. If you are installing on a mini PC, then create a bootable image, plug it into one of the available Mini PC’s USB ports and set the BIOS to boot from the USB drive to start the installation.

<img src="https://lh7-us.googleusercontent.com/KC4jd8eMiHS_n_ja703OI2oLezB7uEiPZCgRkr-SXZGZWWqggnkBr8IXz_2NEDtxWFcafaIpqrbh_MjAaDx-scp5m8K56XGmlaFS7-f9LMj3nUoXfPke3cJrwVTVSqJV5VZN2fvaj-M8CBoxFE24ySrcqc8fasKipBPMuR_IZYP-mzIT89Sn6A" style=" width:500px ; height:250px ">

3. Boot your system with the downloaded 3CX ISO, select “**Install**” from the main boot screen and press the "**Enter**" key.

4. **Important:** You need to configure a static IP address for the 3CX host machine. Select "**Go Back**" or press the ‘**ESC**’ key when asked for the hostname to first set the IP address of the system and then confi gure the hostname. The system’s IP address affects the running PBX services, connected clients and IP phones. To change the IP address on a running 3CX system, you need to reboot the machine and then re-provision the connected apps and IP phones. If you don’t want to set a static IP address, proceed to No. 6.

5. Select “Configure Network Manually” and press the <Enter> key to set the system's static IP address. Select <Continue> to enter the network netmask and select <Continue>. Then, enter the network default gateway IP address and select <Continue>. Finally, enter the name server(s) IP address(es) and select <Continue>.

6. Enter a hostname for the computer so you can easily identify it on your network, using the characters 'a' to 'z', numbers '0' to '9' and the '-' character.

7. Enter a domain name - use the same domain you used on other computers in the network, e.g. mycompany.com. Select <Continue> to proceed.

8. Select the default system language and press the <Enter> key.

9. Select your geographical location from the location menu and press the <Enter> key to proceed.

10. Specify the root account password for the machine and select <Continue> to re-enter the password for verification purposes. Finally, select <Continue> to proceed. Note: Set a strong password for the root user, as this account has no restrictions!

11. Select the system timezone and press the <Enter> key to proceed.

<img src="https://lh7-us.googleusercontent.com/Hh8qTLawWzFdN6jJ7tec_vEjdcDVpRUOmu0wUMWE6nrTlHZKaBVdcRt8723wbjUpCYidI_E975zovyh80b9pspYckANAiH2VBOrMdl5nkrI2W0NQ0FQDy13d0O15NT7ndMK1q7zAdpbPbegViaKu4a311WDkIbc-t0yGe40vls2UMpGWcwo_ow" style=" width:500px ; height:250px ">

12. Partition your disk, selecting “Guided - use the entire disk”. Press the <Enter> key to proceed.

13. Confirm your disk selection by pressing the <Enter> key. If you are installing on a bare metal machine, all data on your disk will be erased!

14. Select the “All files in one partition” partitioning scheme and press the <Enter> key to proceed.

15. Select “Finish partitioning and write changes to disk” and press the <Enter> key to proceed to the 3CX Debian installation.

16. Select ‘Yes’ and press the <Enter> key to confirm writing changes to the disk. The install process can take about 5-20 minutes, depending on your machine’s performance. When the Debian installer finishes, the machine is rebooted and the 3CX installation starts automatically.

17. Now choose the “3CX Version xx” to install and then press <OK>. Agree to the “3CX License Agreement” to proceed.

## Step 5: Upload the Configuration File

<img src="https://lh7-us.googleusercontent.com/cRr3MmkELEEFbfG-mUv0dzzetU3xF5khttEU1oJnEMnJlaJddH8X5tui7XOB4Zk1Z65GuR9h0jowLOCuLY1PIgxhQGpGn54_vYQHQzl6DEvUUAaxrxuEL-1cXy58y1VO30MbRvx80fkKABDLqhrAns8tsKlnufe3pAZW4Ltxf6rTiBmnn95IrMQ" style=" width:500px ; height:250px ">

After the 3CX Debian installation finishes and the machine is rebooted, you need to upload the configuration file to complete the installation.

1. If you do not have a configuration file yet, go to www.3cx.com/install/

2. Login with your 3CX account and configure an On Premise / DIY PBX. At the end of the process, you will be given a link to the configuration file. You can copy the link or download the file. For more information see Installing 3CX

3. Use a web browser on http://<ip of machine>:5015 by selecting option 1

4. Once your installation is ready, you will be prompted to set your password. Login to the PBX using the email you used to register.

<img src="https://lh7-us.googleusercontent.com/qre5s_4RcIpB7EP3hUu_P-Hq-h5U3xVq_QP6FHD-iowKPATK6VhVVre0nUKY6ZN4zyGwcrBfcKI6hptOmeJ4AkrDgQnHnlDaIsOdUO_vnXbBmxiwQ8GUtsgCAiBzoU0MyVEKpGDc3mvuWqnGTcc_worBoqfnGp5h5pduUo8yQ2j71M0OEoaqv1Y" style=" width:500px ; height:250px ">

