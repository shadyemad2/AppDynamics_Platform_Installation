# AppDynamics On-Prem Installation

## Description
This lab guides you through the process of installing and configuring the AppDynamics platform components for an on-premise deployment. The architecture diagram below shows these components with their communication paths and payloads.

<img width="2163" height="1515" alt="image" src="https://github.com/user-attachments/assets/e4370117-b906-4fa4-a42d-14c3baf48d80" />

In this Lab for simplicity we will install all AppDynamics Components (Enterprise Console, Controller, Events Service, End User Monitoring Server) on a Standalone Virtual Machine.

In this Lab, you will learn to:
1. Install the Enterprise Console (aka Platform Admin) tool  
2. Install and configure the AppDynamics Controller software  
3. Install and configure the Events Service  
4. Install and configure the End User Monitoring (EUM) Server  

The software versions used in the lab are the most recent generally available at the time the lab was created. As subsequent versions continue to be released, expect slight variations to command line output and screen shot graphics. Though normally inconsequential, it is recommended to note these differences for reference in troubleshooting should they become significant.

---

## Setup

### Step 1: Prepare Virtual Machine Specs
- **Operating System:** Linux or Windows (This Guide is based on Linux OS, CentOs7 Distribution)
- **Linux Distribution:** Any from the below list (This Guide is based on CentOS7)
- **CPU:** 4 Cores (Minimum)
- **Memory:** 16 GB RAM (Minimum), 32 GB RAM (Recommended)
- **Storage:** 100 GB (Minimum)

---

### Step 2: Enterprise Console Requirements
The Enterprise Console can run on the same host as the Controller and the embedded Events Service.  
If this is the case, the machine you choose to run the Enterprise Console must meet the requirements for all the components that run on that machine.

1. Install Java:
   ```bash
   yum install java-17-openjdk java-17-openjdk-devel -y
   ```

