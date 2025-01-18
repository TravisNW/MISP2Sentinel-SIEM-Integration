<h1>SIEM Setup (MISP to Sentinel Integration)</h1>

<h2>Brief Objective</h2>
I implemented an SIEM solution on Microsoft Azure by integrating MISP with Azure Sentinel using the MISP2Sentinel data connector. The project focused on aggregating threat intelligence feeds, including indicators of compromise and related contextual data from MISP, to provide hands-on experience in how such systems function, what types of information they process, and how they support threat detection and incident response.
<br />


<h2>Skills Learned</h2>

- <b>SIEM Implementation</b> 
- <b>Threat Intelligence Integration</b>
- <b>Cloud Security Solutions</b>

<h2>Tools Used</h2>

- <b>Azure</b>
- <b>VS Code</b> 
- <b>MISP</b>
- <b>Docker</b>

<h2>Steps</h2>

In Azure, I provisioned a virtual machine using the Ubuntu Server 24.04 LTS image, leaving the remaining configurations set to their default values, with the resource group located in East US. Once the VM deployment was complete, I established an SSH connection to the instance via the Azure CLI. Upon accessing the Cloud Shell, I proceeded to configure the Docker Engine by following the installation instructions provided at https://docs.docker.com/engine/install/ubuntu/. After successfully installing the engine, I verified the installation by printing "hello-world" onto the CLI. <br/>
![1](https://github.com/user-attachments/assets/72f6a8f1-8362-4b21-b2cf-6f0011bc4bcd)
![2](https://github.com/user-attachments/assets/1a3d7572-b889-4970-bdd4-d3b51ecf3770)
<br />
<br />
I navigated to the MISP image repository on GitHub and copied the URL: https://github.com/MISP/misp-docker. Using the git clone command, I cloned the repository to my Ubuntu virtual machine. After changing the current directory to misp-docker, I followed the instructions provided in the repository's README. The first step involved copying template.env to .env. I then customised the .env file by editing it with the nano editor. In the BASE_URL field, I specified the public IP address of the virtual machine, which was https://52.186.169.243, and saved the changes to the file. I then proceeded with the remaining steps outlined in the README.  <br/>
![3](https://github.com/user-attachments/assets/15ae7cb1-7be0-4929-9f43-6d7bf5246ba8)
<br />
<br />
To allow a connection from my physical operating system or any host, I modified the network settings of the Ubuntu VM and created an inbound port rule to open port 443. In the .env file, I specified the URL that I would use to access the MISP instance. After navigating to the URL, I proceeded through the "Connection is not private" warning and accessed the MISP instance. I then logged in using the default credentials provided in the GitHub repository. Once inside the MISP GUI, I accessed the admin profile and changed the password, as retaining default credentials on a publicly accessible server presents a significant security risk. <br/>
![4](https://github.com/user-attachments/assets/12cccab2-e4cc-4715-a6fe-e496e326a854)
![5](https://github.com/user-attachments/assets/1d9d4bca-195c-4376-bdc3-7a2aa96f3ecb)
![6](https://github.com/user-attachments/assets/92b9f2d8-ab8b-4d09-a9c5-a5529ffb8be0)
<br />
<br />
To import the feeds into the MISP instance, I retrieved the complete JSON configuration from the following link: https://github.com/MISP/MISP/blob/2.4/app/files/feed-metadata/defaults.json. After copying the contents, I selected all relevant feeds within the MISP GUI and enabled them, ensuring that Sentinel could access and pull resources containing indicators from MISP.  <br/>
![8](https://github.com/user-attachments/assets/e3e1a131-5e62-4787-919e-5f8fc1a7fc1d)
![9](https://github.com/user-attachments/assets/9ea40ea1-9b74-4ad2-be85-4f2411119c12)
<br />
<br />
In Microsoft Sentinel, after setting up Sentinel, making sure to assign the resource group that contains all the resources in use, I navigated to the Content Hub under the Data Connectors subsection and installed the preconfigured data connector MISP2Sentinel. From the GitHub repository at https://github.com/cudeso/misp2sentinel, I followed the instructions provided for integrating MISP with Microsoft Sentinel using the Upload Indicators API methodology.  <br/>
<br />
<br />
Next, I went to the Azure portal's App Registrations section, created a new registration by naming it and clicking Register, leaving the default settings unchanged. After documenting the Client ID, Object ID, and Tenant ID, I proceeded to the Certificates & Secrets section where I created a new client secret. I made sure to document the value of the secret, as it would not be displayed again.  <br/>
![10](https://github.com/user-attachments/assets/110bce99-0d69-4f2b-ace5-887cbaa2b287)
![11](https://github.com/user-attachments/assets/8f72dd35-7ec9-45cb-ad15-68cbfac16b24)
![12](https://github.com/user-attachments/assets/a8b43f07-dfa7-40e9-a03d-163ef113493d)
<br />
<br />
I navigated to my Log Analytics workspaces and selected the workspace where I had created the Sentinel instance. To grant the app access to Sentinel, I went to the Access Control (IAM) section and clicked Add to assign a role. I selected the Microsoft Sentinel Contributor role and clicked Next. In the Select Members section, I searched for the registered application, selected it, and then clicked Review + Assign to complete the process.

In the Key Vaults section, I created a new key vault by following the guided steps. Subsequently, I needed to obtain an API key from MISP to enable the function app to connect to the MISP instance. This connection would allow the function app to retrieve indicators from MISP and send them to Sentinel. After generating the authentication key in MISP, I documented the key value, as it would not be accessible again in the future. <br/>
![13](https://github.com/user-attachments/assets/2656992f-55cb-4578-965a-1b0819739d05)
![14](https://github.com/user-attachments/assets/9d87ae0d-25d8-41f7-8ad6-c4b75721c1bd)
<br />
<br />
I went to Create Function App and selected the App Service hosting plan. I provided a name for the app, chose the resource group I was using for this project, and set the runtime stack to Python (version 3.11). I also selected the East US region and left all other settings as default before clicking Review + Create to proceed. <br />
![15](https://github.com/user-attachments/assets/5bd9365f-9ec6-486e-8081-ce232cbdff3d)
<br />
<br />
To deploy the function app, I downloaded the ZIP file from the MISP2Sentinel repository and opened the extracted folder in Visual Studio Code (VSCode). I ensured that the Azure Resources and Azure Functions extensions were installed in VSCode. <br />
![16](https://github.com/user-attachments/assets/afbe8985-3889-440a-b656-db52df07f536)
<br />
<br />
Afterward, I signed into my Azure account by clicking the Azure icon in the left-side column of VSCode. To deploy the code, I right-clicked on AzureFunction and selected Deploy to Function App…. I then chose the function app I had previously created in Azure and selected Skip Virtual Environment to complete the deployment process.
![17](https://github.com/user-attachments/assets/eb62bf9e-5a64-4f93-a57c-f3530f15c1b9)
<br />
<br />
Once the deployment succeeded, I navigated to the Function App > Overview > Functions and verified that the function was present. Upon clicking into the function, I confirmed that the files sent to the function app were correctly listed. <br />
![18](https://github.com/user-attachments/assets/56761294-68ab-49ca-b004-9f5075165a94)
<br />
<br />
Next, I configured the environment variables under Settings > Configuration. These environment variables are essential for the function's operation. The code in one of the files queries for several values, including tenant_id, client_id, client_secret, workspace_id, and other variables as specified in the MISP2Sentinel GitHub repository instructions (such as timerTriggerSchedule, mispurl, and AzureFunctionsJobHost_functionTimeout). I carefully added these values, ensuring that the variable names matched exactly as specified in the config.py file. <br />
![19](https://github.com/user-attachments/assets/3611a6b7-fc10-41a2-b807-9c8796819c68)
<br />
<br />
After applying the environment variables, I navigated to the Overview section of the function app and selected Invocations to verify that everything was working. However, rather than waiting for the results, I opted to test the function immediately. Since I was on a personal Pay-As-You-Go consumption plan (having exceeded my Azure Free Trial), I used the Code + Test to trigger a Run. This action ran the main function responsible for sending the indicators to the Sentinel instance. The expected output should show an HTTP response code of ‘202 Accepted’ in the Output section. <br />
![20](https://github.com/user-attachments/assets/57e2f40c-c71a-4fb2-ab94-bca3ce8dd324)
<br />
<br />
To verify that the function was operating correctly, I checked the logs. The values input into the environment variables were displayed (though the Workspace ID was added later, as it was initially missing). <br />
![21](https://github.com/user-attachments/assets/25ea788d-f547-4e14-800a-2a49ba935f2f)
<br />
<br />
Given that the system was functioning as expected, I decided not to wait for the massive output of feeds to load and proceeded to delete everything once the SIEM was confirmed as operational. To further confirm the integration, I checked the Data Connector page of MISP2Sentinel, where the status was listed as Connected and the ThreatIntelligenceIndicator table was present. This table can now be used to create alerts and query indicators as intended. <br />
![23](https://github.com/user-attachments/assets/d8ce27e7-4840-4c53-b3c1-0da7f59bbd1d)
![22](https://github.com/user-attachments/assets/e483492e-5d2a-424f-9ee6-1785ccbf710e)
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
