Task 5: Create a persistent state in Cloud Shell
In this section you will learn a best practice for using Cloud Shell. The gcloud command often requires you to specify values such as a Region, Zone, or Project ID. Entering them repeatedly increases the chance of making typing errors. If you use Cloud Shell frequently, you may want to set common values in environment variables and use them instead of typing the actual values.

Identify available regions
Open Cloud Shell from the Cloud Console. Note that this allocates a new VM for you.

To list available regions, execute the following command:

```
gcloud compute regions list
```

Select a region from the list and note the value in any text editor. This region will now be referred to as [YOUR_REGION] in the remainder of the lab.


Create and verify an environment variable
Create an environment variable and replace [YOUR_REGION] with the region you selected in the previous step:

```
INFRACLASS_REGION=[YOUR_REGION]
```

Verify it with echo:

```
echo $INFRACLASS_REGION
```

You can use environment variables like this in gcloud commands to reduce the opportunities for typos and so that you won't have to remember a lot of detailed information.

Every time you close Cloud Shell and reopen it, a new VM is allocated, and the environment variable you just set disappears. In the next steps, you create a file to set the value so that you won't have to enter the command each time Cloud Shell is recycled.
Append the environment variable to a file
Create a subdirectory for materials used in this lab:

```
mkdir infraclass
```

Create a file called config in the infraclass directory:

```
touch infraclass/config
```

Append the value of your Region environment variable to the config file:

```
echo INFRACLASS_REGION=$INFRACLASS_REGION >> ~/infraclass/config
```

Create a second environment variable for your Project ID, replacing [YOUR_PROJECT_ID] with your Project ID. You can find the project ID on the Cloud Console Home page.

```
INFRACLASS_PROJECT_ID=[YOUR_PROJECT_ID]
```

Append the value of your Project ID environment variable to the config file:

```
echo INFRACLASS_PROJECT_ID=$INFRACLASS_PROJECT_ID >> ~/infraclass/config
```

Use the source command to set the environment variables, and use the echo command to verify that the project variable was set:

```
source infraclass/config
echo $INFRACLASS_PROJECT_ID
```

This gives you a method to create environment variables and to easily recreate them if the Cloud Shell is recycled. However, you will still need to remember to issue the source command each time Cloud Shell is opened. In the next step, you modify the .profile file so that the source command is issued automatically every time a terminal to Cloud Shell is opened.
Close and re-open Cloud Shell. Then issue the echo command again:

```
echo $INFRACLASS_PROJECT_ID
```

There will be no output because the environment variable no longer exists.

Modify the bash profile and create persistence
Edit the shell profile with the following command:

```
nano .profile
```

Add the following line to the end of the file:

```
source infraclass/config
```

Press Ctrl+O, ENTER to save the file, and then press Ctrl+X to exit nano.

Close and then re-open Cloud Shell to recycle the VM.

Use the echo command to verify that the variable is still set:

```
echo $INFRACLASS_PROJECT_ID
```
You should now see the expected value that you set in the config file.

