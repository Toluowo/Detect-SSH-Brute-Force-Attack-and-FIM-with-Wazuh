<!-- # Detect-SSH-Brute-Force-Attack-and-FIM-with-Wazuh -->
<h1>Using Wazuh Server To Detect Ssh Brute-Force Attacks And Unauthorized File Changes With File Integrity Monitor</h1>
<!-- <br><br> -->

<h2>Objective:</h2>
This project is focused on setting up a Wazuh server on an Ubuntu machine and onboarding Windows and Linux machines to detect SSH brute-force attacks and unauthorized file changes. The brute-force attack will be simulated using the Wazuh server to target an Ubuntu machine while the unauthorized file changes will be simulated using the Wazuh server and windows 11 operating system.

<h2>Lab Setup:</h2>
<ul>
  <li>System 1: Ubuntu 24.04 (Wazuh Server)</li>
  <li>System 2: Ubuntu 22.04 (Agent Machine/Target Machine - for Wazuh agent installation)</li>
  <li>System 3: Windows 11 (Target Machine – for Wazuh agent installation)</li>
  <li>Wazuh Server hardware setup:</li>
  <ul>
    <li>Storage: 50GB+</li>
    <li>RAM: 8GB+</li>
    <li>CPU: 4 vCPU’s</li>
  </ul>
  <li>Wazuh Agent (Ubuntu 22.04) hardware setup:</li>
  <ul>
    <li>Storage: 20GB+</li>
    <li>RAM: 4GB+</li>
    <li>CPU: 2 vCPU’s</li>
  </ul>
  <li>Wazuh Agent (Windows 11) hardware setup:</li>
  <ul>
    <li>Storage: 40GB+</li>
    <li>RAM: 4GB+</li>
    <li>CPU: 2 vCPU’s</li>
  </ul>
  <li>Network Connectivity: Ensure all systems can communicate over the network</li>
  <li>User permissions:</li>
  <ul>
    <li>Root or Sudo privileges on the Linux (Ubuntu) machines.</li>
    <li>Admin or PowerShell access on the Windows (11) machine.</li>
  </ul>
  <li>Virtualization: VMWare Fusion on Mac</li>
</ul>

<h1>Setting up Ubuntu VM, downloading and installing Wazuh installation assistant.</h1>
<ol>
  <li>Setup an Ubuntu virtual machine using Ubuntu 24.04 or 22.04 which will be used for the Wazuh server installation. Click here for how to setup an Ubuntu virtual machine in VMWare Fusion or VMWare Workstation or VirtualBox.</li>
  <li>Open the Ubuntu terminal and run the command:
<br><br><b><i>Sudo apt-get update && sudo apt-get upgrade</i></b><br><br>
To download package list from repositories and update them to get information on the newest version of packages and their dependencies and consequently fetches new version of packages existing on the machine.
</li>
  <li>Run the command: <br>
<br><b><i>curl -sO https://packages.wazuh.com/4.10/wazuh-install.sh && sudo bash ./wazuh-install.sh -a</i></b> <br><br>
to download and run the Wazuh installation assistant.
** Replace the version in the command with the current version or any other desired version by checking on the website and clicking the dropdown as shown below: <br><br>
</li>
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/b16ab70a-bc04-4fd0-b357-1d67d1aa0a14" /> <br><br>
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/ff89720a-d033-4f43-bba2-bcb693f557db" /><br><br>
<li>It was discovered that “curl” was not installed as shown above, so run the command: <br>
<i><b>“sudo apt install curl”</b></i> <br> to get curl installed as shown below: <br><br>
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/031795b7-ac20-4c00-9082-d8ed0c0f4e8f" /> <br><br>
<li>After successful installation of curl, run the Wazuh download and installation command again which should now download and run the Wazuh installation without any issue. Once the assistant finishes the installation, there will be an output displaying the URL for Wazuh and the login credentials and a success message as well at the bottom as shown below. Take note of the admin credential or copy it out somewhere else as this will be used to access the Wazuh homepage later.</li>
</li> <br><br>
<img width="900" alt="image" src="https://github.com/user-attachments/assets/6e470c7c-3873-4df8-ab1c-4567daafc7a7" /> <br><br>
<li>Now, navigate to the URL on your browser which is the local IP address of the Ubuntu machine on port 443. The browser will likely display a certificate error as shown below. Click on Advanced and accept the risk.</li> <br><br>
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/58792939-be28-4b29-9503-c826b6748ec6" /> <br><br>
<li>This will then load the Wazuh login page. Enter the credentials copied earlier from the installation and click enter to proceed to the overview page.</li>
  <li>The Wazuh server overview page will now load and be displayed. Click on “Deploy new agent” to start onboarding new agents as shown below:</li>
  <br><br> 
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/7755b7ff-76a8-4e4e-a5d1-d58daefaf27d" /> <br><br>
</ol>

