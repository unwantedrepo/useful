# Devops Tools installation on windows



## AWS CLI:
Download latest aws cli installer from below mentioned aws site and manually install it on server. Choose default option during installation steps.
```
https://awscli.amazonaws.com/AWSCLIV2.msi
```

## Putty:
Download latest putty installer from below link and manually install it on server choosing all the default setup.
```
https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.81-installer.msi
```

## VS Code:
Download latest vscode installer from below link and manually install it on server choosing all the default setup.
```
https://code.visualstudio.com/download
```
or run below powershell script to install using powershell

```
# Set the security protocol to use TLS 1.2
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12

# Define URL for the Visual Studio Code installer
$url = "https://code.visualstudio.com/sha/download?build=stable&os=win32-x64"

# Define path to save the installer
$installerPath = "$env:TEMP\VSCodeSetup.exe"

# Download the installer
Invoke-WebRequest -Uri $url -OutFile $installerPath

# Run the installer silently
Start-Process -FilePath $installerPath -ArgumentList "/silent" -Wait

# Clean up
Remove-Item -Path $installerPath

```
## Docker:
1. open server managar on windows server and click on **Add Roles and Features**, then click **Next** on consecutive 3 pages. On features tab, select **containers** to install it on server. After this feature instation, server needs to be restarted.
2. Post restart of server, open powershell and run below commands to install docker.
   ```
   set-executionpolicy unrestricted
   Invoke-WebRequest -UseBasicParsing "https://raw.githubusercontent.com/microsoft/Windows-Containers/Main/helpful_tools/Install-DockerCE/install-docker-ce.ps1" -o install-docker-ce.ps1
   .\install-docker-ce.ps1
   docker --version

   docker pull mcr.microsoft.com/windows/servercore/iis
   ```
3. Run below mentioned additional command in powershell for docker compose
   ```
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
   Start-BitsTransfer -Source "https://github.com/docker/compose/releases/download/v2.28.1/docker-compose-windows-x86_64.exe"
   cp .\docker-compose-windows-x86_64.exe 'C:\Windows\System32\docker-compose.exe'
   docker-compose version
   ```
## JDK
Download latest java msi installer from below link and manually install it on server choosing all the default setup.
```
https://aka.ms/download-jdk/microsoft-jdk-11.0.23-windows-x64.msi
```

## Eclipse
Download latest eclipse installer from below link and manually install it on server choosing all the default setup.

2022 Release:
```
https://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/2022-09/R/eclipse-jee-2022-09-R-win32-x86_64.zip
```
2021 Release:
```
https://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/2021-09/R/eclipse-jee-2021-09-R-win32-x86_64.zip
```
