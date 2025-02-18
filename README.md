# Home Lab

**Objective**

The Home Lab project aimed to establish a controlled environment for learning new tools and malware simulation all the while detecting this cyber attack. The primary focus was to create this sandbox environment to test out tools and investigate malware while having the freedom to break things without consequence. This hands-on experience was designed to deepen understanding of network security, attack patterns, techniques, and a deeper understanding of how systems and networks function.

**Summary**

In my home lab, I used the virtualization software “VirtualBox” as our hypervisor to set up both a Kali Linux machine acting as our attacker machine, along with a Windows 10 machine with Splunk and Sysmon as our victim machine with our SIEM on it giving us the telemetry to catch evil. Before beginning, I had to configure the sandbox environment to segment it from my network. Just because we have virtual machines does not mean that it won’t infect my machine or other machines on my network. Additionally, snapshots were created as well to revert it back from a known good backup before the malware infection making it good to go.

Moving on, we ran an nmap scan to discover open ports on the victim host. Shortly after, we used msfvenom to create our malware. We started building it by using our meterpreter reverse shell as our payload. We then connected our machines and ran the malware on the victim host.

**Skills Learned**

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.
- Network configuration by segmenting lab environment from personal network

**Tools Used**

- Security Information and Event Management (SIEM) system Splunk for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.
- Port scanning tools (such as nmap).
- Payload generation tool (such as msfvenom).

**Steps**

![image.png](attachment:5b807701-0dbf-4d16-aee3-84291f580001:image.png)

*Ref 1: Splunk*

This is my Splunk environment, in this screenshot you can see I’m currently viewing the Windows Event Logs. From here I can pretty much freestyle a bunch of different queries that include event codes, times, process ids. It also helps me practice other ways to display data by using things like tables, wildcards, and/or, stats count by, and more. I can always come here to not only learn Splunk but sharpen my methodology with investigation with SIEMs so that it could translated to others like Elastic, Sentinel, and any other SIEM really.

![image.png](attachment:f02a2826-aeec-45f1-b556-b396947f1fab:image.png)

*Ref 2: Sysmon Installation*

Sysmon being installed on the device as well, bolstering the telemetry about my system and helping me catch evil.

![SHV5leK9Ar.png](attachment:25943dae-4c89-4b2e-b2e7-a75e8bf19eba:SHV5leK9Ar.png)

*Ref 3: Network Connectivity*

Making sure to statically assign IP addresses to test devices to ensure segmentation from personal copmuter and internet while also allowing them to communicate with each other.

![image.png](attachment:2be8dddd-efe8-4f2c-9a60-3b1f58db60ea:image.png)

*Ref 4: Nmap Scan*

Used nmap on the “victim” host to scan for open ports.

![image.png](attachment:10063961-4475-41bb-bdca-1dc84551938a:image.png)

*Ref 5: Transferring + Downloading malware crafted from msfvenom*

We then crafted our malware with msfvenom then transferred it over to the victim machine. To the untrained eye, it can be passed off as a believable pdf file. It’s best to enable file extensions in file explorer. To do that on File Explorer you can select “View” > check “File name extensions”.

![image.png](attachment:0c6f0559-03f5-49c2-a2ec-cc614cd6f04a:image.png)

*Ref 6: Malware ran + connection established*

![image.png](attachment:fdbbbcea-f49f-45a0-9b7d-3d09d4cec1be:image.png)

*Ref 7: Connection established on Kali machine*

The connection was established on the Kali machine (Step 2). We then established a shell on the victim machine (Step 3), then ran commands on the machine (Steps 4 and 5).

![image.png](attachment:e75ea043-9ee8-4857-87cf-d49c23db50ab:image.png)

*Ref 8: Splunk findings*

We can now investigate our attacker IP address to view the timeline. We see this address was targeting mainly the rdp destination port 3389. If we were analayzing this, this spark a few questions we could ask: Should this machine be attempting to connect to our rdp port, what machine is this, who does it belong to?

![image.png](attachment:5387d8f7-9b9b-4d3f-85f8-7fc10f4c3a5e:image.png)

*Ref 9: Splunk findings*

We leveraged inspected our file Resumse.pdf.exe and grabbed the process guid from when it opened its command prompt shell. After, we inspected the process guid along with cleaning up the data in a table form in Splunk to analyze our logs. Finally, we see our commands we ran on the Kali machine in Splunk.

From here we can create detections to alert analysts like myself the next time a similar activity occurs.
