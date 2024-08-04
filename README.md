# SOAR-EDR-Project


## Objective

The SOAR EDR project focuses on creating a practical, automated security workflow using LimaCharlie, Tines, Slack, and email. The goal is to establish a streamlined process for detecting and responding to security threats. This involves setting up LimaCharlie to identify specific threats, such as a hack tool (Lazagne), and utilizing Tines to automate responses. Alerts with detailed information are sent to Slack and email, prompting a decision on whether to isolate a machine. If isolation is approved, it is automatically carried out.
The project integrates various security tools to enhance the efficiency of the detection and response process. By completion, participants will gain hands-on experience in setting up detection rules, creating automated playbooks, and managing security alerts—key skills for any cybersecurity professional.



### Skills Learned
* **EDR Configuration and Management:** Skills in setting up and managing EDR systems, including the integration and customization of LimaCharlie for endpoint security.
* **SOAR Automation and Workflow Design:** Proficiency in using SOAR tools like Tines to automate security processes and create effective incident response workflows.
* **Security Integration and Communication:** Expertise in coordinating various security tools (e.g., Slack, email) for streamlined threat detection and response.
* **Playbook Development:** Ability to design and implement automated playbooks for efficient incident management and response.
* **Alert Management and Criteria Development:** Understanding of how to create and manage detailed security alerts, and developing criteria for automated machine isolation.

### Tools Used