<h2>Onboarding an Ubuntu Machine as a Wazuh Agent</h2>
<ol>
  <li>After clicking on “Deploy new agent” as shown above, the page for the agent download option opens as shown below:</li> <br><br>
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/a9151c7d-bb73-432e-b760-6a1d10c14d1a" /> <br><br>
  <li>In the Linux section, click the “DEB amd64” radio button to select it.</li>
  <li>In the “Server address” section, type in the IP address of the Ubuntu machine and toggle on the “Remember server address” option so that we don’t have to manually enter the address each time, and since it is a value that will be consistent too. Proceed to the optional settings section as shown below:</li><br><br>
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/245bb4ea-38d8-4651-a0a1-984dcd75d01c" /> <br><br>
  <li>Assign an agent name if desired as shown by (1) in the screenshot above.</li>
  <li>Leave the group as default.</li>
  <li>Copy the command as shown by (3) above and proceed to the Ubuntu machine where the agent is to be installed.</li>
  <li>Open the terminal in the other Ubuntu machine and paste the copied command above and hit enter to download and install the agent.</li> <br><br>
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/379b0baa-7cd4-4ae3-8b90-0dfddeed4cbe" /> <br><br>
  <li>After installation, enable and start the agent by running the commands represented with (4) in the agent download options screenshot above. See the screenshot below:</li> <br><br>
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/76b2bd30-b84d-47e0-8940-46e3a34b71d7" /> <br><br>
  <li>Now that the Wazuh agent is installed, enabled, started and running, go back to the Wazuh server overview page and confirm that the agent is now listed by looking at the number of agents listed as active in the agent summary at the left side.
See screenshot below:
</li> <br><br>
<img width="900" alt="image" src="https://github.com/user-attachments/assets/97814880-32d7-43f6-90cc-244359d70953" /> <br><br>
<li>From the screenshot above, one agent is now active, click on the active to see the details of the machine where the agent is running. See the screenshot below:</li> <br><br>
<img width="900" alt="image" src="https://github.com/user-attachments/assets/c8ef9c89-3233-4cc9-817c-7b0404921bd1" /> <br><br>
</ol>

<h2>Simulating An Ssh Brute-Force Attack And Detection With Wazuh Server.</h2>
We want to now detect SSH brute-force attacks on an Ubuntu machine using Wazuh’s security monitoring capabilities, so we will simulate an SSH brute-force attack using Hydra, analyze logs, and detect suspicious authentication attempts using Wazuh alerts.
<h3>Step 1: Install Hydra.</h3>
<ol>
  <li>1.	Install Hydra by running the command below from the attacker machine. For this project, we will be using the Ubuntu machine where the Wazuh server is installed to simulate the attack on the Ubuntu machine where the Wazuh agent is installed. <br>
<i>sudo apt update && sudo apt install hydra -y</i>
</li>
  <li>To simulate a brute-force attack, we need to use a “wordlist” which does not come pre-installed in Ubuntu. Download a Wordlist from here and save it to the desktop or downloads directory or any other directory of your choice. We will be using a wordlist that has 500 entries so that </li>
  <li>SSH is used for brute-forcing, so we need to install SSH if not already installed and enable it.</li>
  <li>First check the status of ssh using the command below:</li><br>
<i>Systemctl status ssh</i> <br>
    <li>If SSH is not installed, then install using the command below: <br>
