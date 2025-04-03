<h1>Comparing AWS and Azure for Threat Detection in Cloud Environments</h1>

<h2>Description</h2>
This project involved setting up and managing a vulnerable honeypot VM in both Azure and AWS to analyze real-world cyberattacks. Security monitoring tools such as GuardDuty and Azure Security Center were used to detect and analyze intrusion attempts. Attack data was visualized through AWS QuickSight and Azure Sentinel.
<br />

<h2>Technologies and Environments</h2>

- <b>Cloud Platforms:</b> Microsoft Azure, AWS Cloud
- <b>Security Tools:</b> AWS GuardDuty, Quicksight, Azure Sentinel, and Security Center
- <b>Languages:</b> Powershell and SQL

<h2>Project Documentation:</h2>

<p align="center">
Figure 1: Microsoft Azure Security Monitoring Workflow. <br/>
<img src="https://i.imgur.com/dAEkXHy.png" height="80%" width="80%" alt="Project Steps"/>
<br />
<br />
<br />
Figure 2: AWS Security Monitoring Workflow. <br/>
<img src="https://i.imgur.com/rdiRljz.png" height="80%" width="80%" alt="Project Steps"/>
<br />
<br />
Figure 3: Security Hub findings. <br/>
<img src="https://i.imgur.com/MVWWIiL.png" height="80%" width="80%" alt="Project Steps"/>
<br />
In figure 3 Security Hub shows some findings about the ec2 honeypot instance I created about letting traffic come in and how it’s a risk. This would be something that’s beneficial to know if it was a network I wanted to be secure but in this case, we don’t mind be vulnerable to attacks.
<br /> 
<br />
Figure 4: Query created in Azure to visualize the RDP attacks <br/>
<img src="https://i.imgur.com/OBP0dUY.png" height="80%" width="80%" alt="Project Steps"/>
<br />
This Azure log analytics query is designed to extract, filter, and summarize data from raw log entries stored in the raw data field. It uses regular expressions to extract specific values such as username, timestamp, latitude, longitude, sourcehost, state, label, destination, and country from the raw log entries. The query then filters out records where the destination is samplehost and excludes rows with an empty sourcehost, ensuring only relevant and valid data is processed. Afterward the data grouped based on the various fields lead to a count of the events generated for each unique combination of them. This summarization helps to identify patterns and trends in the data like the frequency of the events related to specific users and their geographical locations to be able to better visualize them. 
<br /> 
<br /> 
Figure 5: Query created in CloudWatch Logs Insights. <br/>
<img src="https://i.imgur.com/c2nPP8q.png" height="80%" width="80%" alt="Project Steps"/>
<br />
In Figure 5 showing AWS CloudWatch Logs insights, the query is designed to do the exact same thing as the Azure query. Since Azure uses Kusto Query Language (KQL) we just adapted it for AWS as Azure uses a more SQL like syntax that’s unique to AWS. The only difference with this is that this was the final step needed to visualize for Azure but for AWS that wasn’t the case. We had to feed this information to the S3 bucket list which then we could use either Amazon Athena or Amazon QuickSight which we chose the latter.
<br /> 
<br />
Figure 6: GuardDuty Findings. <br/>
<img src="https://i.imgur.com/sRpzJ6G.png" height="80%" width="80%" alt="Project Steps"/>
<br />
After successfully setting everything up in AWS GuardDuty I received this notification notifying me that honeypot I created has an open port. It also states that a malicious host is actively trying to interact with the port to potentially find any vulnerabilities. It’s only seen as a low severity level but could become higher if they somehow were to gain access to the services or data. Which tells me that we succeeded in making the honeypot and that people are already trying to interact with it.
<br />
<br />
Figure 7: Example of both honeypot machines being RDP attack. <br/>
<img src="https://i.imgur.com/6htza6T.png" height="80%" width="80%" alt="Project Steps"/>
<br />
In figure 7 it shows the repeated access attempts targeting the honeypot VMs demonstrating brute force attacks from malicious actors from different parts of the world. In the background there’s a script that’s running with parameters set to automate the monitoring of failed RDP attempts. The attempts are being logged with geolocation details giving the longitude, latitude, state/province, country name and more. This shows how the attackers mainly were going after the administrative names as well as guessing the password.
<br />
<br />
This process on the amazon honeypot took a while as even though I made it vulnerable it wasn’t getting attacked for a few days after running it for hours just waiting for traffic to come in. With Azure it was getting attacks after about 15 minutes. I’m still puzzled by why this was the case, but I believe it was the public Ip address AWS automatically gave the VM. After switching it to another one and a couple of hours waiting it was finally getting attacked.
<br />
<br /> 
Figure 8: CloudWatch Logs. <br/>
<img src="https://i.imgur.com/UVIWasU.png" height="80%" width="80%" alt="Project Steps"/>
<br /> 
This is the same information that was being displayed in figure 7 just being outputted back into the respective log analyzer for both AWS and Azure. This image is specifically from CloudWatch where data can be structured, tagged, and indexed for efficient querying. We use the Insights mode to create queries that highlight the different attempts to understand patterns that are used
<br /> 
<br /> 
Figure 9: CloudWatch Logs. <br/>
<img src="https://i.imgur.com/k0LVTrC.png" height="80%" width="80%" alt="Project Steps"/>
<br />
After everything was completed in Azure, which was fairly straightforward with its use of resource groups by collecting all of the logs and using a query to plot everything on the map, this was some of the results we received. The scan was run for a couple of days which we could only get about 1,000 max of attacks each day with using Ip geolocation free version. We had limited time on the Entire environment from using the “free” subscription of Azure. When watching the attacks come through China was the first one to see our machine and stopped completely after that. Saudi Arabia joined later for about a day in a half followed by Mexico, and they just went back and forth with the attacks. From the large number of attacks we see that the attackers had to be using some type of automated techniques to continuously attack the machine. The most logical explanation would be the use of botnets or an automated script to try and exploit vulnerable systems. 
<br /> 
<br /> 
Figure 10: CloudWatch Logs. <br/>
<img src="https://i.imgur.com/wa7Lc6O.png" height="80%" width="80%" alt="Project Steps"/>
<br />
When completing the setup for AWS unlike Azure we had more steps to getting visualization for the attacks with also different routes to take. We could have sent the queried logs to S3 buckets then used Amazon Athena to complete the rest of the process, which was the initial plan. However, there were policy issues within the environment which made the 2nd option the better route to go. So, after getting the logs we sent them from CloudWatch to the S3 bucket just to store them for future purposes. Storing it in the S3 bucket made storing it within QuickSight easier. On QuickSight it’s possible to just export the data from the bucket which we can then use visualize mode. To specifically plot the attacks, we used the longitude and latitude coordinates for graphing it. The color is based off the Location label which had to be created to ensure parts in the U.S. wasn’t just listed as United States but had that specific state as well. Lastly, we have counts for each attack that stems from the number of times that country launched and attack.  The counts can be seen if you just hover over that specific bubble which it gives you the country followed by the number of times it occurred. 
<br /> 
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