* **[Vultr](https://www.vultr.com):** Cloud infrastructure for creating and managing virtual machines
* **[LimaCharlie](https://limacharlie.io/):** An EDR (Endpoint Detection and Response) tool for detecting malicious activities on endpoints.
* **[Tines](https://www.tines.com/):** A SOAR (Security Orchestration, Automation, and Response) platform for automating workflows and integrating various security tools.
* **Slack:** A communication tool for sending alerts and notifications.
* **Email:** Used for sending alerts and information related to detections and actions.

## Step 1: Diagram of Playbook

![SOAR EDR playbook diagram](https://github.com/user-attachments/assets/ec7a5f1d-02f0-48bf-9145-c91e6fe49a09)
 
1. Detection: Lima Charlie detects the presence of a hacker tool on an endpoint.
2. Notification: Lima Charlie sends a detection alert to Tines. Tines then *sends notifications with the relevant details to both Slack and the SOC analyst's email. These details include the time of detection, computer name, source IP, process, command line, file path, sensor ID, and a link to the detection.
3. User Prompt: The SOC analyst receives the notifications and clicks on a link to access the user prompt provided by Tines. This prompt presents the option to either isolate or not isolate the affected computer.
4. Action Based on SOC Analyst Decision:
* If the SOC analyst chooses not to isolate the computer:
  * A message is sent to Slack (specifically to the #alert channel) indicating that the computer was not isolated and advising to investigate further.
* If the SOC analyst chooses to isolate the computer:
  * Lima Charlie automatically isolates the computer.
  * A message is sent to Slack (specifically to the #alert channel) indicating that the computer has been successfully isolated.

This workflow ensures that appropriate actions are taken based on the SOC analyst's decision and that relevant parties are notified through Slack.

## Step 2: Install and Setup LimaCharlie 

* Use a cloud provider, like Vultr, or virtualization software like VirtualBox or VMware to create two virtual machines (VMs). One VM machine will be Kali Linux and the other machine will be a Windows machine.

![vultr microsoft machine](https://github.com/user-attachments/assets/d61cd845-e3ec-449f-bce4-b626b9fcdb8b)

* Add a firewall to the instance. Select MS RDP (Microsoft Remote Desk Protocol) at port 3389 and the source will be "My IP Address."
 ![SOAR EDR firewall](https://github.com/user-attachments/assets/844ca1af-6310-4159-9cad-1b9181e0a182)
  * Restricting Access: By setting the source to "My IP Address," the firewall rule ensures that only the IP address specified can initiate an RDP session. This restricts access to a known, trusted IP address and blocks any unauthorized IP addresses from attempting to connect.
  * Enhanced Security: This configuration adds a layer of security by limiting potential attack vectors. Only connections from the specified IP address will be allowed, reducing the risk of unauthorized access and brute-force attacks.
* Sign up for LimaCharlie

![limacharlie dashboard](https://github.com/user-attachments/assets/4a63c936-9c86-45e7-9a7b-7121f445034b)

* Navigate to the installation key and download the correct EDR window download. Copy the link address and past it into the url. Copy the sensor key as well. 

![EDR windows download ](https://github.com/user-attachments/assets/3269f7c3-c784-400b-8f54-a160661f8ac5)

![installation info](https://github.com/user-attachments/assets/91f186b6-13c0-47aa-b482-b3eabadf3069)

* Paste the installation key
 
![powershell install](https://github.com/user-attachments/assets/e67a11c8-8810-4c30-9c59-8ecf34f992cb)

![powershell install successful](https://github.com/user-attachments/assets/14300337-b268-4469-a1f1-2f801ec335f9)

* After installing, check LimaCharlie to see if the server is listed.

![limacharlie sensor list info](https://github.com/user-attachments/assets/d13bbd7a-bd5d-4937-ba03-83b4b95ed5af)

## Step 3: Install Lazagne and Create Detection and Response Rule

* Go to the [github](https://github.com/AlessandroZ/LaZagne) for Lazagne and download it. It is a hacker tool, so make sure to turn off Windows Defender to avoid blocking the download.

![lazagne ran](https://github.com/user-attachments/assets/e3bd9cce-4698-488a-a8e6-1e3fb9231b42)

* After installing, check LimaCharlie to see if the server is listed.

![limacharlie sensor list info](https://github.com/user-attachments/assets/d13bbd7a-bd5d-4937-ba03-83b4b95ed5af)

* After installation and execution of Lazagne should be detected by LimaCharlie. Just search the timeline for Lazagne and it should be there.

![lazagne timeline](https://github.com/user-attachments/assets/7006169c-7974-4650-9ff6-a0aa94a23a03)

  * The expanded event tab gives additional information on the event. It shows the command line, file sign, file path, hash, parent, powershell, username, etc.
* Navigate to the Automation tab and create a new detection and response rule, here is the template for that from [github](https://github.com/MyDFIR/SOAR-EDR-Project?tab=readme-ov-file)
   * It is best to look for templates for rules like this rather than build a rule from scratch.
   * This detection and response rule monitors for the execution of LaZagne, a hacking tool, on Windows systems. It triggers when a new or existing process matches any of these conditions: the file path ends with "LaZagne.exe," the command line ends with "all" or contains "LaZagne," or the file hash matches a known LaZagne hash. When these conditions are met, the system initiates a response, such as alerting SOC analysts and isolating the affected machine.
* Verify that the rules are good by testing them before moving on.

![event rules test](https://github.com/user-attachments/assets/5f7324c8-f1aa-44e1-a1a8-0866ccf37f5d)

* Run Lazagne again and LimaCharile should capture it.

![detections](https://github.com/user-attachments/assets/d4a34757-c7cf-449b-93e8-8ca37cb443ed)

## Step 4: Setup Slack and Tines and Test Connection between Tines and LimaCharlie 

* Set up an account on [Slack](https://slack.com/)

![sign up for slack](https://github.com/user-attachments/assets/15b4d34b-4c9f-496c-9871-6de75ecfe275)

* Create a channel for alerts. SOC Analysts tend to have a dedicated channel for alerts.
![create dedicated alerts channel in slack](https://github.com/user-attachments/assets/af12e7d3-0824-46ac-885c-259d2272b167)

* Set up an account on [Tines](https://www.tines.com/) and add a webhook. Copy the webhook url and go to LimaCharlie. Go to outputs and select Tines and paste the url.

![output tines limacharlie webhook](https://github.com/user-attachments/assets/230f2da2-750d-47a1-b6fb-c7fd9bbf83d4)

* If everything works correctly, Tines should have the correct output alert.

## Step 5: Send a Message to Email and Slack and Generate a User Prompt

* Integrate Slack into Tines. Slack is compatible with Tines because Tines is a SOAR and SOARs tend to integrate with messaging apps like Slack easily.

![slack tines](https://github.com/user-attachments/assets/aaddb801-8a55-4825-ba73-c1fc4f4be1b2)

* Add the channel id to Tines in order to have the alert sent to the dedicated alert channel.

![slack alert channel id](https://github.com/user-attachments/assets/b5ec94c8-9b0d-4573-83e5-b7bf155ce44e)

![slack alert channel id 2](https://github.com/user-attachments/assets/01e31be0-a24d-422c-a17d-73960bbc8599)

