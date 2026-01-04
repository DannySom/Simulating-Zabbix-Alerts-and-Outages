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


<h2>Testing Simulated Incidents</h2>

<p>
<img width="400" height="115" alt="image" src="https://github.com/user-attachments/assets/db69814c-5291-459a-95d9-a4c29ea822a3" /> <img width="450" alt="image" src="https://github.com/user-attachments/assets/39e292ce-f896-471b-8883-21b9bb6e4a93" /> <img width="400" alt="image" src="https://github.com/user-attachments/assets/32e12f4c-6160-4106-8e40-0cdfd5382b7c" /> <img width="450" alt="image" src="https://github.com/user-attachments/assets/a86ff69b-6836-4e9b-8d70-778374a4afc4" />


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
