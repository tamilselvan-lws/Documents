<img alt="Windows-Server" src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e2/Windows_logo_and_wordmark_-_2021.svg/250px-Windows_logo_and_wordmark_-_2021.svg.png" height="70" />

<img alt="Windows-Server" src="https://upload.wikimedia.org/wikipedia/en/6/6b/Redis_Logo.svg" height="70" />


## Install WinSW Service

[Click Here](https://raw.githubusercontent.com/tamilselvan-lws/Documents/main/Redis%20Windows%20Server/WinSW.zip) to download WinSW Config File.

Unzip the <code>WinSW.zip</code> file, a folder <code>nssm-2.24</code> with nssm files will be created.

Go inside the folder & copy the nssm-2.24 folder & paste it in <code>C:\WinSW.</code>

<img alt="Windows-Server" src="/Redis Windows Server/images/1.png"/>

Go inside the pasted WinSW folder.

<img alt="Windows-Server" src="/Redis Windows Server/images/2.png"/>

Go to Command Prompt and configure WINSW-SERVICE by typing the following commands. (Faveo-Mail-Worker, Faveo-Recurring, Faveo-Reports & Faveo-Notifications are to be configured in actual).

### 1. Faveo-Mail-Worker

Go inside the folder <code>C:\WinSW\Faveo-Mail-Worker.</code>

Go to Command Prompt by typing the following Image.

<img alt="Windows-Server" src="/Redis Windows Server/images/3.png"/>

```
winsw install
winsw start
winsw status
winsw stop
winsw uninstall

```

<img alt="Windows-Server" src="/Redis Windows Server/images/4.png"/>

Now click SERVICE-NAME and start the service as shown below:

<img alt="Windows-Server" src="/Redis Windows Server/images/5.png"/>