<i>Sudo apt install ssh</i> <br>
  <li>If SSH is installed but not enabled, then enter the command below to enable it: <br>
<i>Systemctl enable ssh</i></li> <br>
</ol>
<h3>Step 2: Simulating a Brute-Force on the target machine</h3>
<ol>
  <li>Now that SSH is installed and enabled, proceed to simulate a brute-force attack using Hydra with the following command: <br>
<i>hydra -l <username> -P <path-to-wordlist> <target-machine-ip> ssh</i>
</li>
  <ul>
    <li>-l <username> specifies the username of the machine to attack.</li>
      <li>-P <path-to-wordlist> represents the path to the wordlist that was downloaded earlier which is a password list that will be used for brute-forcing via SSH</li>
        <li>target-machine-ip> is the IP address of the target machine.
	See the screenshot below for the execution:
</li> <br><br>
        <img width="900" alt="image" src="https://github.com/user-attachments/assets/e98ed90b-c1cd-4d6c-be34-c52825ac1527" /> <br><br>
<li>Wait for Hydra to complete the brute force and check to see if any valid password was found. In this case, no valid password was found.</li>
  </ul>
</ol>

<h3>Step 3: Detecting SSH Brute-Force Attempts on Wazuh Server</h2>
<ol>
  <li>Navigate to the Wazuh overview page again and select the “Configuration Assessment” option under the “Endpoint Security” category. See the screenshot below:</li> <br><br>
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/f311a5b1-ea01-4f47-a9b9-fdb60cd69d79" /> <br><br>
  <li>2.	From the page that opens, click on the “Events” tab (1), and on the search bar (2), type the following query to streamline the search and give us login events.
rule.id:(5551 OR 5712)
See screenshot below:
</li>
<br><br> 
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/5925bdb1-3cb2-4146-931d-9fe554c107ee" /> <br><br>
<li>It can be seen that there are multiple events with the rule.description as “PAM: Multiple failed logins in a small period of time.” This clearly indicates brute-force attempts</li>
</ol>

<h2>Investigating Unauthorized File Changes Using File Integrity Monitor In Wazuh</h2>
The objective of this task is to detect and investigate unauthorized file changes on a Windows machine using Wazuh File Integrity Monitoring (FIM). So, after configuring Wazuh file integrity monitoring, an attack will be simulated, and the alerts and logs will be monitored on the Wazuh server.
<h3>Step 1: Installing wazuh agent on windows 11</h3>
<ol>
  <li>On the Wazuh server overview page, click the “Menu” icon represented with 3 horizontal lines and labeled (1) in the screenshot below. Click on “Agents Management” and select the “Summary” submenu.</li> <br><br>
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/91c791b5-a748-4f28-b422-83d366634116" /> <br><br>
  <li>When the endpoint summary page opens, click on the “Deploy new Agent” as shown in the screenshot below.</li> <br><br>
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/cfb301f1-6728-4bd5-9338-a3fccc405a3c" /> <br><br>
  <li>The configuration options page to deploy a Windows agent will open. Select the “MSI 32/64 bits” option under the “Windows category. In the server address field, enter the IP address of the Wazuh server and toggle on the “Remember server address” option so that we don’t have to manually enter the address each time since it is a value that will be consistent.</li> <br><br>
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/a73d16d8-240a-4756-9c26-ca92bdcaaaa5" /> <br><br>
  <li>In the “Optional Settings” section, supply an agent name in the “Assign an Agent name” field if desired. We are leaving this option, and the group option fields as default here.</li>
  <li>In the “Run the following commands to download and install the agent:” field, copy the command below it labeled (2) in the screenshot below.</li>
  <br>
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/b992ba7f-d588-4cec-8262-f8776b413f91" /> <br><br>
<li>Launch the Windows 11 machine and open a PowerShell window as an administrator. Then paste the copied command into the PowerShell windows and press enter to download and install the Wazuh agent.</li>
  <img width="800" alt="image" src="https://github.com/user-attachments/assets/afbc5fa9-ad02-410a-a506-a13651c42897" /> <br><br>
