# Cloud Monitoring: Qwik Start

Cloud Monitoring provides visibility into the performance, uptime, and overall health of cloud-powered applications. 
Cloud Monitoring collects metrics, events, and metadata from Google Cloud, Amazon Web Services, hosted uptime probes, 
application instrumentation, and a variety of common application components including Cassandra, Nginx, 
Apache Web Server, Elasticsearch, and many others. Cloud Monitoring ingests that data and generates insights 
via dashboards, charts, and alerts. Cloud Monitoring alerting helps you collaborate by integrating with Slack, 
PagerDuty, HipChat, Campfire, and more.

In this lab you'll install monitoring and logging agents to collect information from your instance, which could include 
metrics and logs from 3rd party apps.


## Task 1. Create a Compute Engine instance
1. In the Cloud Console dashboard, go to Navigation menu > Compute Engine > VM instances, then click Create instance.
2. Fill in the fields as follows, leaving all other fields at the default value
3. Click Create


## Task 2. Add Apache2 HTTP Server to your instance
1. In the Console, click SSH in line with lamp-1-vm to open a terminal to your instance.
2. Run the following commands in the SSH window to set up Apache2 HTTP Server:
   - sudo apt-get update
   - sudo apt-get install apache2 php7.0
3. When asked if you want to continue, enter Y
   - sudo service apache2 restart
4. Return to the Cloud Console, on the VM instances page. Click the External IP for lamp-1-vm instance to see the Apache2 default page for this instance.


## Create a Monitoring Metrics Scope
Set up a Monitoring Metrics Scope that's tied to your Google Cloud Project. The following steps create a new account that 
has a free trial of Monitoring.

In the Cloud Console, click Navigation menu (Navigation menu icon) > Monitoring.

When the Monitoring Overview page opens, your metrics scope project is ready.


## Install the Monitoring and Logging agents
Agents collect data and then send or stream info to Cloud Monitoring in the Cloud Console.

1. Run the Monitoring agent install script command in the SSH terminal of your VM instance to install the Cloud Monitoring agent:
   - curl -sSO https://dl.google.com/cloudagents/add-google-cloud-ops-agent-repo.sh
   - sudo bash add-google-cloud-ops-agent-repo.sh --also-install
2. If asked if you want to continue, press Y
3. Run the Logging agent install script command in the SSH terminal of your VM instance to install the Cloud Logging agent:
   - sudo systemctl status google-cloud-ops-agent"*"
4. Press q to exit the status
   - sudo apt-get update


## Task 3. Create an uptime check
Uptime checks verify that a resource is always accessible. For practice, create an uptime check to verify your VM is up.

1. In the Cloud Console, in the left menu, click Uptime checks, and then click Create Uptime Check.
2. For Protocol, select HTTP.
3. For Resource Type, select Instance.
4. For Instance, select lamp-1-vm.
5. For Check Frequency, select 1 minute.
6. Click Continue.
7. In Response Validation, accept the defaults and then click Continue.
8. In Alert & Notification, accept the defaults, and then click Continue.
9. For Title, type Lamp Uptime Check.
10. Click Test to verify that your uptime check can connect to the resource.
11. When you see a green check mark everything can connect, Click Create.


## Task 4. Create an alerting policy
Use Cloud Monitoring to create one or more alerting policies.

1. In the left menu, click Alerting, and then click +Create Policy.
2. Click on Select a metric dropdown. Uncheck the Active.
3. Type Network traffic in filter by resource and metric name and click on VM instance > Interface. Select Network traffic (agent.googleapis.com/interface/traffic) and click Apply. Leave all other fields at the default value.
4. Click Next.
5. Set the Threshold position to Above threshold, Threshold value to 500 and Advanced Options > Retest window to 1 min. Click Next.
6. Click on the drop down arrow next to Notification Channels, then click on Manage Notification Channels.
A Notification channels page will open in a new tab.
7. Scroll down the page and click on ADD NEW for Email.
8. In the Create Email Channel dialog box, enter your personal email address in the Email Address field and a Display name.
9. Click on Save.
10. Go back to the previous Create alerting policy tab.
11. Click on Notification Channels again, then click on the Refresh icon to get the display name you mentioned in the previous step.
12. Click on Notification Channels again if necessary, select your Display name and click OK.
13. Add a message in documentation, which will be included in the emailed alert.
14. Mention the Alert name as Inbound Traffic Alert.
15. Click Next.
16. Review the alert and click Create Policy.

