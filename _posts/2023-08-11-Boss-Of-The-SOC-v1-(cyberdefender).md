---
layout: post
title: CyberDefenders BlueTeam CTF Challenges | Boss Of The SOC v1
date: 2023-09-11 07:27:24 +0700
categories: ctf malware CyberDefenders
---

[CyberDefenders: BlueTeam CTF Challenges | Boss Of The SOC v1](https://cyberdefenders.org/blueteam-ctf-challenges/15)
![](https://user-images.githubusercontent.com/107832241/205862764-f95ac1f5-bdb0-4ffe-ac57-7c738a83bb46.png)

1. This is a simple question to get you familiar with submitting answers. What is the name of the company that makes the software that you are using for this competition? Just a six-letter word with no punctuation.

> Answer: splunk

2. What is the likely IP address of someone from the Po1s0n1vy group scanning imreallynotbatman.com for web application vulnerabilities?

![](https://user-images.githubusercontent.com/107832241/205863035-53121b7c-73f3-44fc-8b24-c753c5138c2e.png)
![](https://user-images.githubusercontent.com/107832241/205863088-c4377e39-52e1-40bb-891d-f4db616c5a34.png)
![](https://user-images.githubusercontent.com/107832241/205863088-c4377e39-52e1-40bb-891d-f4db616c5a34.png)
![](https://user-images.githubusercontent.com/107832241/205863272-a48b71ad-e234-40d8-b2fe-6dd7b32dc8bd.png)
![](https://user-images.githubusercontent.com/107832241/205863372-747bd827-9b67-4f0d-a9db-2d3979bb109f.png)
![](https://user-images.githubusercontent.com/107832241/205863503-c719a0a5-097a-4fab-a91f-a1ea139a7bcd.png)
![](https://user-images.githubusercontent.com/107832241/205863577-c0f3c30c-9df3-4381-a162-3b268982922f.png)

```
index="botsv1" "imreallynotbatman.com" sourcetype=suricata event_type=alert
```

> Answer: 40.80.148.42

3. What company created the web vulnerability scanner used by Po1s0n1vy? Type the company name. (For example, "Microsoft" or "Oracle")
![Pasted image 20221205095955](https://user-images.githubusercontent.com/107832241/205863641-b6d68a1a-8958-42b7-8c46-5d544b7ac191.png)
```
index="botsv1" "imreallynotbatman.com" 40.80.148.42 sourcetype="stream:http"
```

>Answer: Acunetix

4. What content management system is imreallynotbatman.com likely using? (Please do not include punctuation such as . , ! ? in your answer. We are looking for alpha characters only.)

> [!NOTE]
> A content management system is computer software used to manage the creation and modification of digital content. A CMS is typically used for enterprise content management and web content management. ([Content management system - Wikipedia](https://en.wikipedia.org/wiki/Content_management_system))

![Pasted image 20221205100533](https://user-images.githubusercontent.com/107832241/205863723-1b5c01d4-ebe0-4913-8c78-b025086f1d26.png)

>Answer: joomla

5. What is the name of the file that defaced the imreallynotbatman.com website? Please submit only the name of the file with the extension (For example, "notepad.exe" or "favicon.ico").

I had a lot of trouble with this question. But one thing to pay attention to is that instead of we search in the usual way (dest_ip is the address of our website), here we need to do the opposite. If this is new knowledge, can you write it in your own notes?

Why do we need to do the opposite? That's because the defaced action of the website is done after the attacker takes control of our server. And in more detail, it will be an action to download files from outside. So the query will originate from the outbound server.

```
index=botsv1 sourcetype="stream:http" src_ip="192.168.250.70"
```

![Pasted image 20221205140200](https://user-images.githubusercontent.com/107832241/205863782-cdb441f9-5af8-4dbf-b1c2-ccfd0834e5fc.png)

>Answer:  poisonivy-is-coming-for-you-batman.jpeg

6. This attack used dynamic DNS to resolve to the malicious IP. What is the fully qualified domain name (FQDN) associated with this attack?

![Pasted image 20221205140838](https://user-images.githubusercontent.com/107832241/205863854-78c5b060-2bd2-45d4-acd1-2ada78367ab6.png)

>Answer: prankglassinebracket.jumpingcrab.com

7. What IP address has Po1s0n1vy tied to domains that are pre-staged to attack Wayne Enterprises?

```
index=botsv1 sourcetype="stream:http" src_ip="192.168.250.70" uri="/poisonivy-is-coming-for-you-batman.jpeg"
```

![Pasted image 20221205142833](https://user-images.githubusercontent.com/107832241/205863924-f901f611-6fcb-4dd2-99bf-584550888579.png)

>Answer:  23.22.63.114

8. Based on the data gathered from this attack and common open-source intelligence sources for domain names, what is the email address most likely associated with the Po1s0n1vy APT group?

![Pasted image 20221205142536](https://user-images.githubusercontent.com/107832241/205863984-16f1ce65-db9f-41ee-be67-79249ead3138.png)

>Answer:  lillian.rose@po1s0n1vy.com

9. What IP address is likely attempting a brute force password attack against imreallynotbatman.com?

Since it's a brute-force attack, it will involve mainly the http POST method. In addition, *username* and *passwd* are also attributes that should be included in the filter.

```
index=botsv1 sourcetype="stream:http" imreallynotbatman.com http_method=POST form_data=*username*passwd*
| table _time src_ip dest_ip form_data
```
![Pasted image 20221205143704](https://user-images.githubusercontent.com/107832241/205864039-e537787a-4d30-47fa-bf81-ee20843def17.png)

>Answer: 23.22.63.114

10. What is the name of the executable uploaded by Po1s0n1vy? Please include the file extension. (For example, "notepad.exe" or "favicon.ico")

```
index=botsv1 sourcetype="stream:http" imreallynotbatman.com
```

![Pasted image 20221205143809](https://user-images.githubusercontent.com/107832241/205864087-3b538ac4-f7a8-4c9d-9344-e2665f54b10d.png)

>Answer: 3791.exe

11. What is the MD5 hash of the executable uploaded?

https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon#event-id-1-process-creation
![Pasted image 20221205144747](https://user-images.githubusercontent.com/107832241/205864168-7f4b43e9-e92d-4879-8220-93a19c2082fa.png)

Great, we have 76 events here. Somehow we have to filter and collapse the results. The first thing that came to my attention was the EventID, and EventID 1 is a very nice compact step. According to Microsoft Sysmon, EventID=1 means creating a process. That means when the file is uploaded to the server, launching it will create a new process.
![Pasted image 20221205145151](https://user-images.githubusercontent.com/107832241/205864238-5049832e-4c75-46fe-b34f-786d71fbb7e4.png)
Finally, filtering the results by CommandLine="3791.exe" will give the final result.
![Pasted image 20221205145251](https://user-images.githubusercontent.com/107832241/205864292-53961f0a-576a-4d1c-a886-1a6b26949b4f.png)

>Asnwer: AAE3F5A29935E6ABCC2C2754D12A9AF0

12. GCPD reported that common TTP (Tactics, Techniques, Procedures) for the Po1s0n1vy APT group, if initial compromise fails, is to send a spear-phishing email with custom malware attached to their intended target. This malware is usually connected to Po1s0n1vy's initial attack infrastructure. Using research techniques, provide the SHA256 hash of this malware.

[Host: 23.22.63.114 | ThreatMiner.org](https://www.threatminer.org/host.php?q=23.22.63.114)
![Pasted image 20221205150458](https://user-images.githubusercontent.com/107832241/205864365-7bece9b1-5ffc-4ddf-963f-85a4e6d28eba.png)
![Pasted image 20221205150510](https://user-images.githubusercontent.com/107832241/205864434-7acebacd-58ba-4a28-8a2a-29473b15fd5a.png)

>Answer: 9709473ab351387aab9e816eff3910b9f28a7a70202e250ed46dba8f820f34a8

13. What is the special hex code associated with the customized malware discussed in question 12? (Hint: It's not in Splunk)

[VirusTotal - File - 9709473ab351387aab9e816eff3910b9f28a7a70202e250ed46dba8f820f34a8](https://www.virustotal.com/gui/file/9709473ab351387aab9e816eff3910b9f28a7a70202e250ed46dba8f820f34a8/community)

>Answer:  53 74 65 76 65 20 42 72 61 6e 74 27 73 20 42 65 61 72 64 20 69 73 20 61 20 70 6f 77 65 72 66 75 6c 20 74 68 69 6e 67 2e 20 46 69 6e 64 20 74 68 69 73 20 6d 65 73 73 61 67 65 20 61 6e 64 20 61 73 6b 20 68 69 6d 20 74 6f 20 62 75 79 20 79 6f 75 20 61 20 62 65 65 72 21 21 21

14. One of Po1s0n1vy's staged domains has some disjointed "unique" whois information. Concatenate the two codes together and submit them as a single answer.
[Whois History API demo by Whoxy.com](https://www.whoxy.com/whois-history/demo.php)


![Pasted image 20221205152802](https://user-images.githubusercontent.com/107832241/205864503-d441c337-3098-4c73-87b9-6762e5bfe112.png)

15. What was the first brute force password used?

```
index=botsv1 http_method=POST form_data=*username*passwd* 
| table _time src_ip dest_ip form_data
| sort -_time | reverse
```
![Pasted image 20221205153500](https://user-images.githubusercontent.com/107832241/205864541-af855b0d-09c5-4fd1-b724-1237d6c94a04.png)

>Answer: 12345678

16. One of the passwords in the brute force attack is James Brodsky's favorite Coldplay song. Hint: we are looking for a six-character word on this one. Which is it?

>Answer: yellow

17. What was the correct password for admin access to the content management system running "imreallynotbatman.com"?

```
index="botsv1" sourcetype="stream:http" http_method=POST form_data=*username*passwd*
| rex field=form_data "passwd=(?<passwd>\w+)"
| stats count by passwd 
| sort - count
```

![Pasted image 20221205164712](https://user-images.githubusercontent.com/107832241/205865186-5a87c80a-7139-4250-ac18-20fc23d5cc4f.png)

>Answer: batman

18. What was the average password length used in the password brute-forcing attempt? (Round to a closest whole integer. For example "5" not "5.23213")

```
index="botsv1" sourcetype="stream:http" http_method=POST form_data=*username*passwd*
| rex field=form_data "passwd=(?<passwd>\w+)"
| stats count by passwd 
| sort - count
```

![Pasted image 20221205164831](https://user-images.githubusercontent.com/107832241/205865247-b38ec75f-60b7-4c07-8d89-e467d505013a.png)

>Answer: 6

19. How many seconds elapsed between the brute force password scan identified the correct password and the compromised login? Round to 2 decimal places.

```
index="botsv1" sourcetype="stream:http" http_method=POST form_data=*username*passwd*
| rex field=form_data "passwd=(?<passwd>\w+)" 
| search passwd=batman
| transaction passwd
| table duration
```
![Pasted image 20221206103243](https://user-images.githubusercontent.com/107832241/205865320-ce5eeb8f-fff7-443b-89cd-3fdf9a9766fb.png)
```
index="botsv1" sourcetype="stream:http" http_method=POST form_data=*username*passwd*
| rex field=form_data "passwd=(?<passwd>\w+)" 
| search passwd=batman 
| delta _time AS timeDeltaS 
| eval timeDeltaS=abs(timeDeltaS)
| stats sum(timeDeltaS) AS sumtime
```
![Pasted image 20221206105305](https://user-images.githubusercontent.com/107832241/205865390-b7f5b9ac-05ab-4721-a9f4-90718239166c.png)

>Answer: 92.17

20. How many unique passwords were attempted in the brute force attempt?

```
index="botsv1" sourcetype="stream:http" http_method=POST form_data=*username*passwd*
| rex field=form_data "passwd=(?<passwd>\w+)" 
| stats dc(passwd)
```
![Pasted image 20221206110604](https://user-images.githubusercontent.com/107832241/205865442-dc5b4978-b9b1-4c39-ba77-eae6c65cd841.png)

>Answer: 412

21. What was the most likely IP address of we8105desk in 24AUG2016?

```
index="botsv1"  we8105desk
```
![Pasted image 20221206110936](https://user-images.githubusercontent.com/107832241/205865486-297185a0-5908-4abb-a529-dba111c6b30e.png)

>Answer: 192.168.250.100

22. Amongst the Suricata signatures that detected the Cerber malware, which one alerted the fewest number of times? Submit ONLY the signature ID value as the answer. (No punctuation, just 7 integers.)

```
index="botsv1" sourcetype="suricata" cerber
```
![Pasted image 20221206113959](https://user-images.githubusercontent.com/107832241/205865554-3628d71e-3b17-41c9-a492-fee87c8d07c5.png)

>Answer: 2816763

23. What fully qualified domain name (FQDN) makes the Cerber ransomware attempt to direct the user to at the end of its encryption phase?

In the "Challenge Details" tab
![Pasted image 20221206115810](https://user-images.githubusercontent.com/107832241/205865622-a8a1d4c9-a801-4281-8527-cfb8642d1bed.png)
```
index="botsv1" cerberhhyed5frqa.xmfir0.win 192.168.250.100
```
![Pasted image 20221206133417](https://user-images.githubusercontent.com/107832241/205865684-df6c4417-e781-44ab-af62-87e52bf08a47.png)

>Answer: cerberhhyed5frqa[.]xmfir0[.]win

24. What was the first suspicious domain visited by we8105desk in 24AUG2016?

[IOCs/cerber.ioc at master · jasonmiacono/IOCs](https://github.com/jasonmiacono/IOCs/blob/53a0b7a5c84f38b4fd34714ccf9cd41189ec3be7/cerber.ioc#L92)

```
index="botsv1" src_ip="192.168.250.100" sourcetype="stream:dns" Solidaritedeproximite.org
| sort - _time 
| reverse
```
![Pasted image 20221206134521](https://user-images.githubusercontent.com/107832241/205865732-1e2307cf-5d6a-42ce-9875-749c3d3c7260.png)

>Answer: Solidaritedeproximite.org

25. During the initial Cerber infection a VB script is run. The entire script from this execution, pre-pended by the name of the launching .exe, can be found in a field in Splunk. What is the length in characters of the value of this field?

```
index="botsv1" vbs source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
| table CommandLine
| eval length=len(CommandLine)
```
![Pasted image 20221206140047](https://user-images.githubusercontent.com/107832241/205865863-e93f10a1-748c-4803-86f6-52ca4fe44228.png)

>Answer: 4490

26. What is the name of the USB key inserted by Bob Smith?

[USB device registry entries - Windows drivers | Microsoft Learn](https://learn.microsoft.com/en-us/windows-hardware/drivers/usbcon/usb-device-specific-registry-settings)
[Finding USB and Removable Media Detection - Splunk Community](https://community.splunk.com/t5/Splunk-Search/Finding-USB-and-Removable-Media-Detection/m-p/273918)
![Pasted image 20221206142354](https://user-images.githubusercontent.com/107832241/205865906-e77aa795-f87f-446d-8215-0afbb92fff9d.png)

>Answer: MIRANDA_PRI

27. Bob Smith's workstation (we8105desk) was connected to a file server during the ransomware outbreak. What is the IP address of the file server?

![Pasted image 20221206142717](https://user-images.githubusercontent.com/107832241/205865964-9609691a-fe96-4ad6-9eea-af33c686e15f.png)
![Pasted image 20221206142731](https://user-images.githubusercontent.com/107832241/205866024-bef511db-ef1b-4474-a6c5-160c0839b5d9.png)
```
index=botsv1 we8105desk sourcetype="stream:smb" "command{}"="smb2 session setup"
```
>Answer: 192.168.250.20

28. How many distinct PDFs did the ransomware encrypt on the remote file server?

![Pasted image 20221206143406](https://user-images.githubusercontent.com/107832241/205866072-f455f4ac-81d7-45e7-afbe-a7cff7a54dae.png)
![Pasted image 20221206143429](https://user-images.githubusercontent.com/107832241/205866122-2066512c-b2c5-4a8c-bd8e-75ac3abf8a84.png)
```
index=botsv1 pdf Source_Address="192.168.250.100"
| stats dc(Relative_Target_Name)
```
```
index=botsv1 *.pdf
| regex Relative_Target_Name=".pdf$"
| stats dc(Relative_Target_Name)
```
![Pasted image 20221206145926](https://user-images.githubusercontent.com/107832241/205866187-6807b6fb-a1d1-4c3e-b3db-28b5de9b3536.png)

>Answer: 257

29. The VBScript found in question 25 launches 121214.tmp. What is the ParentProcessId of this initial launch?

![Pasted image 20221206150318](https://user-images.githubusercontent.com/107832241/205866240-5df526fe-3210-440e-a5d7-45c0a5b9b5c6.png)
```
index=botsv1 vbs source="WinEventLog:Microsoft-Windows-Sysmon/Operational" 121214.tmp
```

>Answer: 3968

30. The Cerber ransomware encrypts files located in Bob Smith's Windows profile. How many .txt files does it encrypt?

```
index=botsv1 *.txt sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" file_path="C:\\Users\\bob.smith.WAYNECORPINC\\*.txt"
| stats dc(file_path)
```
![Pasted image 20221206151122](https://user-images.githubusercontent.com/107832241/205866307-62cfd534-ed30-4bf2-a40e-714b38795c9e.png)

>Answer: 406

31. The malware downloads a file that contains the Cerber ransomware crypto code. What is the name of that file?

```
index=botsv1 sourcetype="suricata" src=192.168.250.100 event_type=http  solidaritedeproximite.org
```
![Pasted image 20221206154031](https://user-images.githubusercontent.com/107832241/205866353-1499d65a-5777-491b-8ee0-c4955faf6568.png)

>Answer: mhtr.jpg

32. Now that you know the name of the ransomware's encryptor file, what obfuscation technique does it likely use?

>Answer: steganography