<li>After installation, start the Wazuh agent by executing the command labeled (3) in the previous screenshot above: <br>
<i>NET START WazuhSvc</i><br>
See the screenshot below for both the installation and starting the service.</li> <br><br>
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/df929a44-2917-40a4-addf-45a4281cba2b" /> <br><br>
<li>Confirm that the service is running by opening the start menu and searching for “Manage agent” program and open it. Confirm that the status is showing “Running”.  See the screenshot below. </li> <br><br>
  <img width="500" alt="image" src="https://github.com/user-attachments/assets/f3e4ead3-c726-498d-90af-048d9f36274f" /> <br><br>
  </ol>
<h3>Step 2: Configuring the File Integrity Monitoring (FIM) in Wazuh</h3>
  <ol>
    <li>In the agent window, click on “View” and select “View config”. See screenshot below:</li><br><br>
    <img width="500" alt="image" src="https://github.com/user-attachments/assets/c2bcb507-4aac-4ca5-817c-6835585e4e81" /> <br><br>
    <li>This will open the Wazuh agent config file in a notepad window. Scroll up to the “File Integrity monitoring” section and introduce a new line as follows:
    <i>‹directories realtime="yes" check_all="yes" report_changes="yes"> C: \directory\to\be\monitoored </directories></i><br><br>
      <img width="900" alt="image" src="https://github.com/user-attachments/assets/3d64e1e0-1efd-47e7-85a3-ddee0aab7885" /> </li> <br><br>
      With the screenshot above, we are setting the public document folder for monitoring. Once configured, save the text file and close it.
    <li>Click on the “Manage” menu and click on the “Restart” submenu to restart the Wazuh agent service.</li><br><br>
    <img width="500" alt="image" src="https://github.com/user-attachments/assets/d247d7d3-3c43-41d4-a8cd-c79a9082c006" /> <br><br>
  </ol>
<ol>
  <h3>Step 4: Simulating an unauthorized file change:</h3>
  <li>Type the following to create a new text file “vitalInfo.txt” in the public document directory: <br>
<i>Echo "This is sensitive document to test the function of FIM on Wazuh" > C: Users Public Documents vitalInfo.txt</i>
</li><br><br>
<img width="800" alt="image" src="https://github.com/user-attachments/assets/01bd0db8-204f-42ab-9364-1b18fbc8dd17" /> <br><br>
<li>Modify the content of the new file with the following command to simulate unauthorized changes to the file: <br>
<i>Echo "This is a sensitive document to test the function of FIM on Wazuh ....... This file has now been modified ......" > C: Users Public Documents vitalInfo.txt</i> <br> </li>
<li>Delete the file with the following command to simulate data tampering:</li>
<br>
<i>Remove-Item C:\Users\Public\Documents\vitalInfo.txt -Force</i> <br>
</ol>
<h3>Step 5: Detecting the Unauthorized file change in Wazuh</h3>
<ol>
  <li>Navigate to Wazuh server dashboard from the “Endpoint Security” section, and select File Integrity Monitoring (FIM). </li>
  <li>Select the “Files” tab as shown below and select the file created earlier.</li><br><br>
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/f851547f-1601-438b-9502-7dd1a8a8fff9" /> <br><br>
  <li>Take note of the file operation actions performed earlier i.e. create, update, and delete. See screenshot below:</li><br><br>
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/0cce2385-8d1a-47ad-9b51-9ea19ef1fb44" /> <br><br>
</ol>

<h2>Conclusion.</h2>

We have been able to completely execute the following tasks:
<ul>
<li>Install Wazuh server using the quick start method.</li>
<li>eployed Wazuh agent on Ubuntu 22.04 and Windows 11 machines.</li>
<li>Configured Wazuh agent for SSH monitoring on Ubuntu machine.</li>
<li>Simulated SSH brute-force attack using Hydra on Ubuntu.</li>
<li>Investigated brute-force attempts in Wazuh server via alerts and logs.</li>
<li>Configured FIM in Wazuh on Windows 11.</li>
<li>Simulated unauthorized file modifications on Windows 11 machine.</li>
<li>Investigated unauthorized file changes from the Wazuh server alerts and logs.</li>
</ul>