You've created an alert! While you wait for the system to trigger an alert, create a dashboard and chart, and then check out Cloud Logging.


## Task 5. Create a dashboard and chart
You can display the metrics collected by Cloud Monitoring in your own charts and dashboards. In this section you create the charts for the lab metrics and a custom dashboard.

1. In the left menu select Dashboards, and then +Create Dashboard.
2. Name the dashboard Cloud Monitoring LAMP Qwik Start Dashboard.

### Add the first chart
1. Click on + ADD WIDGET
2. Select the Line option under Visualization in the Add widget.
3. Name the Widget title CPU Load.
4. Click on Select a metric dropdown. Uncheck the Active.
5. Type CPU load (1m) in filter by resource and metric name and click on VM instance > Cpu. Select CPU load (1m) and click Apply. Leave all other fields at the default value. Refresh the tab to view the graph.

### Add the second chart
1. Click + Add WIDGET and select Line option under Visualization in the Add widget.
2. Name this Widget title Received Packets.
3. Click on Select a metric dropdown. Uncheck the Active.
4. Type Received packets in filter by resource and metric name and click on VM instance > Instance. Select Received packets and click Apply. Refresh the tab to view the graph.
5. Leave the other fields at their default values. You see the chart data.


## Task 6. View your logs
Cloud Monitoring and Cloud Logging are closely integrated. Check out the logs for your lab.

1. Select Navigation menu > Logging > Logs Explorer.
2. Select the logs you want to see, in this case, you select the logs for the lamp-1-vm instance you created at the start of this lab:
   - Click on Resource.
   - Select VM Instance > lamp-1-vm in the Resource drop-down menu.
   - Click Apply.
   - Leave the other fields with their default values.
   - Click the Stream logs.

### Check out what happens when you start and stop the VM instance.
To best see how Cloud Monitoring and Cloud Logging reflect VM instance changes, make changes to your instance in one browser window and then see what happens in the Cloud Monitoring, and then Cloud Logging windows.

1. Open the Compute Engine window in a new browser window. Select Navigation menu > Compute Engine, right-click VM instances > Open link in new window.
2. Move the Logs Viewer browser window next to the Compute Engine window. This makes it easier to view how changes to the VM are reflected in the logs
3. In the Compute Engine window, select the lamp-1-vm instance, click the three vertical dots at the right of the screen and then click Stop, and then confirm to stop the instance. 
It takes a few minutes for the instance to stop.
4. Watch in the Logs View tab for when the VM is stopped.
5. In the VM instance details window, click the three vertical dots at the right of the screen and then click Start/resume, and then confirm. It will take a few minutes for the instance to re-start. Watch the log messages to monitor the start up.


## Task 7. Check the uptime check results and triggered alerts
1. In the Cloud Logging window, select Navigation menu > Monitoring > Uptime checks. This view provides a list of all active uptime checks, and the status of each in different locations.
You will see Lamp Uptime Check listed. Since you have just restarted your instance, the regions are in a failed status. It may take up to 5 minutes for the regions to become active. Reload your browser window as necessary until the regions are active.

2. Click the name of the uptime check, Lamp Uptime Check.
Since you have just restarted your instance, it may take some minutes for the regions to become active. Reload your browser window as necessary.

### Check if alerts have been triggered
1. In the left menu, click Alerting.
2. You see incidents and events listed in the Alerting window.
3. Check your email account. You should see Cloud Monitoring Alerts.