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

   <img width="1877" height="861" alt="5" src="https://github.com/user-attachments/assets/d0157575-7017-4abd-a4ab-5c7b0a018820" />
   the detailes of the smart agent:
   <img width="1066" height="637" alt="6" src="https://github.com/user-attachments/assets/cbe18472-d6d8-4abd-84bf-6e9cd403e864" />


---

### Auto-Attach Agents to Applications
Smart Agent provides the auto-attach feature to auto instrument a Java application with Java Agent or a NodeJS application with NodeJS Agent. This feature is used to detect and start the supported Splunk AppDynamics agents without modifying the start configuration of the applications. You can use Smart Agent Command Line Utility for additional configuration.

1. Start smart agent and check that is running  
   <img width="561" height="189" alt="7" src="https://github.com/user-attachments/assets/8f352eae-b11e-406f-add0-973061e023bb" />

2. Customize Auto-Attach Configuration
   You can overwrite the default ld_preload.json (attaches the agent to all supported Java frameworks and NodeJS applications) and add your own with your updated     rules. The default location for ld_preload.json is /etc/opt/appdynamics/ld_preload.json.
   
   <img width="1222" height="259" alt="8" src="https://github.com/user-attachments/assets/ac16ec4e-7ad3-47fb-a0dd-24f2fa62eba8" />
   <img width="985" height="754" alt="9" src="https://github.com/user-attachments/assets/b2e316eb-1533-4130-becc-160ef14b20d3" />

   > **Note: What is `ld_preload.json`**
>
> - A configuration file for **AppDynamics Smart Agent Auto-Attach**.  
> - It tells Smart Agent which **Java** or **NodeJS** processes to attach agents to automatically, without modifying JVM or NodeJS startup commands.
>
> **Structure:**
> ```json
> {
>   "java": [ ... ],
>   "nodejs": [ ... ]
> }
> ```
> - `"java"`: rules for Java processes.  
> - `"nodejs"`: rules for NodeJS processes.  
>
> Each rule can contain:
>
> | Field       | Description |
> |------------|-------------|
> | `pattern`  | Regex to match process name. Example: `.*MyApp.*` |
> | `agentPath` | Path to the agent JAR (Java) or JS (NodeJS). Example: `/opt/appdynamics/smartagent/agents/java/java-agent/javaagent.jar` |
> | `properties` (optional, Java only) | Defines `appName`, `tierName`, `nodeName` to show in Controller |
>
> **How it works:**
> 1. Smart Agent reads the file at **startup**.  
> 2. Matches running processes using `"pattern"`.  



4. In the **Add a Host** section, choose **Use Enterprise Console Host**.  
   <img width="1515" height="814" alt="install platform 2" src="https://github.com/user-attachments/assets/a658bc35-0037-41f2-b4d1-453b66376b9b" />

5. In the **Install the Controller** section:
   - Select the **Demo profile**
   - Set **Controller Admin Username** = `admin`
   - Set **Controller Admin Password** = `welcome1`
   - Set **Controller Root User Password** = `welcome1`
   - Set **Database Root Password** = `welcome1`
   <img width="1513" height="811" alt="install platform 4 1" src="https://github.com/user-attachments/assets/fe6edb88-4d13-4117-8c78-49f058a41256" />

6. Click **Submit**, and wait for the jobs to complete (may take up to 15 minutes).

7. When done, ensure both Controller and Events Service show a health state of **Normal**.  
   <img width="1513" height="815" alt="running jobs" src="https://github.com/user-attachments/assets/21d4afcb-a434-4bd0-9ec5-fcd098a85a5a" />

8. Verify Controller via browser:
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



