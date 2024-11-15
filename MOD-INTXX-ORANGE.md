# Your Lab Guide is Here!

## Click **Next** to at the Bottom of the Page to Continue

===

@lab.Title

## Lab Contents

- [Lab Overview](#lab-overview)

- [Lab 1: Backing Up and Restoring BIG-IP Next Central Manager](#lab-1-backing-up-and-restoring-big-ip-next-central-manager)

- [Lab 2: Backing Up and Restoring a Single BIG-IP Next Instance](#lab-2-backing-up-and-restoring-a-single-big-ip-next-instance)

- [Lab 3: Creating a QkView and Uploading it to iHealth](#lab-3-creating-a-qkview-and-uploading-it-to-ihealth)

- [Lab 4: Troubleshooting with the Debug Sidecar](#lab-4-troubleshooting-with-the-debug-sidecar)

## Lab Credentials and IP Address List

| External IP  | Internal IP     | Management IP  | Name          | Credentials / Comments |
|--------------|-----------------|----------------|---------------|------------------------|
| 10.10.0.0/16 | 172.16.0.0/16   | 192.168.0.0/16 |               | *Lab Networks*         |
| 10.10.0.254  |                 | 192.168.0.254  |               | *Default Routes*       |
| 10.10.1.30   |                 | 192.168.1.30   | jump          | **student / student**  |
| 10.10.1.31   | 172.16.1.31     | 192.168.1.31   | bigip1        | **admin / F5trn001!**  |
|              |                 | 192.168.1.51   | cm1           | **admin / F5trn001!**  |
| 10.10.1.101  |                 |                |               | *Virtual Server*       |
|              | 172.16.20.161-3 |                |               | *Backend Servers*      |

## Lab Topology

!IMAGE [grtdch3v.jpg](instructions261136/grtdch3v.jpg)

===

# Lab Overview

[Return to Lab Contents](#lab-contents)

===

# Lab 1: Backing Up and Restoring BIG-IP Next Central Manager

[Return to Lab Contents](#lab-contents)

## Requirements

Before you begin you need:

* A running BIG-IP Next Central Manager
* A running BIG-IP Next VE Instance
* An account on MyF5 and a BIG-IP Next JSON Web Token (JWT) to license your BIG-IP instance

## Scenario

Your BIG-IP Next Central Manager has the configuration for all your applications running on all of your BIG-IP Next instances. You have not yet configured a BIG-IP Next Central Manager High Availability cluster, so if anything happens to this Central Manager, you will have a lot of work to reconfigure all of the applications currently running. You have decided to start backing up your Central Manager to mitigate a future disaster.

## Objectives

At the end of this lab you will be able to:

* Backup and restore your BIG-IP Next Central Manager

## Lab

1. Log into the **jump** box using the credentials **student** / <code spellcheck="false">student</code>
2. In the next three steps, you will add a BIG-IP to CM, license BIG-IP and create an app running on BIG-IP so that you have a configuration to backup. For this step, start by adding **bigip1** to **cm1**
    1. Open a Chrome browser and navigate to <code spellcheck="false">https://192.168.1.51</code>
    2. Log in with <code spellcheck="false">admin</code> / <code spellcheck="false">F5trn001!</code>

        > [!Note] Remember to give Central Manager a few minutes to start up
    3. Click the blue **Manage Instances** button
    4. Click **Start Adding Instances**
    5. Provide the IP address for **bigip1**, <code spellcheck="false">192.168.1.31</code> and press **Connect**
    6. Enter creds for **bigip1**, <code spellcheck="false">admin</code> / <code spellcheck="false">F5trn001!</code> and press **Next**
    7. Reuse the <code spellcheck="false">F5trn001!</code> password for the new one and click **Add Instance**
    8. Click **Accept**

        > [!Note] Do **not** click **Ok**

        !IMAGE [a3ud0kty.jpg](instructions273128/a3ud0kty.jpg)
<br>
        @lab.Activity(Question1)
3. License your BIG-IP
    1. Using the Chrome browser, open a new tab and navigate to <code spellcheck="false">https://my.f5.com</code> and grab your BIG-IP Next token

        > [!Note] **Reminder**: Check out *Lesson 1: Deploying BIG-IP Next CM and VE from VM Image* if you forgotten how to create or retrieve your token
    2. Go back to the Chrome browser tab with BIG-IP Next CM
    3. Using the checkbox in the left-most column, select both BIG-IP instances
    4. Click **··· Actions**, select **License** and click **Next**
    5. Paste in the token in the *JSON Web Token (JWT)* box
    6. Give your token a name, such as <code spellcheck="false">trial token</code> and click **Activate**
    7. Wait for the BIG-IP instances to be licensed and click **Exit**
4. Create an application to run on **bigip1**
    1. Click the **Workspace Menu** button (the 3x3 dot matrix icon at the top-left of the page) and select **Applications**

        > [!Note] Later in this lab, the previous step will be reduced to "Navigate to **Workspace Menu :: Application**"
    2. Click the blue **Start Adding Apps** button
    3. Use <code spellcheck="false">web-app</code> as the *Application Service Name* and click **Start Creating Apps**
    4. Click **Start Creating** on the new page
    5. Click the **Pools** tab
    6. Click **+ Create** and enter <code spellcheck="false">web-pool</code> for the *Pool Name*
    7. Click the **Virtual Servers** tab
    8. Enter <code spellcheck="false">web-vs</code> for the *Virtual Server Name*, click the *Pool* dropdown and select **web-pool** and click **Review & Deploy**
    9. Click **Start Adding**, select **big-ip1.f5trn.com** to run the app and press **+ Add to List**
    10. Enter <code spellcheck="false">10.10.1.101</code> for *Virtual Address*
    11. Click the edit icon next to **web-pool**
    12. Click **+ Add Row** *twice* and add the following
<br>
        <code spellcheck="false">www1</code> / <code spellcheck="false">172.16.20.161</code>
<br>
        <code spellcheck="false">www2</code> / <code spellcheck="false">172.16.20.162</code>
    13. Click **Save**
    14. Click **Deploy Changes** and click **Yes, Deploy** to confirm
<br>
        !IMAGE [apurb01g.jpg](instructions273128/apurb01g.jpg)
<br>
        @lab.Activity(Question2)
5. Make a backup of your configuration
    1. Click the **Workspace Menu** icon and select **System**
    2. From the vertical menu on the left side, click **Backup & Restore**

        > [!Note] Later in this lab, the previous two steps will be reduced to "Navigate to **Workspace Menu :: System : Backup & Restore**"
    3. Click **Start Backup**
    4. Click **Next ->**
    5. Use <code spellcheck="false">F5trn001!</code> as your encryption password

        > [!Note] Take a moment to explore the scheduled (automated) daily, weekly and monthly backup feature

        > [!Note] Including *Analytics* in the backup is not an option for the Central Manager because it does not use external storage
    6. Click **Back Up**. You may have to refresh the browser, but you will see a *Backup in progress* message. When it completes you will see
<br>
        !IMAGE [0iaytrrs.jpg](instructions273128/0iaytrrs.jpg)
6. Save the backup *offsite* and delete the local copy
    1. Click the **checkbox** next to the backup you just created. If you have multiple backups, you can compare the creation time in the fifth column
    2. Click the **···Actions** button and select **Download** to save it on the jump box

        > [!Note] For the sake of the use case, consider the jump box as an offsite backup repository
    3. Click the **···Actions** button, select **Delete** and click **Yes, Delete** to confirm
7. Update the **web-app** configuration so the it no longer match the backup configuration
    1. Navigate to **Workspace Menu :: Applications**
    2. Click **web-app**
    3. Mouse over the **web-pool** box and click on the **eye** icon
    4. Click the **Edit** button at the top-right of the page
    5. Click **Review & Deploy**
    6. Click the **Edit** icon next to **web-pool**
    7. Select the **www2** row and click **Remove**
    8. Click **Save**
    9. Click **Deploy Changes**
8. Confirm you only have one pool member now
    1. Click **web-app**
    2. Click the **View** button at the top-right of the page
<br>
        @lab.Activity(Question3)
    3. Click **Cancel**
    4. Click **Exit**
9. Upload the *offsite* backup and restore it
    1. Navigate to **Workspace Menu :: System : Backup & Restore**
    2. Click **Upload Backup File**
    3. Click **Choose File**
    4. In the *Open File* dialog box
        * Select the **Downloads** directory
        * Select the **backup file**
        * Click **Select**
    5. Click **Done**
    6. Select the newly uploaded backup file
    7. Click **···Actions** and select **Restore**
    8. Read the page and then click **Next ->**
    9. Enter <code spellcheck="false">F5trn001!</code> for the *Encryption Password* and click **Yes, Restore**
10. Confirm that Central Manager is configured with the original configuration that was saved in the backup file
    1. After restoring, you will need to log in again using <code spellcheck="false">admin</code> / <code spellcheck="false">F5trn001!</code>

        > [!Note] If you see the message, *Login Failed*, wait a few minutes and try again
    2. Navigate to **Workspace Menu :: Applications**
    3. Click **web-app**
    4. Click the **View** button at the top-right of the page
<br>
        @lab.Activity(Question4)
    5. Click **Cancel**
    6. Click **Exit**

You have backed up and restored your Central Manager configuration and have completed this lab.

===

# Lab 2: Backing Up and Restoring a Single BIG-IP Next Instance

[Return to Lab Contents](#lab-contents)

## Requirements

Before you begin you need:

* A running BIG-IP Next Central Manager that already has a BIG-IP Next VE instance configured
* To have completed the previous lab, *Backing Up and Restoring BIG-IP Next Central Manager*

## Scenario

You are experimenting with configurations on a BIG-IP Next VE instance. You need a way of saving the configuration of a single BIG-IP. Restoring an older Central Manager archive won't work because it will also change the configuration of all the other BIG-IPs managed by that CM

## Objectives

At the end of this lab you will be able to:

* Archive and restore the configuration of a single BIG-IP Next VE instance

## Lab

1. Save the configuration of a single BIG-IP Next VE instance, **bigip1**
    1. In BIG-IP Next CM, navigate to **Workspace Menu :: Infrastructure**
    2. Select **bigip1.f5trn.com** by clicking the check box on the left
    3. Click **··· Actions** and select **Back Up & Schedule** in the dropdown
    4. Use <code spellcheck="false">F5trn001!</code> as your encryption password and click **Schedule Backup -->**
    5. Take a minute to explore the options for scheduling single BIG-IP backups. Notice that it is similar to Central Manager scheduled backups
    6. Click **<-- Back** because you do not want to schedule this backup
    7. Click **Back Up Now**
2. Restore the archive you just created
    1. Navigate to **Workspace Menu :: Infrastructure : Back Up & Restore**
    2. Select the backup you just created and click **Restore**
    3. Enter the encryption password, <code spellcheck="false">F5trn001!</code> and click **Yes, Restore**

You have backed up and restored the configuration of a single BIG-IP instance and have completed this lab.

===

# Lab 3: Creating a QkView and Uploading it to iHealth

[Return to Lab Contents](#lab-contents)

## Requirements

Before you begin you need:

* A running BIG-IP Next Central Manager that already has a BIG-IP Next VE instance configured

## Scenario

You are having a problem making an BIG-IP Next app work the way you think it should work and have contacted F5 Support. They have asked you to create a QkView and upload it to iHealth, so they can diagnose the problem in a Support Lab and not on your production BIG-IP.

## Objectives

At the end of this lab you will be able to:

* Create a QkView and upload it to iHealth

## Lab

1. Get your iHealth **Client Id** and **Secret** info ready so you can create a QkView
    1. Open a new Chrome browser tab and navigate to <code spellcheck="false">https://my.f5.com</code>; you may already have this tab open from licensing your BIG-IP in the first lab
    2. At the top of the web page, click **Resources** and select **iHealth** from the dropdown menu
    3. At the top of the *iHealth* app, click **Settings**
    4. Under *API Token*, click **Generate New Credentials (1)**
    5. Keep this page handy, you will need it directly
2. Create a QkView
    1. Go to your open BIG-IP Next CM browser tab and navigate to **Workspace Menu :: Infrastructure**
    2. Select **bigip1.f5trn.com** by clicking on the checkbox
    3. Click **···Actions** and select **Create QkView** in the dropdown
    4. Copy and paste the **Client Id** from the iHealth app to Central Manager
    5. Copy and paste the **Secret** from iHealth to Central Manager
    6. If you have already opened up a case with F5 Support, enter the *Case Number* now
    7. Click **Submit**
    8. The page will show *Authenticated*
    9. Enter **my-first-qkview** for the *QkView File Name* and click **Generate**
    10. You can refresh the page or wait for it to show the status for *bigip1.f5trn.com* as *QkView creation in progress*
    11. After a few minutes, the message will go away and your QkView has been uploaded to iHealth
3. What exactly did you upload? The QkView provide information the F5 Support team needs to perform diagnostics, but that same information is also available to you
    1. Go back to your iHealth browser tab
    2. Click the **F5 iHealth** logo/title at the top of the page to go the home page
    3. There you will see your uploaded QkViews. Click on the **hostname link** for the one you just uploaded. If there are duplicates, you can use the *Upload Date* in the right column to determine the correct one
    4. From there, you can drill down many different config and log files and command output files to see the same information that F5 Support sees, but this is as far as this lab goes

You have created and uploaded a QkView and have completed this lab.

===

# Lab 4: Troubleshooting with the Debug Sidecar

[Return to Lab Contents](#lab-contents)

## Requirements

Before you begin you need:

* A running BIG-IP Next Central Manager that already has a BIG-IP Next VE instance configured and has a working application
* A basic familiarity with *Kubernetes* will be helpful, but is not required to complete this lab
* A basic familiarity with **tcpdump** will also help, but is not required

## Scenario

Either of following:

* You are installing your BIG-IP Next VE instances and need to confirm the BIG-IP interfaces are connected to the correct VLANs
* You are working with F5 Support and they ask you to run <code spellcheck="false">tcpdump</code> or other troubleshooting commands on the BIG-IP instance

## Objectives

At the end of this lab you will be able to:

* Run troubleshooting commands on BIG-IP Next VE instances

## Lab

### Part 1: Working with Ping and Tcpdump

1. Determine if, from bigip1, you can ping the jump box on the external interface and backend server on the internal interface
    1. On the jump box, open the **terminal** app
    2. Ssh into bigip1 on the management interface with <code spellcheck="false">ssh admin@192.168.1.31</code> using password <code spellcheck="false">F5trn001!</code>
    3. See if you can ping the jump box on its external interface and the backend server on its internal interface. Allow about 10 seconds for the command to run
<br>
        <code spellcheck="false">ping -c 3 10.10.1.30</code>
<br>
        <code spellcheck="false">ping -c 3 172.16.1.160</code>
<br>
        @lab.Activity(Question5)
    4. The above commands work on BIG-IP version 17.1 and earlier, but not BIG-IP Next. The external and internal interfaces are only available to *TMM* which is running in *Kubernetes*. The following two commands show that those two interfaces are not available to Linux
<br>
        <code spellcheck="false">ip address show dev ens192</code>
<br>
        <code spellcheck="false">ip address show dev ens224</code>
<br>
        @lab.Activity(Question6)
2. Try this again running from inside the *debug sidecar*
<br>
    TMM is the part of BIG-IP that is responsible for receiving, processing and sending traffic from one interface to another. The TMM pod has five containers, one of which is the TMM process itself.
    1. You cannot connect directly to the TMM container, but you can connect to the *debug sidecar*, using the following command
<br>
        <code spellcheck="false">sudo kubectl exec --stdin --tty deployment/f5-fsm-tmm --container f5-fsm-debug-sidecar -- bash</code>
<br>
        You are now running a shell (bash) inside the debug container in the TMM pod. The bash prompt is "/"
    2. Now try and ping the jump box on its external interface and the backend server on its internal interface
<br>
        <code spellcheck="false">ping -c 3 10.10.1.30</code>
<br>
        <code spellcheck="false">ping -c 3 172.16.1.160</code>
<br>
        @lab.Activity(Question7)
3. In addition to **ping**, there are several other common tools used for debugging, including **tcpdump** which can be used to see packets moving between hosts as well as inspecting the contents of unencrypted packets
    1. Run the following command to look at traffic *coming from* 172.16.20.161:80. Allow about 10 seconds for the command to run
<br>
        <code spellcheck="false">tcpdump --interface internal-vlan -Ac 10 port 80 and src host 172.16.20.161</code>
<br>
        Inspect the traffic. You will observe the default web page in the traffic
    2. Run the following command to look at traffic *going to* 172.16.20.161:80
<br>
        <code spellcheck="false">tcpdump --interface internal-vlan -Ac 10 port 80 and dst host 172.16.20.161</code>
<br>
        Inspect the traffic, again. If you have worked with **tcpdump**, you will recognize that all of the packets are coming from **bigip1**
<br>
        @lab.Activity(Question8)
    3. Capture user requests going to vitual server 10.10.1.101:80. Because it on port 80, you know the traffic will be HTTP. To make it easier to view the output, filter on "GET"
<br>
        <code spellcheck="false">tcpdump \-\-interface external\-vlan \-\-snapshot\-length 0 port 80 and dst host 10\.10\.1\.101 \| grep GET</code>
    4. Open a new Chrome browser tag and navigate to <code spellcheck="false">http://10.10.1.101</code>
    5. Go back to the terminal and inspect the results. You should see "GET /", which is the default page
<br>
        @lab.Activity(Question9)
    6. Press **CTRL-C** to exit out of **tcpdump**
4. Finally, you can use **traceroute** to track the route a packet takes to its destination
    1. Trace the route a packet takes from this lab to **my.f5.com**
<br>
        <code spellcheck="false">traceroute my.f5.com</code>

***

### Part 2: Viewing Log Files

1. Inspect the available log files
    1. Run the **cd** command to change directory to the log files and then use the **ls** command to list the log files in that directory
<br>
        <code spellcheck="false">cd /logs</code>
<br>
        <code spellcheck="false">ls</code>
<br>
        @lab.Activity(Question10)
2. In general, you will typically not need to view any of these files, unless specifically requested by F5 Support after you have opened a ticket. However you will use a few of these files while troubleshooting iRules developmet in a future lesson.
    1. The debug sidecar does not have a **less** command, so use **more** to page through a file
<br>
        <code spellcheck="false">more f5-fsm-tmm-0.log</code>
    2. You will notice each log entry is a JSON object consisting of the same repeated keys
        * ts: Timestamp
        * sev: Severity
        * ct: Container
        * stream
        * scid
        * sysid
        * mid
        * args
        * log: Log Message
3. The **jq** command will allow you to view only the log message from each log entry. **jq** is not available in the container or even on the BIG-IP Next instance, but there is a way around this problem, because it is available on the *jump box*.
    1. Type <code spellcheck="false">exit</code> to exit the container
    2. Previously you ran the command

        ```-nocopy
        sudo kubectl exec --stdin --tty deployment/f5-fsm-tmm --container f5-fsm-debug-sidecar -- bash
        ```

        where the final argument **bash** told kubectl to run a BASH shell in the debug sidecar container of the TMM pod
    3. Instead of running BASH, run the command **cat /log/f5-fsm-tmm-0.log** to view a specific file. Since the **cat** command is not interactive, remove the **--stdin** and **--tty** flags from the **kubectl exec** command and run
<br>
        <code spellcheck="false">sudo kubectl exec deployment/f5-fsm-tmm --container f5-fsm-debug-sidecar -- cat /logs/f5-fsm-tmm-0.log</code>

        > [!Note] Make sure you understand that you ran this command to list a log file in the debug sidecar container, from the BIG-IP Next instance and not from inside the container
    4. The BIG-IP Next instance, does not have the **jq**, so the next step is to run the above command from the *jump box* using **ssh**
    5. Type <code spellcheck="false">exit</code> to logout out of the BIG-IP Next instance and confirm you are back on the *jump box*
    6. Use **ssh** to run the previous command from the *jump box*
<br>
        <code spellcheck="false">ssh admin@192.168.1.31 sudo kubectl exec deployment/f5-fsm-tmm --container f5-fsm-debug-sidecar -- cat /logs/f5-fsm-tmm-0.log</code>

        > [!Note] You will still be asked for the password for **bigip1**: <code spellcheck="false">F5trn001!</code>
    7. Now you can pipe this command through **jq** view it in a more readable format
<br>
        <code spellcheck="false">ssh admin@192\.168\.1\.31 sudo kubectl exec deployment/f5\-fsm\-tmm \-\-container f5\-fsm\-debug\-sidecar \-\- cat /logs/f5\-fsm\-tmm\-0\.log \| jq \-\-raw\-output \.</code>
    8. You can also use **jq** to filter on just the *log* key
<br>
        <code spellcheck="false">ssh admin@192\.168\.1\.31 sudo kubectl exec deployment/f5\-fsm\-tmm \-\-container f5\-fsm\-debug\-sidecar \-\- cat /logs/f5\-fsm\-tmm\-0\.log \| jq \-\-raw\-output \.log</code>
    9. You can further clean up the output by removing the annoying backslash used to escape some quote marks.
<br>
        <code spellcheck="false">ssh admin@192\.168\.1\.31 sudo kubectl exec deployment/f5\-fsm\-tmm \-\-container f5\-fsm\-debug\-sidecar \-\- cat /logs/f5\-fsm\-tmm\-0\.log \| jq \-\-raw\-output \.log \| sed 's/\\"/"/g'</code>

        > [!Note] The escaping was done because a JSON object was embedded inside of a JSON object, but that doesn't matter for our use. If you don't understand this concept, don't be concerned. The only point here is to make the output more readable
    10. Finally pipe the output through **less** to easily page through it
<br>
        <code spellcheck="false">ssh admin@192\.168\.1\.31 sudo kubectl exec deployment/f5\-fsm\-tmm \-\-container f5\-fsm\-debug\-sidecar \-\- cat /logs/f5\-fsm\-tmm\-0\.log \| jq \-\-raw\-output \.log \| sed 's/\\"/"/g' \| less</code>
4. To summarize, log files are available for your access and can be made more readable, but using them is highly specialized and will be related to specific tasks (creating iRules, for example) or when working with F5 Support on an open ticket.

***

You have run troubleshooting commands and viewed the logs in the debug sidecar and have completed this lab.

===

# Congratulations, You Have Completed This Lesson

## Click **End** to finish this lab
