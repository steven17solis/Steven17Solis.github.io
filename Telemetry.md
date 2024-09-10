---
layout: default
---

# Generating Telemetry

In this lab I'll be simulating a attack on a VM to generate telemetry and identify detection capabilities. 

For this lab we will be using a Windows VM with Wazuh installed and Invoke-Atomic to simulate an attack. 

Also, I'll be using certain tools in Kali Linux to generate more telemetry.


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



Once Invoke-Atomic is installed we can start simulating attacks on the VM to generate telemetry and see what alerts will be triggered in Wazuh. 

I will be using attack reference T1003 -6 which is a credential dumping attack utilizing kmgr.dll and rund1132.exe.

Run the following command in PowerShell to start the simulation. 

```
Invoke-AtomicTest T1003 -TestNumbers 6
```

Starting the simulation, the Stored User Names and Password application will be triggered.






















[back](./)
