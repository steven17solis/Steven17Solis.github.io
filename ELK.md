---
layout: default
---

# ELK

ELK is the combination of the following technologies:

E - Elasticsearch

L - Logstash

K - Kibana

These technologies provide a dynamic solution when it comes to working with large data sets. They can also be used to setup SIEM rules which would enable defenders to conduct threat hunting and detect attacks.

## Setup An Account

Go to the following website https://cloud.elastic.co/registration and create an account.

Once account is verified, create a deployment.

![ELK2](/Images/ELK2.PNG)

## Installation

In the menu, select I'd like to do something else. 

In the Welcome Home page, sreach Kibana and hit enter.

![ELK3](/Images/ELK3)

In the next page click Add Kibana.

At the bottom of the screen, click the Install Elastic Agent.

![ELK4](/Images/ELK4.PNG)

Copy the command and paste it in Powershell and hit enter.

![ELK5](/Images/ELK5.PNG)

Type y and hit enter.

Now switch back to Elastic and confirm 1 agent has been installed and click Add The Integration.

![ELK7](/Images/ELK7.PNG)

Leave the default fields checked and click Confirm Incoming Data.

![ELK8](/Images/ELK8.PNG)

Once that's finished click View Assets. 

![ELK9](/Images/ELK9.PNG)

Now confirm the device has successfully connected by clicking the stacked line icon in the top left corner, scrolling to Management and selecting Fleet.

![ELK10](/Images/ELK10.PNG)

## Download Sysmon

To get logs that are more readable and useful, we can use Sysmon.

Download Sysmon from https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon

![ELK11](/Images/ELK11.PNG)

Extract all the files from the Sysmon Folder. 

![ELK12](/Images/ELK12.PNG)

Once all the files are extracted, open up Powershell and Run as Administrator.

In Powershell cd into the directory where the Sysmon files where extracted and run the following command.

```
.\System.exe -i -n -accepteula
```

## Integration

Go back to Elastic and navigate to Integrations under Management. 

In the Integration page search for Windows and select it.

![ELK15](/Images/ELK15.PNG)

Click the Add Windows button and by default Sysmon Opertional should be active in the Collect events from the following Windows event log channels. 

![ELK17](/Images/ELK17.PNG)

Hit Save and Continue then click Add Elastic Agent to your Host.

![ELK18](/Images/ELK18.PNG)

## Filter

Navigate to Discovery by click on the stacked line icon and add the following filter to limit results to sysmon data, this will make it easier to search in the logs. 

![ELK20](/Images/ELK20.PNG)


## Conclusion

With the Elastic agent installed we can generate logs by moving files, creating files, start programs, etc...

In a later lab, I will go over how to leverage ELK for threat detection and hunting. 


[back](./)
