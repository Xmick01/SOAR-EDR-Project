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

* **[Vultr](https://www.vultr.com):** Cloud provider
* **LimaCharlie:** An EDR (Endpoint Detection and Response) tool for detecting malicious activities on endpoints.
* **Tines:** A SOAR (Security Orchestration, Automation, and Response) platform for automating workflows and integrating various security tools.
* **Slack:** A communication tool for sending alerts and notifications.
* **Email:** Used for sending alerts and information related to detections and actions.

## Step 1: Diagram of Playbook

![SOAR EDR playbook diagram](https://github.com/user-attachments/assets/ec7a5f1d-02f0-48bf-9145-c91e6fe49a09)
 
1. Detection: Lima Charlie detects the presence of a hacker tool on an endpoint.
2. Notification: Lima Charlie sends a detection alert to Tines. Tines then sends notifications with the relevant details to both Slack and the SOC analyst's email. These details include the time of detection, computer name, source IP, process, command line, file path, sensor ID, and a link to the detection.
3. User Prompt: The SOC analyst receives the notifications and clicks on a link to access the user prompt provided by Tines. This prompt presents the option to either isolate or not isolate the affected computer.
4. Action Based on SOC Analyst Decision:
* If the SOC analyst chooses not to isolate the computer:
  * A message is sent to Slack (specifically to the #alert channel) indicating that the computer was not isolated and advising to investigate further.
* If the SOC analyst chooses to isolate the computer:
  * Lima Charlie automatically isolates the computer.
  * A message is sent to Slack (specifically to the #alert channel) indicating that the computer has been successfully isolated.

This workflow ensures that appropriate actions are taken based on the SOC analyst's decision and that relevant parties are notified through Slack.

## Step 2: Set up VM
