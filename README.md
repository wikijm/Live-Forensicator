<h1 align="center">📝 Forensicator 📝</h1>
<h3 align="center">POWERSHELL SCRIPT TO AID LIVE FORENSICS & INCIDENCE RESPONSE</h3>
                                               
```bash


___________                                .__               __                
\_   _____/__________   ____   ____   _____|__| ____ _____ _/  |_  ___________ 
 |    __)/  _ \_  __ \_/ __ \ /    \ /  ___/  |/ ___\\__  \\   __\/  _ \_  __ \
 |     \(  <_> )  | \/\  ___/|   |  \\___ \|  \  \___ / __ \|  | (  <_> )  | \/
 \___  / \____/|__|    \___  >___|  /____  >__|\___  >____  /__|  \____/|__|   
     \/                    \/     \/     \/        \/     \/                    

                                                                          v2.0



```


# ABOUT

Live Forensicator is part of the Black Widow Toolbox, its aim is to assist Forensic Investigators and Incidence responders in carrying out a quick live forensic investigation.
<p>It achieves this by gathering different system information for further review for anomalous behaviour or unexpected data entry, it also looks out for unusual files or activities and points it out to the investigator.</p>
<p>It is paramount to note that this script has no inbuilt intelligence its left for the investigator to analyse the output and decide on a conclusion or decide on carrying out more deeper investigation.</p>

## Optional Dependencies

This script is written in powershell for use on windows PCs and Servers. 
<p>For additional features it depends on external binaries.</p>
<p>It has a supporting file WINPMEM for taking RAM dumps https://github.com/Velocidex/WinPmem </p>
<p>It also depends on Nirsoft's BrowserHistoryView for exporting browser history http://www.nirsoft.net/utils/browsing_history_view.html </p>
<p>This script is expected to work out of the box. </p>

```bash
winpmem_mini_x64_rc2.exe | BrowsingHistoryView64.exe | BrowsingHistoryView86.exe | etl2pcapng64.exe | etl2pcapng86.exe
```

## Usage

```python
# copy the files to the computer
git clone https://github.com/Johnng007/Live-Forensicator.git

# Execution
.\Forensicator.ps1 <parameters>

```

## Examples

```python
# Basic
.\Forensicator.ps1

# Check your Version
.\Forensicator.ps1 -Version

# Check for Updates
.\Forensicator.ps1 -Update

# Decrypt An Encrypted Artifact
.\Forensicator.ps1 -DECRYPT DECRYPT

# Extract Event Logs alongside Basic Usage
.\Forensicator.ps1 -EVTX EVTX

#Grab weblogs IIS & Apache
.\Forensicator.ps1 -WEBLOGS WEBLOGS

#Run Network Tracing & Capture PCAPNG for 120 secounds
.\Forensicator.ps1 -PCAP PCAP

# Extract RAM Dump alongside Basic Usage
.\Forensicator.ps1 -RAM RAM

# Check for log4j with the JNDILookup.class
.\Forensicator.ps1 -log4j log4j

# Encrypt Artifact after collecting it
.\Forensicator.ps1 -ENCRYPTED ENCRYPTED

# Yes of course you can do all
.\Forensicator.ps1 -EVTX EVTX -RAM RAM -log4j log4j -PCAP PCAP -WEBLOGS WEBLOGS

# For Unattended Mode on Basic Usage
.\Forensicator.ps1 -OPERATOR "Ebuka John" -CASE 01123 -TITLE "Ransomware Infected Laptop" -LOCATION Nigeria -DEVICE AZUZ

# You can use unattended mode for each of the other parameters
.\Forensicator.ps1 -OPERATOR "Ebuka John" -CASE 01123 -TITLE "Ransomware Infected Laptop" -LOCATION Nigeria -DEVICE AZUZ -EVTX EVTX -RAM RAM -log4j log4j

# Check for files that has similar extensions with ransomware encrypted files (can take some time to complete)
.\Forensicator.ps1 -RANSOMWARE RANSOMWARE

# You can compress the Forensicator output immidiately after execution Oneliner
.\Forensicator.ps1 ; Start-Sleep -s 15 ; Compress-Archive -Path "$env:computername" -DestinationPath "C:\inetpub\wwwroot\$env:computername.zip" -Force

```

## Notes
Run the script as an administrator to get value.<br>
The results are outputed in nice looking html files with an index file. <br>
You can find all extracted Artifacts in the script's working directory.
<p>Forensicator Has the ability to Search through all the folders within a system looking for files with similar extensions as well known Ransomwares, Albeit this search takes long but its helpful if the Alert you recieved is related to a Ransomware attack, Use the -RANSOMWARE Parameter to invoke this.</p>
<p>Forensictor now hs the ability to capture network traffic using netsh trace, this is useful when your investigation has to do with asset communicating with known malicious IPs, this way you can parse the pcapng file to wireshark and examine for C&C servers. By Defult i set the capture to take 120secs</p> 
<p>Sometimes it may be paramount to maintain the integrity of the Artifacts, where lawyers may argue that it might have been compromised on transit to your lab.
Forensicator can now encrypt the Artifact with a unique randomely generated key using AES algorithm, you can specify this by using the -ENCRYPTED parameter. You can decrypt it at will anywhere anytime even with another copy of Forensicator, just keep your key safe. This task is performed by the FileCryptography.psm1 file</p>

