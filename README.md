<p align="center">
<img width="600" height="300"  alt="download (1)" src="https://github.com/user-attachments/assets/17a37c1d-3eca-4976-a339-0e2fd80fe10e" />
</p>

<h1>Simulating Monitoring Alerts and Incident Responses in Zabbix</h1>
In this section, I simulated multiple monitoring alerts and incidents in Zabbix to replicate real-world IT/NOC scenarios. Simulations include testing avalability, ping, alerts, triggers, and observing the graphs. These simulations generated alerts with appropriate severity levels, allowing me to verify monitoring functionality, practice troubleshooting, and understand how Zabbix differentiates between service-level and infrastructure-level failures. This exercise reinforced my ability to detect, investigate, and respond to incidents in a controlled lab environment. <br />


<h2>Environments and Technologies Used</h2>

- Digital Ocean (Virtual Machines/Compute)
- Putty

<h2>Operating Systems Used </h2>

- Ubuntu 24.04 (LTS) x64
- Centos 9 Stream x64
- Windows 11 x64


<h2>Simulating Agent Unreachable</h2>

<p>
<img width="400" height="115" alt="image" src="https://github.com/user-attachments/assets/db69814c-5291-459a-95d9-a4c29ea822a3" /> <img width="450" alt="image" src="https://github.com/user-attachments/assets/39e292ce-f896-471b-8883-21b9bb6e4a93" /> <img width="400" alt="Screenshot 2026-01-04 200915" src="https://github.com/user-attachments/assets/4f46f8b1-e53a-4c47-b7e4-9c8344882838" />
 <img width="450" alt="image" src="https://github.com/user-attachments/assets/f3ba3d01-1b9b-4627-a272-bc15b9d5547f" />




</p>
<p>
Now I'm going to test the dashboard to see if Zabbix server will give me a response if an agent goes down. I went into Host-2, and typed in the command `service zabbix-agent stop.` </p>
Once the agent was stopped, Zabbix detected the issue and displayed a new problem under Monitoring → Problems, classified with a Disaster severity level. The issue was also reflected in Monitoring → Hosts, where Host-2 showed a red availability indicator, signaling a problem state. </p> The error message states "Get value from agent failed: Cannot establish TCP connection to [[10.120.0.3]:10050]: [111] Connection refused"</p>
This error confirmed that the network is Ok, the Host is up, but the Agent is down. If the host was actually down, then the error message would state that the host is unreachable and couldn't ping host-1 but we could still ping the Host </p>
If I also click update problem, I could put a note in there so if anybody else is monitoring Zabbix, they will see the information provided on the problem. </p>
</p>
<br />

<p>
<img width="400" alt="image" src="https://github.com/user-attachments/assets/bcf23f45-7b30-439c-a3eb-946f2272500b" /> <img width="450" alt="image" src="https://github.com/user-attachments/assets/02cf0bdf-ea17-4234-8c3f-4d193fd27534" />

</p>
<p>
To resolve the incident, I restarted the Zabbix Agent service on Host-2 by typing `service zabbix-agent start` . Zabbix then automatically detected recovery, cleared the alert, and restored the host to a normal operational state. </p>
This test confirmed that triggers were correctly configured to detect service outages, generate alerts, and resolve incidents once the underlying issue was corrected.
</p>
<br />

<h2>Simulating Host is Down</h2>

<p>
<img width="1500" alt="image" src="https://github.com/user-attachments/assets/011477ca-c4d0-48dd-b64e-cbc6da17f384" /> <img width="1500" alt="image" src="https://github.com/user-attachments/assets/6f335ebb-f9a5-4a64-a2b2-d88adb6fa950" />
</p>
<p>
Now, to simulate a host outage, I stopped the VM Host-1. </p>
Notice instead of the error message saying connection refused, it reported "no route to host". Instead of the agent misbehaving like it did in the last incident, the host is actually offline which indicates the host is completely unreachable. </p>
It also triggered the alert which was `NODATA 60 seconds` meaning it tried pinging Host-1 but it didnt get a response back. </p>
This could be a network issue, or it's just that the the computer is just simply turned off.
</p>
<br />

<p>
<img width="1500" alt="image" src="https://github.com/user-attachments/assets/011477ca-c4d0-48dd-b64e-cbc6da17f384" /> <img width="1500" alt="image" src="https://github.com/user-attachments/assets/6f335ebb-f9a5-4a64-a2b2-d88adb6fa950" />
</p>
<p>
Then I'll try pinging Host-1 myself from Zabbix Server. </p>
After a couple of seconds, ping returned no response, confirming that the host availability alert was correctly triggered. </p>
To resolve this, I restarted the VM Host-1, waited for the agent to reconnect, confirmed the alert cleared, and verified normal operation in the dashboards.
</p>
<br />

<p>
<img width="1732" height="429" alt="image" src="https://github.com/user-attachments/assets/9789787e-b780-4a23-9913-1cbda205f760" />
</p>
<p>
Trigger
</p>
<br />

<h2>Simulating High CPU Usage</h2>

<p>
<img width="589" height="81" alt="image" src="https://github.com/user-attachments/assets/6d1d8deb-adcf-44b7-8883-2a64e420faca" />
<img width="600" alt="Screenshot 2026-01-04 232807" src="https://github.com/user-attachments/assets/e5f09a39-ef89-4c95-8c5d-d5acff4bb4e6" />
</p>
<p>
 
To simulate high CPU alert, I typed in the command, ``stress --cpu 1 --timeout 600`` on Host-1.

`stress` → simulates CPU overload

`--cpu 1` → one CPU worker will generate load. Host-1 has one vCPU which will max out this one core.

`--timeout 600` → runs for 600 seconds which is 10 minutes </p>

This triggered an alert which was "High CPU Usage 75% for 2 minutes" which we can see in Monitoring → Problems.
</p>
<br />


<p>
<img width="1729" height="417" alt="Screenshot 2026-01-04 233357" src="https://github.com/user-attachments/assets/76c0c7f8-120f-476a-b962-5744e9448c10" />
</p>
<p>
 
In the CPU utilization graph, we can see that CPU usage spikes to ~100% at 05:19, which corresponds to the start of the stress test `stress --cpu 1 --timeout 600`. The CPU remains at this high level for the full 10-minute duration, and returns to baseline (-<5%) at 05:29 after the stress process ends.

</p> In real-world scenarios, short CPU spikes are normal and often caused by routine tasks. However, if CPU utilization remains sustained above ~75%, this usually indicates a potential issue, such as a runaway process, a Denial-of-Service (DoS) attack, or unusually heavy traffic.
</p>
<br />


<p>
<img width="656" height="410" alt="image" src="https://github.com/user-attachments/assets/77c97c6e-1e0d-4177-bc7e-3d727acbc8e3" />
</p>
<p>

To identify the process causing high CPU utilization, I ran `top` on the host VM. This allowed me to see which process was consuming the most CPU resources in real time.

</p> 

Here, we can see that the `stress` process is consuming approximately 99.7% of CPU. We can also identify it by its PID and the user account running the process.

</p>

<br />


<p>
<img width="654" height="411" alt="image" src="https://github.com/user-attachments/assets/58daae64-4f05-4509-97da-e26ca7c10dd6" /> <img width="647" height="413" alt="image" src="https://github.com/user-attachments/assets/698a4532-c20e-4876-8b32-b7158ea39cbf" />

</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
