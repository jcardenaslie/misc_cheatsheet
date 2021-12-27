Resource Monitoring
1 hour
Free
Overview
In this lab, you learn how to use Cloud Monitoring to gain insight into applications that run on Google Cloud.

Objectives
In this lab, you learn how to perform the following tasks:

Explore Cloud Monitoring

Add charts to dashboards

Create alerts with multiple conditions

Create resource groups

Create uptime checks

Qwiklabs setup
For each lab, you get a new GCP project and set of resources for a fixed time at no cost.

Make sure you signed into Qwiklabs using an incognito window.

Note the lab's access time (for example, img/time.png and make sure you can finish in that time block.

There is no pause feature. You can restart if needed, but you have to start at the beginning.

When ready, click img/start_lab.png.

Note your lab credentials. You will use them to sign in to Cloud Platform Console. img/open_google_console.png

Click Open Google Console.

Click Use another account and copy/paste credentials for this lab into the prompts.

If you use other credentials, you'll get errors or incur charges.

Accept the terms and skip the recovery resource page.
Do not click End Lab unless you are finished with the lab or want to restart it. This clears your work and removes the project.

Task 1: Create a Cloud Monitoring workspace
Verify resources to monitor
Three VM instances have been created for you that you will monitor.

In the Cloud Console, on the Navigation menu (Navigation menu), click Compute Engine > VM instances. Notice the nginxstack-1, nginxstack-2 and nginxstack-3 instances.
Create a Monitoring workspace
You will now setup a Monitoring workspace that's tied to your Qwiklabs GCP Project. The following steps create a new account that has a free trial of Monitoring.

In the Google Cloud Platform Console, click on Navigation menu > Monitoring.

Wait for your workspace to be provisioned.

When the Monitoring dashboard opens, your workspace is ready.

Overview.png

Why is monitoring important to Google?
check
It is at the base of site reliability which incorporates aspects of software engineering and applies that to operations whose goals are to create ultra-scalable and highly reliable software systems.

Google uses monitoring to ensure they have all the important metrics for reporting purposes to customers and the other interested bodies. The number of reports requires the collection and reporting to be both broad and deep.

Monitoring is important to ensure that Google complies with regulatory requirements defined by both government and industry security bodies.

Task 2: Custom dashboards
Create a dashboard
In the left pane, click Dashboards.

Click +Create Dashboard.

For New Dashboard Name, type My Dashboard.

Add a chart
From Chart library, Select Line.

For Title, give your chart a name (you can revise this before you save based on the selections you make).

For Resource type, select VM Instance.

For Metric, select a metric to chart for the Instance resource, such as CPU utilization or CPU usage.

Click + Add Filter and explore the various options.

Metrics Explorer
The Metrics Explorer allows you to examine resources and metrics without having to create a chart on a dashboard. Try to recreate the chart you just created using the Metrics Explorer.

In the left pane, click Metrics Explorer.
For Find resource type and metric, type a metric or resource name.
Explore the various options and try to recreate the chart you created earlier.
Not all metrics are currently available on the Metrics Explorer, so you might not be able to find the exact metric you used on the previous step.

Task 3: Alerting policies
What is not a recommended best practice for alerts?

Customize your alerts to the audience need.
check
Report all noise to ensure all data points are presented.

Use multiple notification channels so you avoid a single point of failure.

Configure alerting on symptoms and not necessarily causes.

Create an alert and add the first condition
In the left pane, click Alerting.
Click + Create Policy.
Click Add Condition.
For Find resource type and metric, select VM Instance.
If you cannot locate the VM Instance resource type, you might have to refresh the page.

Select a metric you are interested in evaluating, such as CPU usage or CPU Utilization.

Under Configuration, for Condition, select is above.

Specify the threshold value and for how long the metric must cross this set value before the alert is triggered. For example, for THRESHOLD, type 20 and set FOR to 1 minute.

Click ADD.

Add a second condition
Click Add Condition.

Repeat the steps above to specify the second condition for this policy. For example, repeat the condition for a different instance. Click ADD.

In Policy Triggers, for Trigger when, click All conditions are met.

Click Next.

Configure notifications and finish the alerting policy
Click on dropdown arrow next to Notification Channels, then click on Manage Notification Channels.
A Notification channels page will open in new tab.

Scroll down the page and click on ADD NEW for Email.
Enter your personal email in the Email Address field and a Display name.
Click Save.
Go back to the previous Create alerting policy tab.
Click on Notification Channels again, then click on the Refresh icon to get the display name you mentioned in the previous step.
Now, select your Display name and click OK.
Click Next.
Enter a name of your choice in Alert name field.
Click Save.
Click Check my progress to verify the objective.
Create alerting policies

Task 4: Resource groups
In the left pane, click Groups.

Click + Create Group.

Enter a name for the group. For example: VM instances

In the Criteria section, type nginx in the value field below Contains.

Click DONE.

Click CREATE.

Review the dashboard Cloud Monitoring created for your group.

Task 5: Uptime monitoring
Select all valid targets for Cloud Monitoring uptime alert notifications.

Pub/sub

3rd party service

EC2 service

email

SMS

webhook
There is at least one more correct answer.

In the Monitoring tab, click on Uptime Checks.
Click + Create Uptime Check.
Specify the following, and leave the remaining settings as their defaults:
Property	Value (type value or select option as specified)
Title	Enter a title then click Next
Protocol	HTTP
Resource Type	Instance
Applies To	Group
Group	Select your group
Check Frequency	1 minute
Click on Next to leave the other details to default. Under Alert & Notification, select your Notification Channels from the dropdown.

Click Test to verify that your uptime check can connect to the resource.

When you see a green check mark everything can connect. Click Create.

The uptime check you configured takes a while for it to become active.

Click Check my progress to verify the objective.
Create uptime monitoring

Task 6: Review
In this lab, you learned how to:

Monitor your projects
Create a Cloud Monitoring workspace
Create alerts with multiple conditions
Add charts to dashboards
Create resource groups
Create uptime checks for your services
