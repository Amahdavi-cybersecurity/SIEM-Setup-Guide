# SIEM-Setup-Guide
A beginner-friendly guide to setting up a SIEM tool using Elasticsearch, Kibana, and Winlogbeat, complete with step-by-step instructions and screenshots.
## Project Overview
This project demonstrates the implementation of a **Security Information and Event Management (SIEM)** solution using the Elastic Stack (Elasticsearch, Kibana, and Beats). The goal is to configure and deploy a free, open-source SIEM tool to monitor and analyze security events in real-time. The project includes step-by-step configuration and deployment instructions, ensuring optimal setup and functionality.

---

## Tools Used
- **Operating Systems**:
  - **Ubuntu**: Host for Elastic Stack components (Elasticsearch, Kibana, and Beats).
  - **Windows 10**: Used to generate and forward logs using Winlogbeat.
- **Virtual Environment**:
  - **VirtualBox**: Both Ubuntu and Windows 10 are running as virtual machines for isolated configuration and testing.
- **Elastic Stack Components**:
  - **Elasticsearch**: For storing and analyzing logs.
  - **Kibana**: For visualization and dashboard creation.
  - **Winlogbeat**: For collecting and shipping logs from Windows 10 to Elasticsearch.

---

## Skills Demonstrated
- Configured the Elastic Stack for a real-time SIEM solution.
- Installed and set up Elasticsearch, Kibana, and Beats on Ubuntu.
- Collected logs from a Windows 10 machine using Winlogbeat.
- Visualized and analyzed security events in Kibana dashboards.
- Troubleshot and resolved compatibility and service configuration issues.

---

## Description
This project showcases the step-by-step implementation of a SIEM solution using open-source tools. It includes:
- Installing and configuring the Elastic Stack on an Ubuntu virtual machine.
- Setting up Winlogbeat on a Windows 10 virtual machine for log forwarding.
- Creating and customizing Kibana dashboards for log analysis.
- Troubleshooting service configurations and ensuring proper communication between components.

The project is designed to provide hands-on experience with SIEM setup, making it an ideal proof-of-concept for organizations exploring open-source security monitoring tools.

## Step 1: Setting Up the Environment

