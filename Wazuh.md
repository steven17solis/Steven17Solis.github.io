---
layout: default
---

# Wazuh

Wazuh is a free open source XDR and SIEM for endpoints and cloud deployments. To read more go to https://wazuh.com/

In this lab I'll be running through how to install and confiure Wazuh Agent on Windows VM. 

## Install Wazuh Server

There are a few differnt ways to install Wazuh. The quickest and easiest is to download the .OVA file for virtualized environments and import it. To do this download the file from https://documentation.wazuh.com/current/deployment-options/virtual-machine/virtual-machine.html

Once it's downloaded, right click on the file and select Open With and hit VirtualBox Manager.

Click Finish in the Import Virtual Appliance window to start the importation. 

![WA1](/Images/WA1.PNG)

## Configure Wazuh Server

Wazuh recommends changing the graphics controller to VMSVGA to prevent the VM from freezing. To do this open the Settings and navigate to Display. In Graphics Controller drop menu select VMSVGA. 
![WA2](/Images/WA2.PNG)

By default, the Wazuh server's network is set to Bridged which allows the VM to act as a unique device on the host machine. This might cause some security risk and it is recommened to switch it to NAT Network. This isolates the VM in a virtual network with a unique IP separate from the host network but allows it to communicate with other VMs. 

![WA4](/Images/WA4.PNG)

## Run Wazuh Server

Start the Wazuh server and log in with the credentials presented.

![WA4](/Images/WA5.PNG)

Leave this VM running.

![WA6](/Images/WA6.PNG)

## Install Wazuh Agent

Start the Windows VM and go to the following link https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-windows.html and click on Windows Installer.

![WA7](/Images/WA7.PNG)

Open the file to start the installation wizard and follow prompts. 

Check the Run Agent configuration interface box and hit finish.

![WA9](/Images/WA9.PNG)

Go back to the Wazuh Server and type the command ifconfig to get the server's IP. 

![WA11](/Images/WA11.PNG)

Copy the Wazuh Server IP and paste it in Manager IP in Wazuh Agent window in the Windows VM and leave the Authentication key blank and hit save.

Open a browser and type the IP address of the Wazuh Server to access the dashboard. 

Click Advance and Procces to the IP address. In the login page use the default credentials admin and admin to access the page.

![WA119](/Images/WA119.PNG)

Click the stacked line icon in the top left corner and select Endpoint Summary under Server Management. 

![WA16](/Images/WA16.PNG)

Click Deploy New Agent.

## Deploy Agent

Select Windows and in the Server Address field type the IP of the Wazuh Server.

![WA12](/Images/WA12.PNG)

Name the Agent and run the commands listed in Powershell. 

![WA13](/Images/WA13.PNG)

![WA15](/Iamges/WA15.PNG)

## Confirm Connection 

Go back to the Wazuh Dashboard to confirm the agent is active. 

![WA17](/Images/WA17.PNG)

Open the Wazuh Agent Manager and the authentication key should populate.

![WA18](/Images/WA18.PNG)

## Conclusion 

Now we can detect and respond to security events. In a later lab I will go over how to use Wazuh and perform an attack to generate telemetry. 


[back](./)
