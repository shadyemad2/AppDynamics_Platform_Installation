# AppDynamics On-Prem Installation

## Description

This lab guides you through the process of installing and configuring the AppDynamics platform components for an on-premise deployment.  The architecture diagram below shows these components with their communication paths and payloads.

<img width="2163" height="1515" alt="image" src="https://github.com/user-attachments/assets/e4370117-b906-4fa4-a42d-14c3baf48d80" />

In this Lab for simplicity we will install all AppDynamics Components (Enterprise Console, Controller, Events Service, End User Monitoring Server) on a Standalone Virtual Machine.

In this Lab, you will learn to:
1. Install the Enterprise Console (aka Platform Admin) tool
2. Install and configure the AppDynamics Controller software
3. Install and configure the Events Service
4. Install and configure the End User Monitoring (EUM) Server

The software versions used in the lab are the most recent generally available at the time the lab was created.  As subsequent versions continue to be released, expect slight variations to command line output and screen shot graphics.  Though normally inconsequential, it is recommended to note these differences for reference in troubleshooting should they become significant.

## Setup

### Step 1: Prepare Virtual Machine Specs.

- **Operating System:** Linux or Windows  (This Guide  is based on Linux OS, CentOs7 Distribution)
- **Linux Distribution:** Any from the below list (This Guide is based on CentOS7)
- **CPU:** 4 Cores (Minimum)
- **Memory:** 16 GB RAM (Minimum), 32 GB RAM (Recommended)
- **Storage:** 100 GB (Minimum)


### Step 2: Enterprise Console Requirements

The Enterprise Console can run on the same host as the Controller and the embedded Events Service. If this is the case, the machine you choose to run the Enterprise Console must meet the requirements for all the components that run on that machine.
1. we will need to install Java
        <pre><code>
        yum install java-17-openjdk java-17-openjdk-devel -y
        <pre><code>
        