![Screenshot (1222)](https://github.com/user-attachments/assets/df3b9229-8540-4a39-9ad0-83a265786155)

In this step, we are configuring the Ubuntu environment to install Elasticsearch securely.

**Command Executed**

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg

-------
## Step 2: Install apt-transport-https

![Screenshot (1223)](https://github.com/user-attachments/assets/a45ab865-864f-47e9-b6b6-86db88de6919)

In this step, the apt-transport-https package is installed on the Ubuntu system.

**Command Executed:**

sudo apt-get install apt-transport-https

**Purpose:**
apt-transport-https is a required package for enabling the Ubuntu package manager (apt) to communicate with repositories over secure HTTPS connections.
This ensures the system can securely download Elasticsearch and other Elastic Stack components from the official repositories.

**Output:**
The package is successfully installed, as indicated by the "Setting up apt-transport-https" message.
No additional upgrades or removals were performed during this operation.
This step is essential for ensuring secure communication with external repositories. After completing this, the system is ready to add the Elasticsearch repository.

----------

## Step 3: Add Elasticsearch Repository

![Screenshot (1224)](https://github.com/user-attachments/assets/d7339077-045c-4607-ae28-67bced5e00ae)

in this step, the official Elasticsearch repository is added to the Ubuntu system.

**Command Executed:**

echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list

**Purpose:** This command integrates the Elasticsearch package source into the system's repository list. It ensures that any Elastic Stack components, such as Elasticsearch and Kibana, can be securely downloaded and installed. The signed key provides verification to prevent unauthorized package modifications.

**Output:** The repository was successfully added to the system, as confirmed by the displayed path: /etc/apt/sources.list.d/elastic-8.x.list.

This step prepares the system to install Elasticsearch and other Elastic Stack tools securely.

--------

## Step 4: Update Package List

![Screenshot (1225)](https://github.com/user-attachments/assets/24b0eddc-b06c-4dff-abef-df45f394c954)

In this step, the system's package list is updated to include the newly added Elasticsearch repository.

**Command Executed:**

sudo apt update

**Purpose:** This command ensures that the system fetches the latest package information from all configured repositories, including the newly added Elasticsearch repository. It prepares the system for installing the required Elastic Stack components.

**Output:** The command successfully updates the package list and indicates that 88 packages can be upgraded. This step ensures the system is ready for subsequent installations.

-------

## Step 5: Install Elasticsearch

![Screenshot (1226)](https://github.com/user-attachments/assets/1d8926de-a706-48ba-be15-9ecd6c8fbd5d)


In this step, Elasticsearch is installed on the Ubuntu system.

**Command Executed:**

sudo apt install elasticsearch -y

**Purpose:** This command downloads and installs Elasticsearch from the previously configured repository. The -y flag automatically confirms the installation process.

**Output:** Elasticsearch is being installed, with the download progress visible. The system confirms the successful installation, setting the stage for configuring Elasticsearch in the next steps.

----

## Step 6: Start and Enable Elasticsearch Service

![Screenshot (1227)](https://github.com/user-attachments/assets/7d868ec5-7157-4e73-9b3b-5e5f1a737a7e)

In this step, Elasticsearch is started and configured to run automatically at system startup.

**Commands Executed:**

sudo systemctl daemon-reload

sudo systemctl enable elasticsearch.service

sudo systemctl start elasticsearch.service

**Purpose:**

sudo systemctl daemon-reload: Reloads the systemd manager configuration to recognize new or updated service files.

sudo systemctl enable elasticsearch.service: Configures Elasticsearch to start automatically at boot.

sudo systemctl start elasticsearch.service: Starts the Elasticsearch service immediately.

**Output:** Elasticsearch is now running, with TLS and authentication configured. The auto-generated password and enrollment tokens for future Kibana or node configurations are displayed.

---------

## Step 7: Verify Elasticsearch Service Status

![Screenshot (1228)](https://github.com/user-attachments/assets/1da13a80-7984-4a76-afad-a5953d3c2fb2)

In this step, we check the status of the Elasticsearch service to confirm it is running.

**Command Executed:**

sudo service elasticsearch status

**Purpose:** This command verifies if the Elasticsearch service is active and running. It provides details such as uptime, memory usage, and associated processes.

**Output:**
The status shows active (running) confirming that Elasticsearch is successfully running.
Additional details, like memory allocation and process IDs, are displayed for troubleshooting or monitoring purposes.

--------

## Step 8: Stop Elasticsearch Service

![Screenshot (1229)](https://github.com/user-attachments/assets/b3e45ac1-924b-42b3-bd65-15903e70f238)

In this step, we stop the Elasticsearch service.

**Command Executed:**

sudo service elasticsearch stop

**Purpose:** Stopping the Elasticsearch service is useful when performing maintenance, updates, or troubleshooting to ensure the service does not interfere with ongoing changes.

**Output:** The command executes without errors, indicating the Elasticsearch service has been successfully stopped.

-------

## Step 9: Editing the Elasticsearch Configuration File

![Screenshot (1230)](https://github.com/user-attachments/assets/02f8c8fb-1759-45ae-b178-509f1029356c)

In this step, the elasticsearch.yml file is edited to configure Elasticsearch settings.

**File Location:**

/etc/elasticsearch/elasticsearch.yml

**Changes Made:**

Uncommented and updated the network.host to 10.0.2.15, allowing Elasticsearch to bind to a specific IP address.

Uncommented and set the discovery.seed_hosts to ["10.0.2.15"] for node discovery in a multi-node cluster.

**Purpose:** These configurations allow the Elasticsearch node to be accessible on the specified IP address and prepare it for cluster discovery.

**Output:** The updated configuration ensures the node can communicate effectively within a network or cluster environment. After saving, these settings will take effect when Elasticsearch is restarted.

-------------

## Step 10: Verifying and Stopping Elasticsearch

![Screenshot (1231)](https://github.com/user-attachments/assets/f63b0b0f-d4f6-49ee-8866-6b705ee216ed)

In this step, the status of the Elasticsearch service is verified and then stopped.

**Commands Executed:**

**Check Elasticsearch Status:**

sudo service elasticsearch status

**Purpose:** Confirmed that the Elasticsearch service was running successfully.

**Stop Elasticsearch Service:**

sudo service elasticsearch stop

**Purpose:** Stopped the Elasticsearch service after confirming it was active.

**Output:** The service status showed it was active before stopping.
After executing the stop command, Elasticsearch was deactivated successfully.
Why this step matters: This step ensures the service can be gracefully stopped when required, avoiding any abrupt disruptions or issues.

------

## Step 11: Restarting and Verifying Elasticsearch Service

![Screenshot (1232)](https://github.com/user-attachments/assets/7aaeb342-8479-4c6c-bee4-c8c40d8f3c62)

In this step, Elasticsearch is restarted, and its status is verified.

**Commands Executed:**

**Start Elasticsearch Service:**

sudo service elasticsearch start

**Purpose:** Restarts the Elasticsearch service to ensure it is running.

**Check Elasticsearch Status:**

sudo service elasticsearch status

**Purpose:** Confirms that Elasticsearch is now active and running.

**Output:** The service status indicates it is active and running successfully.
Additional details, such as the process ID and memory usage, are displayed.
Why this step matters: Restarting the service ensures Elasticsearch operates correctly after configuration changes, and verifying its status confirms the changes are effective.

-------

## Step 12: Testing Elasticsearch Connectivity

![Screenshot (1233)](https://github.com/user-attachments/assets/cc3b3e00-b20a-4cd5-ac7c-b224735a0cde)

In this step, we test the Elasticsearch installation to ensure it is running and accessible.

**Commands Executed:**

**Check Listening Ports:**

netstat -tuln | grep 9200

**Purpose:** Verifies that Elasticsearch is listening on port 9200, indicating that the service is up and running.

**Query Elasticsearch:**

curl -X GET "http://10.0.2.15:9200"

**Purpose:** Sends a GET request to the Elasticsearch server to check its response and confirm successful connectivity.

**Output:** The netstat command shows that port 9200 is in the LISTEN state.
The curl command returns a JSON response with details about the Elasticsearch cluster, including version, build, and tagline: "You Know, for Search."
Why this step matters: This step ensures that Elasticsearch is correctly installed, running, and accessible over the network at the specified IP address and port. It verifies the health of the setup and confirms that the service is ready for further configurations or integrations.

------------

## Step 13: Installing Kibana

![Screenshot (1236)](https://github.com/user-attachments/assets/5d5052a0-52ab-4168-ab2a-6761e209e981)

In this step, Kibana, the visualization component of the Elastic Stack, is installed.

**Command Executed:**

sudo apt install -y kibana

**Purpose:** This command installs Kibana from the Elastic repository. Kibana provides an interface to interact with Elasticsearch, enabling you to visualize and analyze data.

**Output:** The Kibana package is downloaded and installed successfully.
A Kibana keystore is created in /etc/kibana/kibana.keystore.
A note about legacy OpenSSL providers being enabled is displayed, with a link to disable them if necessary.
Why this step matters: Kibana is essential for visualizing Elasticsearch data. Completing this step ensures the Elastic Stack is ready for data visualization and further configuration.

----------

## Step 14: Installing Auditbeat

![Screenshot (1237)](https://github.com/user-attachments/assets/129e5118-2cd0-4c61-9aec-cf694f25bf93)

In this step, Auditbeat, a lightweight shipper for auditing activities on your system, is installed.

**Command Executed:**

sudo apt install auditbeat -y

**Purpose:** Auditbeat is a part of the Elastic Stack that collects audit data and forwards it to Elasticsearch or Logstash for further analysis. This tool is commonly used for monitoring system security and compliance.

**Output:** Auditbeat package is downloaded and installed successfully.
The setup process ensures all required dependencies are installed.
Why this step matters: Auditbeat enables monitoring of user activity and file integrity, making it a crucial component for security analysis in the Elastic Stack.

----------

## Step 15: Configuring Kibana and Auditbeat
![Screenshot (1238)](https://github.com/user-attachments/assets/7c6d2ca9-4b9e-453c-a8ba-106240c7e48a)
![Screenshot (1239)](https://github.com/user-attachments/assets/896bcaee-6b13-4015-a560-55ac558547de)
![Screenshot (1240)](https://github.com/user-attachments/assets/3b85164d-26d4-4b58-944c-39826a2f3fed)
![Screenshot (1241)](https://github.com/user-attachments/assets/fd458d4f-b9ec-49c4-a33c-314e079b9fca)
![Screenshot (1242)](https://github.com/user-attachments/assets/d9879126-aba6-46e5-9535-acbd6b752442)



nano command opened the Kibana configuration file (/etc/kibana/kibana.yml) to configure Elasticsearch integration.

The Elasticsearch host is set (elasticsearch.hosts) to http://10.0.2.15:9200.

Configuring the Auditbeat configuration file (/etc/auditbeat/auditbeat.yml) to specify the Elasticsearch output settings.

The Auditbeat output host is also pointed to http://10.0.2.15:9200.

**Commands Executed:**

sudo nano /etc/kibana/kibana.yml

sudo nano /etc/auditbeat/auditbeat.yml

**Purpose:** This step links Kibana and Auditbeat to the Elasticsearch node, ensuring that all logs and analytics can flow between these components for proper data visualization and security monitoring.

**Note**: Proper file permissions are required to edit these files. Ensure both configurations are saved with correct indentation.

-----

## Step 16: Running Auditbeat

![Screenshot (1243)](https://github.com/user-attachments/assets/cbc1acd5-51bc-4de3-b3b7-8e4faf19ff99)

In this step, Auditbeat is executed to monitor and send logs to the configured Elasticsearch instance.

**Command Executed:**


sudo auditbeat -e

**Purpose:** This command starts Auditbeat in debug mode (-e), displaying detailed logs in the terminal. This helps confirm that Auditbeat is working correctly and communicating with Elasticsearch.

**Output:** Auditbeat starts collecting data and outputs logs to the terminal.
Messages confirm the connection to the Elasticsearch instance and the data being sent.
Note: Check the logs for any warnings or errors to ensure the process runs smoothly. If issues arise, revisit the configuration files for corrections.

-----------

## Step 17: Accessing Kibana Dashboard

![Screenshot (1244)](https://github.com/user-attachments/assets/a533b8f0-da50-415f-851f-d8b63917fda6)

In this step, the Kibana dashboard is accessed via the browser.

**Details:**

The Kibana interface is successfully loaded at http://127.0.0.1:5601.
This page confirms that Kibana is set up and connected to Elasticsearch.
Purpose: The Kibana dashboard is a crucial part of the Elastic Stack, providing tools for data visualization, security monitoring, and analytics.

**Next Steps:** You can now explore the Kibana features, such as search, observability, security, and analytics, or start by adding data integrations.

------

## Step 18: Installing and Configuring Sysmon

![Screenshot (1245)](https://github.com/user-attachments/assets/2d369581-9c72-4d07-baba-c9401533e88c)
![Screenshot (1246)](https://github.com/user-attachments/assets/2d920d6f-1266-4b15-8205-fff922637843)
![Screenshot (1247)](https://github.com/user-attachments/assets/011d1399-d7c4-4e73-b978-298ff47aebef)
![Screenshot (1248)](https://github.com/user-attachments/assets/310be8fe-4b1c-4e07-8cba-990257e32f90)
![Screenshot (1249)](https://github.com/user-attachments/assets/92f91902-fcac-441e-abf8-53b589f78d59)
![Screenshot (1250)](https://github.com/user-attachments/assets/766755b8-904a-4a14-9cd9-45d75cacade1)


**Overview:**
Sysmon, a system monitoring tool, is installed and configured to track system activity and events.

**Actions Taken:**
Downloaded and extracted the Sysmon executable files to the system.

Obtained a Sysmon configuration file (e.g., sysmonconfig.xml) from a public GitHub repository.

**Installed Sysmon using the following command:**

sysmon -i sysmonconfig.xml -accepteula -h sha256 -l -n

**Purpose:**

Sysmon logs detailed system activity, such as process creation, file creation, and network connections, for enhanced security monitoring.

**Verification:**

Checked Sysmon's status in the Windows "**Services**" app, where it appeared as running.

Verified that logs were being generated according to the configuration file.

----

## Step 19: Installing and Configuring Winlogbeat

![Screenshot (1251)](https://github.com/user-attachments/assets/a24d073e-69c9-4415-aa8e-ad141841aad9)
![Screenshot (1252)](https://github.com/user-attachments/assets/2cbe7079-02c5-44ca-9d0e-582144076246)
![Screenshot (1253)](https://github.com/user-attachments/assets/54351ce0-57dc-4eaf-8f2b-24460de181d3)
![Screenshot (1254)](https://github.com/user-attachments/assets/fd3331d7-75f7-4e48-9db2-58d2694c3dc0)

**Overview:**
In this step, Winlogbeat, a lightweight shipper for forwarding and centralizing Windows Event Log data, is installed, configured, and set up as a service.

**Actions Taken:**

Downloaded and unzipped the Winlogbeat package into the C:\Program Files directory.

Renamed the extracted folder to Winlogbeat with a capital "W" for consistency and ease of use.

Opened PowerShell and ran the command to install Winlogbeat as a service:

./install-service-winlogbeat.ps1

Navigated to the Windows "Services" app, located the Winlogbeat service, and changed its startup type from Automatic to Manual.

**Purpose**: Winlogbeat collects and ships Windows Event Logs to the configured Elasticsearch instance for real-time monitoring and analytics.

**Notes:**
The startup type was set to Manual to ensure greater control over when the service runs.

Configuring the service ensures that event log data can be collected and analyzed as needed.

-------

## Step 20: Configuring Kibana, Elasticsearch, and Winlogbeat

![Screenshot (1255)](https://github.com/user-attachments/assets/fc0cbd63-7d48-4295-bb25-79b1cac5c037)
![Screenshot (1256)](https://github.com/user-attachments/assets/558e95c2-beba-493c-90e0-85ac9e8b651a)
![Screenshot (1257)](https://github.com/user-attachments/assets/c0f44e38-70ae-4117-921d-d5d9a5a9ac59)
![Screenshot (1258)](https://github.com/user-attachments/assets/b2eb0a16-437c-4885-8271-a0d4b9a9ceb9)
![Screenshot (1259)](https://github.com/user-attachments/assets/4765c94e-0a0b-4f8d-b12c-4a2f40febde5)
![Screenshot (1260)](https://github.com/user-attachments/assets/77ab0db9-0e32-4874-a43c-ed426d5bf871)
![Screenshot (1261)](https://github.com/user-attachments/assets/487f3954-5c10-4213-8af2-cac93c17340a)
![Screenshot (1262)](https://github.com/user-attachments/assets/ce067663-bbf6-4258-828b-6ca5858c8851)


**Actions Taken:**

On Ubuntu:

Opened the Kibana configuration file with the command:

sudo nano /etc/kibana/kibana.yml

Changed the server.host configuration to match the system's IP address.

Restarted the services in sequence to apply the changes:

sudo service elasticsearch stop

sudo service kibana stop

sudo service auditbeat stop

sudo service elasticsearch start

sudo service kibana start

sudo service auditbeat start

**On Windows 10 Virtual Machine:**

Navigated to C:\Program Files\Winlogbeat and opened the winlogbeat.yml file using Notepad.

Updated the following configurations:

Kibana Host: Changed the kibana.host to the IP address of the host machine.

Elasticsearch Output Host: Updated the output.elasticsearch.hosts to match the IP address of the Elasticsearch instance.

Saved the changes to the file.

**Running Winlogbeat:**

Opened PowerShell and executed the following command to set up and connect Winlogbeat:

./winlogbeat.exe setup -e

**Purpose:** This step ensures that Kibana is properly configured to listen on the correct IP address for external connections.

Winlogbeat is connected to both Elasticsearch and Kibana, allowing it to send Windows Event Log data for centralized monitoring and analysis.

**Notes:**

Restarting the services ensures that configuration changes take effect.

The setup -e command initializes Winlogbeat and tests its connectivity with Elasticsearch and Kibana.

**Why This Step Matters:**

This configuration bridges the gap between the Ubuntu-hosted Elastic Stack and the Windows Event Logs collected by Winlogbeat. By aligning the IP addresses across all components, the system achieves a seamless flow of data for monitoring, visualization, and alerting purposes.

------

## Step 21: Accessing Elasticsearch and Monitoring Data

![Screenshot (1264)](https://github.com/user-attachments/assets/1072de31-a276-4cfa-9f4b-eaebbbc6dd99)
![Screenshot (1265)](https://github.com/user-attachments/assets/6862b20a-0b4b-439f-bb24-9b27c70f4f82)
![Screenshot (1266)](https://github.com/user-attachments/assets/edca958d-a984-4956-b41d-d4b75f2ce39b)
![Screenshot (1267)](https://github.com/user-attachments/assets/ed98a761-ccf2-4179-978f-f9e57e1cccf5)
![Screenshot (1268)](https://github.com/user-attachments/assets/caca0623-eaa9-454e-85a1-e38876657d85)


**Actions Taken:**

Accessing the Elasticsearch Web Interface:

Opened a web browser and navigated to the configured IP address of Elasticsearch:

http://<your_ip_address>:5601

Logged into the Kibana interface.

Monitoring and Configuring Data:

Navigated to the Discover section in Kibana to analyze the ingested data from Auditbeat, Winlogbeat, and other configured Beats.

Created visualizations and dashboards using the following:

Winlogbeat Logs: Monitored Windows Event Logs, including system, application, and security events.

Auditbeat Data: Analyzed audit data, such as file integrity monitoring and user activity logs.

Kibana Dashboards: Created custom dashboards to display key metrics, such as user logins, system modifications, and network activity.

Configured data filters and queries to refine the insights for monitoring specific events related to the project.

**Purpose:** This step confirms that the Elastic Stack components are successfully integrated and operational. It allows for:

Real-time monitoring of system and security events.

Visualization of ingested data for better analysis and decision-making.

**Why This Step Matters:**
The ability to monitor data in Elasticsearch and create visualizations in Kibana completes the project. It validates the functionality of the Elastic Stack setup, showcasing its capability to collect, process, and present data for effective monitoring and security analysis.

--------


## Conclusion:

This project provided a comprehensive understanding of setting up a Security Information and Event Management (SIEM) solution using the Elastic Stack. Throughout the process, I learned how to configure and integrate components such as Elasticsearch, Kibana, Auditbeat, and Winlogbeat, ensuring seamless communication across different systems.

**Key takeaways include:**

- The importance of proper configuration and troubleshooting for service connectivity.

- The ability to monitor real-time security events and analyze them using custom Kibana dashboards.
- Gaining hands-on experience in managing and securing a SIEM environment.
  
This project not only strengthened my technical skills but also highlighted the value of open-source tools in building robust security solutions for organizations. It serves as a strong foundation for future explorations in cybersecurity and system monitoring.
