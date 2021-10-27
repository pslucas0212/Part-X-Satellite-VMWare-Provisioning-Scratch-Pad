# Part X: Satellite VMWare Provisioning Scratch Pad

[Tutorial Menu](https://github.com/pslucas0212/RedHat-Satellite-VM-Provisioning-to-vSphere-Tutorial)  

### Scrach space for temporary instructions. Content on this page will change as I transition the content to a "formal" document.

## Enabling Cloud Connector on Satellite for Insights Remote Remediation

Preparing Satellite to upload host inventory

We will need to install the invntory plug-in on Satellite and we will also enable users to run commands over SSH as root on Satellite if needed.  Watch for the Success! message when the task is completed.

```
# satellite-installer \
--enable-foreman-plugin-rh-cloud \
--foreman-proxy-plugin-remote-execution-ssh-install-key true
```

From the Satellite console we restart the inventory upload process.  From the left hand navigation bar click Confgure -> Inventory Upload.

![Configure -> Inventory Upload](/images/satxx.png)

On the Red Hat Inventory page clear the Filter to see the Operations Department organization.  Alternatively you can choose the Operations Department organization near the top of the Satellite console in the Organizations drop down list.  

Click on the > on the Operations Department line to expand the view, and the click the blue Restart button.  Notice the Settings options in the upper right corner of the Red Hat Inventory screen.  Click the ? symbol to get more information on each setting and chose the settings that best meet your organizations requirements.  For this tutorial we will only chose Automatic inventory upload setting to be set to on.  

![Red Hat Inventory Page](/images/satxx.png)

We whenwe provision a RHEL 8.3 host from Satellin, we have enable host registration to Insights in the cloud-init template.  No other action is needed for this step.