3. We will need to install these required libraries
        <pre><code>
        yum install libaio
        yum install numactl
        yum install tzdata
        yum install ncurses-libs-5.x
        </code></pre>
        Note: the above required libraries are based on Red Hat and CentOS, for other Distros, please refer to [Enterprise Console Requirements]
        (https://docs.appdynamics.com/appd/onprem/24.x/24.4/en/enterprise-console/enterprise-console-requirements).

2.  AppDynamics requires the following hard and soft per-user limits in Linux:
    * Open file descriptor limit (nofile): 65535
    * Process limit (nproc): 65535

    Following the steps in [Configure User Limits in Linux Controllers]
    (https://docs.appdynamics.com/appd/onprem/24.x/24.4/en/plan-your-deployment/physical-machine-controller-deployment-guide/prepare-the-controller-host/prepare-linux-for-the-controller):
        <pre><code>
        vi /etc/security/limits.d/appdynamics.conf
        </code></pre>
        And add the following at the end of the file:
        <pre><code>
        * hard nofile 65535
        * soft nofile 65535
        * hard nproc 65535
        * soft nproc 65535
        </code></pre>
        Then logout from the terminal and login again.

### Step 3: Install Enterprise Console (aka Platform Admin)

In this exercise, you will be setting up the Enterprise Console.  This utility provides a browser-based user interface that allows an AppDynamics administrator to install and manage the Controller and Events Service components of the AppDynamics platform.  A CLI for the Enterprise Console is available, but is outside the scope of this lab.

Reference documentation can be found on the AppDynamics documents site - [Enterprise Console Documentation]
(https://docs.appdynamics.com/appd/onprem/24.x/24.4/en/enterprise-console).

1. Download and copy the Platform Admin Installer Script to the lab host.
   Log into / Register on https://accounts.appdynamics.com to download the "Enterprise Console - 64-bit Linux(sh)"
![EnterpirseConsoleDownload](assets/images/01-EnterpriseConsoleDownload.png)

2. Copy the .sh file to your Host either using SCP on if your Desktop is MAC/Linux or using WINSCP if your Desktop is Windows

3. On the Host, Make the installer script executable:
        <pre><code>
        chmod a+x platform-setup-x64-linux-20.x.x.x.sh
        </code></pre>
<img width="999" height="552" alt="run script on cli" src="https://github.com/user-attachments/assets/093d962c-8e4e-4fcf-a6b1-a00a9fa06737" />

4. Install the Platform Admin software:
        <pre><code>
        sudo su
        ./platform-setup-x64-linux-20.x.x.x.sh
        </code></pre>
    step 1
    <img width="1967" height="908" alt="enterprise 1" src="https://github.com/user-attachments/assets/13f9d154-058d-4555-b0e3-e879fd20fbe2" />
    step 2
    <img width="997" height="912" alt="enterprise 2" src="https://github.com/user-attachments/assets/95c1d9b3-a059-4669-957d-6ce1911e5504" />
    step 3
    <img width="1000" height="905" alt="enterprise 3" src="https://github.com/user-attachments/assets/4503519f-deec-4715-ab45-9da8dade287d" />
    step 4
    <img width="996" height="896" alt="enterprise 4" src="https://github.com/user-attachments/assets/bdf6f38d-8f30-442a-92ea-0fb8541f051c" />
    step 5
    <img width="1002" height="906" alt="enterprise 5" src="https://github.com/user-attachments/assets/f38a5f6e-cb36-4ddf-8a32-3dac0fa0b97f" />
    step 6
    <img width="998" height="908" alt="enterprise 6" src="https://github.com/user-attachments/assets/162a8adc-607a-4b08-8ca5-cb9512c18853" />
    step 7
    <img width="998" height="904" alt="enterprise 7" src="https://github.com/user-attachments/assets/eb7f73fe-33b9-4230-8777-58cc21f8ba4b" />
    step 8
    <img width="992" height="901" alt="enterprise 8" src="https://github.com/user-attachments/assets/97a76d6d-2ec0-40b3-beb6-ba88dd7afa18" />

    After a few minutes, you should see output similar to that shown below...
        <pre><code>
        Setup has finished installing AppDynamics Enterprise Console on your computer.
        To install and manage your AppDynamics Platform, use the Enterprise Console
        CLI from /opt/appdynamics/platform/platform-admin/bin directory.
        Finishing installation ...
        </code></pre>

6. To confirm the Enterprise Console is functioning properly, verify you can connect to its URL in a web browser and authenticate using the information specified in the installation (Username: admin, Password: shadyemad)
        <pre><code>
        http://[your-ip-address]:[EnterpriseConsolePort]
        </code></pre>
        <img src="https://github.com/sherifadel90/AppDynamicsPlatformInstallation/blob/master/assets/images/02-EnterpriseConsoleLogin.png" width="800">

### Step 4: Install AppDynamics Controller & Events Service

In the Enterprise Console browser interface.  You will use the Enterprise console to install the Controller and Events Service components of the AppDynamics platform.
The Controller provides the main AppDynamics GUI which is backed by a MySQL database, while the Events Service is the facility that stores unstructured data generated by various AppDynamics functions.

Installation of both components can be accomplished using the CLI, but that procedure is beyond the scope of this lab.
Also note that the lab focuses on installation and configuration procedures in a learning environment, but you should be aware that additional steps may be necessary to address security, performance, availability, and scalability considerations in a production deployment.

Reference documentation can be found on the AppDynamics web site - [Controller Documentation](https://docs.appdynamics.com/appd/onprem/24.x/24.4/en/controller-deployment) and [Events Service Documentation](https://docs.appdynamics.com/appd/onprem/24.x/24.4/en/events-service-deployment).

1. From the Enterprise Console UI, select the Install tab and click the Express Install type.
<img width="1516" height="810" alt="install platform 3" src="https://github.com/user-attachments/assets/82d9404f-5467-4395-89b6-e68865058620" />

2. In the **Name the Platform** section, Provide a name of your choice for the platform and confirm the default **Installation Path** value is a subdirectory of the installation directory we specified earlier.
<img width="1516" height="813" alt="install platform 1" src="https://github.com/user-attachments/assets/5c192976-0309-4650-9393-d362df719b02" />

3. In the **Add a Host** section, choose Use **Enterprise Console Host**.
<img width="1515" height="814" alt="install platform 2" src="https://github.com/user-attachments/assets/a658bc35-0037-41f2-b4d1-453b66376b9b" />


4. In the **Install the Controller** section:
        - Select the **Demo profile**
        - Set the Controller Admin Username to **admin**
        - Set the Controller **Admin Password** to **welcome1**
        - Set the Controller Controller Root User Password to **welcome1**
        - Set Database Root Password to **welcome1**
        <img width="1513" height="811" alt="install platform 4 1" src="https://github.com/user-attachments/assets/fe6edb88-4d13-4117-8c78-49f058a41256" />


5. Click the **Submit** button, The installation will begin immediately but it may take a few minutes before the jobs appear and begin reporting their progress on the page.

6. After the jobs have completed successfully (up to 15 minutes), click the Controller and Events Service sections to make sure each shows a health state of Normal.
<img width="1513" height="815" alt="running jobs" src="https://github.com/user-attachments/assets/21d4afcb-a434-4bd0-9ec5-fcd098a85a5a" />

7. To confirm the Controller is running properly, verify you can connect to its URL in a web browser and authenticate using the information specified in **Step 4** (Username: admin, Password: welcome1)
        <pre><code>
        http://[your-ip-address]:8090
        </code></pre>
        <img width="3360" height="1944" alt="09-ControllerLogin" src="https://github.com/user-attachments/assets/2f4667d2-4f2e-4321-ad15-e345acba76c0" />


-----

## Notes

1. In case of a Server restart, you can start back all the serivces via the CLI:
        <pre><code>
        cd /opt/appdynamics/platform/platform-admin
        bin/platform-admin.sh start-platform-admin
        bin/platform-admin.sh start-controller-db
        bin/platform-admin.sh start-controller-appserver
        bin/platform-admin.sh submit-job --platform-name AppDPlatform --service events-service --job start
        cd /opt/appdynamics/eum/eum-processor/
        bin/eum.sh start
        </code></pre>

## Author
 Shady Emad



