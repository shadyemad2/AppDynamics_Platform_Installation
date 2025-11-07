# Smart Agent

<img width="590" height="420" alt="image" src="https://github.com/user-attachments/assets/20e991d0-469c-4f32-a1a2-5240aa28a679" />


---

## What Is Smart Agent
 Smart Agent allows you to manage the agent operations (install, upgrade, and rollback) for the supported agents by using the Controller user interface. It also provides smart agent cli option for advanced configurations. 
 You can use the Controller UI to view all the installed agents with the inventory details. The Smart Agent inventory is also displayed along with other agent inventory details.


### The Following Requirements To Use Smart Agent:
-	Controller version >=23.11.0
-	Sufficient agent licenses
-	Required Memory: 10MB -15MB Idle and 500MB during install, upgrade, or rollback
-	Required Disk: 500 MB

---
## Supported Platforms for Agents

| Agent              | Supported Platforms                                  |
|-------------------|-----------------------------------------------------|
| Python            | CentOS 8 and 9<br>Ubuntu 22.04 and 20.04<br>Alpine |
| PHP               | Ubuntu 22.04 and 20.04                              |
| Machine Agent     | CentOS 8 and 9<br>RHEL 8 and 9<br>Ubuntu 22.04 and 20.04<br>Windows |
| Java              | CentOS 8 and 9<br>RHEL 8 and 9<br>Ubuntu 22.04 and 20.04<br>Windows |
| Apache Web Server | CentOS 8 and 9<br>RHEL 8 and 9<br>Ubuntu 22.04 and 20.04 |


---

### Install Smart Agent

1. Download and Unzip the the appdsmartagent_<version>.tar.gz file 
   <img width="1920" height="791" alt="1" src="https://github.com/user-attachments/assets/b2c95140-6dd2-43e5-953a-cf99d56e778c" />
   <img width="1240" height="209" alt="2" src="https://github.com/user-attachments/assets/a314db12-a7e4-4611-b962-4b2a7dc66cde" />


2. Configure Smart Agent.
   Edit /opt/appdynamics/appdsmartagent/config.ini and add ControllerURL, AccountName, AccountAccessKey, ControllerPort information
   <img width="1380" height="716" alt="3" src="https://github.com/user-attachments/assets/2cf3e400-f2f4-43d7-8dcc-513c6ad1742e" />

3. Start the Smart Agent.
   ```bash
   ./smartagent
   ```
   <img width="1515" height="681" alt="4" src="https://github.com/user-attachments/assets/7e40f0f1-2d7f-4521-b0aa-eaacf3e6a7ce" />


4. From Controller UI, navigate to Home > Agent Management > Manage Agents to start managing your agents

   <img width="952" height="438" alt="image" src="https://github.com/user-attachments/assets/021f8084-1321-4554-9d92-7fc1736da6f5" />
   the detailes of the smart agent:
   <img width="542" height="339" alt="image" src="https://github.com/user-attachments/assets/bcc19725-f804-479d-b16c-6f12fd87d97d" />

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