## What Forensicator Grabs
```bash

   =================================
     USER AND ACCOUNT INFORMATION
   =================================
     1. GETS CURRENT USER.
     2. SYSTEM DETAILS.
     3. USER ACCOUNTS
     4. LOGON SESSIONS
     5. USER PROFILES
     6. ADMINISTRATOR ACCOUNTS
     7. LOCAL GROUPS

   =================================
     SYSTEM INFORMATION
   =================================
     1. INSTALLED PROGRAMS.
     2. INSTALLED PROGRAMS FROM REGISTERY.
     3. ENVIRONMENT VARIABLES
     4. SYSTEM INFORMATION
     5. OPERATING SYSTEM INFORMATION
     6. HOTFIXES
     8. WINDOWS DEFENDER STATUS AND DETAILS

   =================================
     NETWORK INFORMATION
   =================================
     1. NETWORK ADAPTER INFORMATION.
     2. CURRENT IP CONFIGURATION IPV6 IPV4.
     3. CURRENT CONNECTION PROFILES.
     4. ASSOCIATED WIFI NETWORKS AND PASSWORDS.
     5. ARP CACHES
     6. CURRENT TCP CONNECTIONS AND ASSOCIATED PROCESSES
     7. DNS CACHE
     8. CURRENT FIREWALL RULES
     9. ACTIVE SMB SESSIONS (IF ITS A SERVER)
     10. ACTIVE SMB SHARES
     11. IP ROUTES TO NON LOCAL DESTINATIONS
     12. NETWORK ADAPTERS WITH IP ROUTES TO NON LOCAL DESTINATIONS
     13. IP ROUTES WITH INFINITE VALID LIFETIME

   ========================================
     PROCESSES | SCHEDULED TASK | REGISTRY
   ========================================
    1. PROCESSES.
    2. STARTUP PROGRAMS
    3. SCHEDULED TASK
    4. SCHEDULED TASKS AND STATE
    5. SERVICES
    6. PERSISTANCE IN REGISTRY

   =================================
     OTHER CHECKS
   =================================
    1.  LOGICAL DRIVES
    2.  CONNECTED AND DISCONNECTED WEBCAMS
    3.  USB DEVICES
    4.  UPNP DEVICES
    5.  ALL PREVIOUSLY CONNECTED DRIVES
    6.  ALL FILES CREATED IN THE LAST 180 DAYS
    7.  100 DAYS WORTH OF POWERSHELL HISTORY
	8.  POWERSHELL CONSOLE AND VISUAL STUDIO CODE HOST HISTORY
    9.  EXECUTABLES IN DOWNLOADS FOLDER
    10. EXECUTABLES IN APPDATA
    11. EXECUATBLES IN TEMP
    12. EXECUTABLES IN PERFLOGS
    13. EXECUTABLES IN THE DOCUMENTS FOLDER

   =========================================
      ORTHER REPORTS IN THE HTML INDEX FILE
   =========================================
    1.  GROUP POLICY REPORT
    2.  WINPMEM RAM CAPTURE
    3.  LOG4J
    4.  IIS LOGS
    5.  TOMCAT LOGS
    6.  BROWSING HISTORY OF ALL USERS 
    7.  CHECK FOR FILES THAT HAS SIMILAR EXTENSIONS WITH KNOWN RANSOMWARE ENCRYPTED FILES
        NOTE: THIS CHECK CAN TAKE SOME TIME TO COMPLETE DEPENDING ON THE NUMBER OF DRIVES AND AMOUNT OF FILES.
    8.  RUNS NETWORK TRACING USING NETSH TRACE & CONVERTS TO PCAPNG FOR FURTHER ANALYSIS

```

##ChangeLog
```bash

v2.0 25/04/2022
Minor Bug Fixes
Added the possiblity of encrypting the Artifact after acquiring it to maintain integrity.

v1.4 14/04/2022
Added Ability perform network tracing using netsh trace, the subsequent et1 is converted to pcapng
Minor Bug Fixes in Script Update.
Added Weblogs as an option parameter.

v1.3 11/04/2022
Added a feature to check for files that has similar extensions with known ransomware encrypted files.
You can now check for updates within the script.
UI update

v1.2 29/03/2022 
Added unattended Mode Feature
Added Ability to grab browsing history of all users
Minor Bug Fix

v1 28/01/2022
Initial Release

```

## Screenshot
<img src="https://john.ng/wp-content/uploads/2022/04/NEW_FORENSICATOR_IMAGE.png" alt="Forensicator"  /> <br>
## HTML Output
<img src="https://john.ng/wp-content/uploads/2022/04/HTML-VIEW-FORENSICATOR.png" alt="Forensicator"  /> <br>
<br></br>

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change or add.



## License
[MIT](https://mit.com/licenses/mit/)


<h3 align="left">Support:</h3>
<p><a href="https://www.buymeacoffee.com/ebuka"> <img align="left" src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" height="50" width="210" alt="ebuka" /></a></p><br><br>

<h3 align="left">Connect with me:</h3>
<p align="left">
<a href="https://linkedin.com/in/ebuka john onyejegbu" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/linked-in-alt.svg" alt="ebuka john onyejegbu" height="30" width="40" /></a>
</p>

