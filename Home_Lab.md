---
layout: default
---

# Basic Home Lab

This is a virtualized environment utilizing VirtualBox with a Kali Linux VM and Windows 10 VM.


## Download and Install VirtualBox

Go to https://www.virtualbox.org/wiki/Downloads and download the package applicable to your machine. 

Run the installer and the installation wizard should appear. 

Click next a few times and finish the installation. 

When you open VirtualBox the following window will populate:

![VB1](VB1.PNG)

## Download and Install Kali Linux in VirtualBox

Go to https://www.kali.org/get-kali/#kali-virtual-machines and download the Kali Linux VirtualBox image.

![K2](K2.PNG)

Open your Downloads folder, right click and extract the VirtualBox image. 

In VirtualBox click on the Add button and navigate to the Downloads folder where you extracted the image and open it. 

The Kali Linux VM should be listed VirtualBox.

![K1](K1.PNG)

## Download and Install Windows 10 in VirtualBox

Go to https://www.microsoft.com/en-us/software-download/windows10 and under Create Windows 10 Installation Media click download.

Run the MediaCreationTool file and click yes to to allow the app to make changes.

Next, agree to the terms of service then select the Create Installation Media option. 

![W1](W1.PNG)

Click next a few times until you get to the Choose Which Media To Use page and select ISO file.

![W2](W2.PNG)

Open VirtualBox and click on the New button. 

Name the VM and in the ISO Image field click the drop down menu and navigate to Windows ISO image we just created.

![W3](W3.PNG)

Click next and allocate 4 GB for RAM, leave Hard disk as VDI, and enter 50GB for the virtual hard disk size.

Create and open the Windows 10 VM in VirtualBox to set up username and password and complete the installation.

![W4](W4.PNG)

During the Windows setup, the wizard will ask for a product key. Select I don't have a product key option at the bottom.

![W5](W5.PNG)

## Create Snapshot

Before testing on any VM it's best practice to create a snapshot. Snapshots allow us to revert our VM to a specific condition, so if malware is executed and breaks something you can start from the snapshot and everthing is good to go. 

To create a snapshot click on the stacked line icons next to the VM and select Snapshot.

![VB2](VB2.PNG)

Click on Take and name the snapshot.

Follow the same process for the other VM.

If malware was executed and the VM needs to be restored, select the snapshot and click on Restore.

![VB3](VB3.PNG)

## Network Configuration

There are several different types of network configurations that could be used depending on the type of testing being done.

![VB5](/Images/VB5.PNG)

### NAT

NAT is the default networking mode in VirtualBox. In NAT mode, the VM operates in a private network and is assigned a virtual IP. The VMs configured with NAT are isolated and would not be able to communicate with other VMs.

### NAT Network

NAT Network is similar to NAT, but it provides a shared network between VMs, meaning any VM with this configuration can communicate with each other. To create a NAT Network select Tools then NAT Network, and hit Create. Name NAT Network and enter a IPv4 address.

![VB4](/Images/VB4.PNG)

### Internal Network 

This type of configuration is useful for analyzing malware being that the VMs are connected to each other and completely isolated from the outside/external network. For this configuration we will need to statically assign an IP to each VM or setup a DHCP server. 

I will cover this in another lab. 

## Conclusion 

I've built a few home labs before, but this one is the most simplistic and will serve as a good base for future projects. 



[back](./)
