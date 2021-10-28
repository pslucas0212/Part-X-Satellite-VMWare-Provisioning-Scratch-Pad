# Part X: Satellite VMWare Provisioning Scratch Pad

[Tutorial Menu](https://github.com/pslucas0212/RedHat-Satellite-VM-Provisioning-to-vSphere-Tutorial)  

### Scrach space for temporary instructions. Content on this page will change as I transition the content to a "formal" document.

## Enabling Cloud Connector on Satellite for Insights Remote Remediation

??? Blah, blah, why are we doing this...  to automatically remediate RHEL via Insights.

Preparing Satellite to upload host inventory

We will need to install the invntory plug-in on Satellite and we will also enable users to run commands over SSH as root on Satellite if needed.  Watch for the Success! message when the task is completed.

```
# satellite-installer \
--enable-foreman-plugin-rh-cloud \
--foreman-proxy-plugin-remote-execution-ssh-install-key true
```

From the Satellite console we set up the inventory upload process.  From the left hand navigation bar click Confgure -> Inventory Upload.

![Configure -> Inventory Upload](/images/sat70.png)

On the Red Hat Inventory page choose the Operations Department organization near the top of the Satellite console in the Organizations drop down list.  

Click on the Configure Cloud Connector button.  After clicking on the button you  will see the status change to Cloud Connector is in progress.  

Click on the view the job in progress link from the pop-up to watch that the job successfully completes.

![Red Hat Inventory Page](/images/sat71.png)

Click on the > on the Operations Department line to expand the view, and the click the blue Restart button.  Blue Restart button will be grayed out.  When the Restart process is finished, click the Uploading icon.

![Red Hat Inventory Page - Refresh - Upload](/images/sat72.png)

Now you will see that the Generating and Uploading statuses have check marks.  Also, notice the Settings options in the upper right corner of the Red Hat Inventory screen.  Click the ? symbol to get more information on each setting and chose the settings that best meet your organizations requirements.  For this tutorial we will only chose Automatic inventory upload setting to be set to on.  

![Statuses Checked](/images/sat73.png)

If you have any issues, you can often get additional information from the Job status.  On left hand navigation bar, chose Dashboard -> Jobs.

![Dashboard -> Jobs](/images/sat74.png)

On the Jobs Invocation page, click the link to the particular job for more detail.

![Job Invocation Page](/sat/images/sat75.png)

We enabled host registration to Insights in the cloud-init template.  No other action is needed for this step.

To finish setting up ??? automatic RHEL remediation, we need to enable Insights...

First login to your Red Hat customer portal and click the Subscriptions tab.

![RH Portal Subscriptions Tab](/images/sat76.png)

On your Red Hat customer portal page, click the Manage drop down list and chose RHSM API Token.

![RH Portal -> Manage -> RHSM API Token](/inages/sat77.png)

On the Red Hat API Tokens screen clock the blue GENERATE TOKEN button and on the next page clock the click the copy button.

![RHSM API Tokens - GENERATE TOKEN](/images/sat78.png)

Return to the Red Hat Satellite console and on the left hand navigation bar choose Administer -> Settings

![Administer -> Settings](/images/sat79.png)

On the settings page click the RH Cloud tab and on that tab page, click the pencil on the Red Hat Cloud Token line.

![Edit Red Hat Cloud Token](/images/sat80.png)

In the Update value for Red Hat Cloud token setting dialog box, paste in the token you copied from the Red Hat customer portal page and click the blue Submit button.

![Update value for Red Hat Cloud token](/images/sat81.png)

On the left hand navigation bar choose Configure -> Insights.

![Configure -> Insighs](/images/sat82.png)

On the Red Hat Insights page, let's change the Settings | Synchronize Automaticall to On and then click the Start recommendations sync to start the first synchronization.  On this page we will see RHEL VM hostnames and recommended remediations.  

![Red Hat Insights Page]{/images/sat83.png)


