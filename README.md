# Part X: Satellite VMWare Provisioning Scratch Pad

### Scrach space for temporary instructions. Content on this page will change as I transition the content to a "formal" document.

[Tutorial Menu](https://github.com/pslucas0212/RedHat-Satellite-VM-Provisioning-to-vSphere-Tutorial) 

## Registering an Exsiting RHEL VM to Satellite

In this tutorial we will add an existing RHEL VM to Satellite for content management.  

If you don't already have an operating system defined in the host section we will need to do that first.  On the left side navibation bar chose Hosts -> Operating Systems.

![Host -> Operating Systems](/images/sat83.png)

On the Operating Systems page click the blud Create Operating System button. 

![Blue Operating System button](/images/sat84.png)

On the Operating Systems > Create Operating Systems page fill in or chose options from the following table, and click the blue Submit button.  We will only be filling in information on the Operating System tab and accept the default settings for any other tabs on this page.  

Name | Choice
---- | ------
Name* | RedHat
Major Version* | 8
Minor Version | 4
Family | Red Hat
Architecture | x86_64

![Define Operating System](/images/sat85.png)