2. Install the required libraries:
   ```bash
   yum install libaio -y
   yum install numactl -y
   yum install tzdata -y
   yum install ncurses-libs-5.x -y
   ```

   > Note: The above required libraries are based on Red Hat and CentOS, for other distros refer to [Enterprise Console Requirements](https://docs.appdynamics.com/appd/onprem/24.x/24.4/en/enterprise-console/enterprise-console-requirements).

3. AppDynamics requires the following hard and soft per-user limits in Linux:

   - Open file descriptor limit (nofile): 65535  
   - Process limit (nproc): 65535  

   Configure limits:
   ```bash
   vi /etc/security/limits.d/appdynamics.conf
   ```

   Add the following:
   ```
   * hard nofile 65535
   * soft nofile 65535
   * hard nproc 65535
   * soft nproc 65535
   ```

   Then logout and login again.

---

### Step 3: Install Enterprise Console (aka Platform Admin)
The Enterprise Console provides a browser-based UI to install and manage the Controller and Events Service.  
A CLI version exists but is out of scope for this lab.  
Reference: [Enterprise Console Documentation](https://docs.appdynamics.com/appd/onprem/24.x/24.4/en/enterprise-console)

1. Download and copy the Platform Admin Installer Script from your AppDynamics account.  
   Log into https://accounts.appdynamics.com and download **Enterprise Console - 64-bit Linux (sh)**.

   ![EnterpirseConsoleDownload](assets/images/01-EnterpriseConsoleDownload.png)

2. Copy the `.sh` file to your host using `scp` (Linux/Mac) or `WinSCP` (Windows).

3. Make the installer executable:
   ```bash
   chmod a+x platform-setup-x64-linux-20.x.x.x.sh
   ```

4. Install the Platform Admin software:
   ```bash
   sudo su
   ./platform-setup-x64-linux-20.x.x.x.sh
   ```

   step 1  
   <img width="1967" height="908" alt="enterprise 1" src="https://github.com/user-attachments/assets/13f9d154-058d-4555-b0e3-e879fd20fbe2" />  
   step 2  
   <img width="997" height="912" alt="enterprise 2" src="https://github.com/user-attachments/assets/95c1d9b3-a059-4669-957d-6ce1911e5504" />  
   step 3  
   <img width="1000" height="905" alt="enterprise 3" src="https://github.com/user-attachments/assets/4503519f-deec-4715-ab45-9da8dade287d" />   
   step 4 
   <img width="998" height="908" alt="enterprise 6" src="https://github.com/user-attachments/assets/162a8adc-607a-4b08-8ca5-cb9512c18853" />  
   step 5 
   <img width="998" height="904" alt="enterprise 7" src="https://github.com/user-attachments/assets/eb7f73fe-33b9-4230-8777-58cc21f8ba4b" />  
   step 6  
   <img width="992" height="901" alt="enterprise 8" src="https://github.com/user-attachments/assets/97a76d6d-2ec0-40b3-beb6-ba88dd7afa18" />  

   After a few minutes, you should see:
   ```bash
   Setup has finished installing AppDynamics Enterprise Console on your computer.
   To install and manage your AppDynamics Platform, use the Enterprise Console CLI from
   /opt/appdynamics/platform/platform-admin/bin directory.
   Finishing installation ...
   ```

5. Verify Enterprise Console via web browser:
   ```
   http://[your-ip-address]:[EnterpriseConsolePort] 8091
   ```
   Login:
   - Username: admin  
   - Password: shadyemad  

   <img width="3360" height="1952" alt="02-EnterpriseConsoleLogin" src="https://github.com/user-attachments/assets/d7670394-c048-47fe-8e54-ad05947850b2" />


---

### Step 4: Install AppDynamics Controller & Events Service
From the Enterprise Console UI, you will install both the Controller and Events Service.

1. From the **Install** tab, click **Express Install**.  
   <img width="1516" height="810" alt="install platform 3" src="https://github.com/user-attachments/assets/82d9404f-5467-4395-89b6-e68865058620" />

2. In the **Name the Platform** section, provide a name and confirm the **Installation Path**.  
   <img width="1516" height="813" alt="install platform 1" src="https://github.com/user-attachments/assets/5c192976-0309-4650-9393-d362df719b02" />

3. In the **Add a Host** section, choose **Use Enterprise Console Host**.  
   <img width="1515" height="814" alt="install platform 2" src="https://github.com/user-attachments/assets/a658bc35-0037-41f2-b4d1-453b66376b9b" />

4. In the **Install the Controller** section:
   - Select the **Demo profile**
   - Set **Controller Admin Username** = `admin`
   - Set **Controller Admin Password** = `welcome1`
   - Set **Controller Root User Password** = `welcome1`
   - Set **Database Root Password** = `welcome1`
   <img width="1513" height="811" alt="install platform 4 1" src="https://github.com/user-attachments/assets/fe6edb88-4d13-4117-8c78-49f058a41256" />

5. Click **Submit**, and wait for the jobs to complete (may take up to 15 minutes).

6. When done, ensure both Controller and Events Service show a health state of **Normal**.  
   <img width="1513" height="815" alt="running jobs" src="https://github.com/user-attachments/assets/21d4afcb-a434-4bd0-9ec5-fcd098a85a5a" />

7. Verify Controller via browser:
   ```
   http://[your-ip-address]:8090
   ```
   Login:
   - Username: admin  
   - Password: welcome1  

   <img width="3360" height="1944" alt="09-ControllerLogin" src="https://github.com/user-attachments/assets/2f4667d2-4f2e-4321-ad15-e345acba76c0" />

---

## Notes
In case of a server restart, you can start all services via CLI:
```bash
cd /opt/appdynamics/platform/platform-admin
bin/platform-admin.sh start-platform-admin
bin/platform-admin.sh start-controller-db
bin/platform-admin.sh start-controller-appserver
bin/platform-admin.sh submit-job --platform-name AppDPlatform --service events-service --job start

cd /opt/appdynamics/eum/eum-processor/
bin/eum.sh start
```

---

## Author
**Shady Emad**
