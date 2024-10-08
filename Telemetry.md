---
layout: default
---

# Generating Telemetry and Wazuh Detection

In this lab I'll be simulating a attack on a VM to generate telemetry and identify detection capabilities. 

For this lab we will be using a Windows VM with Wazuh installed and Invoke-Atomic to simulate an attack. 

Also, I'll be performing a brtue force attack with Kali Linux to generate more telemetry.


## Invoke-Atomic

To simulate attacks on the VM, I'll be using Invoke-Atomic. Atomic Red Team has a repository of detection tests based on the MITRE ATT&CK framework. Invoke-Atomic is the PowerShell module of Atomic Red Team. This tool helps to aid cybersecurity professionals in understanding, as well as simulating, relevant threats in their environment. To read more go to the following website https://github.com/redcanaryco/invoke-atomicredteam/wiki

To install Invoke-Atomic, we will need to bypass Powershell execution policies as that might prevent the installation. To do this run the following PowerShell command as Administrator. 

```
function Disable-ExecutionPolicy {($ctx = $executioncontext.gettype().getfield("_context","nonpublic,instance").getvalue( $executioncontext)).gettype().getfield("_authorizationManager","nonpublic,instance").setvalue($ctx, (new-object System.Management.Automation.AuthorizationManager "Microsoft.PowerShell"))} 
```

```
Disable-ExecutionPolicy  .runme.ps1
```

Next, run the following command to install In-Atomic and its framework

```
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);
Install-AtomicRedTeam –getAtomics
```

The following screenshot shows a successful installation.

![T1](/Images/T1.PNG)

Once Invoke-Atomic is installed we can start simulating attacks on the VM to generate telemetry and see what alerts will be triggered in Wazuh. 

I will be using attack reference T1614.001 -2 which is a command to identify system languages.

![T12](/Images/T12.PNG)

Run the following command in PowerShell to start the simulation. 

```
Invoke-AtomicTest T1614.001 -TestNumbers 2
```

![T7](/Images/T7.PNG)

Open Wazuh and navigate to the Overview page.

![T6](/Images/T6.PNG)

Here we can get an overview of security related incidents and activities for the environment. 

From here open the Threat Hunting page. In this page we can proactively search for any security alerts that may indicate a compromise. 

![T11](/Images/T11.PNG)

Scrolling down we can see the log of recent security alerts. There are a few default fields selected, we're going to add the data.win.eventdata.commandLine to the columns.

This value shows us what command is being excuted. 

![T9](/Images/T9.PNG)

Upon added the field we see the command that was executed by Atomic Red Team attack simulation. 

![T13](/Images/T13.PNG)

Clicking on the magnifying glass next to the timestamp, the Document Details window pops up giving us more information abut the alert. 

![T10](/Images/T10.PNG)

With this information, we can confirm Wazuh was able to detect the Atomic Red Team attack simulation. 

After running any attack simulation, it's best practice to run a cleanup command to remove any temporary files generated or to return settings to their previous state.

```
Invoke-AtomicTest T1614.001 -TestNumbers 2 -Cleanup
```

![T14](/Images/T14.PNG)

## Kali Linux

Before testing, we need to turn off Windows Defender and enable RDP. Having Windows Defender on will not allow us to ping the Windows VM and run an nmap scan from the Kali Vm. Once this is done spin up the Kali VM.

Open the terminal and ping the two VMs to make sure they are communicating. 

![T15](/Images/T15.PNG)

![T16](/Images/T16.PNG)

Next run the following nmap command to see any open ports and services. 

```
nmap -A
```

![T17](/Images/T17.PNG)

We see RDP is open, let's try and brute force it and see what alerts we get in Wazuh.

I'll be using crowbar to perform the brute force attack and the rockyou.txt file. To read more info on crowbar go to the following link https://www.kali.org/tools/crowbar/ .

Before runing the command, we need to convert the rockyou.txt file t UTF-8. To do this run the following command.

```
iconv -f ISO-8859-1 -t UTF-8 rockyou.txt > rockyouutf8.txt
```

![T18](/Images/T18.PNG)

Now run crowbar.

```
crowbar -b rdp -s VM IP -u VM USERNAME -C rockyouutf8.txt
```

![T19](/Images/T19.PNG)

Switching back to Wazuh, we can see Logon Failure alerts.

![T20](/Images/T20.PNG)

Opening the document details we can see the Kali VM which confirms Wazuh detected the attack.

![T21](/Images/T21.PNG)

If we open up the Multiple Windows Logon Failures log we can also see Wazuh detected this as a Brute Force attack.

![T24](/Images/T24.PNG)

## Conclusion

This lab demonstrates how we preformed simluated attacks with Atomic Red Team and a brute force attack with Kali Linux to generate telemetry and see if Wazuh was able to detect it.

It took me a while to navigate the Threat Hunting dashboard in wazuh, but once I understood how to filter for certain columns it started to make sense. 

Will need to practice more on filtering to make threat hunting easier, and eventually create custom dashboards.


[back](./)
