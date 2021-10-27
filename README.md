# Part X: Satellite VMWare Provisioning Scratch Pad

[Tutorial Menu](https://github.com/pslucas0212/RedHat-Satellite-VM-Provisioning-to-vSphere-Tutorial)  

Scrach space for temporary instructions. Content on this page will change as I transition the content to a "formal" document.

First set the networking parameters for the template VM.  This can be done via the System Tools GUI or from the command line.
```
[root@localhost ~]# nmcli con modify ens192 connection.id ens192
[root@localhost ~]# nmcli con mod ens192 ipv4.method auto
[root@localhost ~]# nmcli con edit ens192
nmcli> remove ipv4.dns
nmcli> set ipv4.ignore-auto-dns yes
nmcli> set ipv4.dns 10.1.10.254
nmcli> save
nmcli> quit
[root@localhost ~]# nmcli device reapply ens192
[root@localhost ~]# nmcli device show ens192
[root@localhost ~]# hostnamectl set-hostname "localhost.localdomain"
```

We will now need to temporarily subscribe to Satellite to access template packages. Note: Here for --org parameter we use the Operations Department label.
```
# rpm -ivh http://sat01.example.com/pub/katello-ca-consumer-latest.noarch.rpm
# subscription-manager register --org=operations --activationkey=ak-ops-rhel8-prem-server 
```

To support the creation of the VM teamplate, we willInstall the followeing the cloud-init, open-vm-tools and perl packages.
```
# yum -y install cloud-init open-vm-tools perl
```

Enable the CA certificates for the image:
```
# update-ca-trust enable 
```

Download the katello-server-ca.crt file from Satellite Server:
```
# wget -O /etc/pki/ca-trust/source/anchors/cloud-init-ca.crt http://sat01.example.com/pub/katello-server-ca.crt
```

To update the record of certificates, enter the following command:
```
# update-ca-trust extract
```

Configure cloud-init to skip networking
```
# cat << EOF > /etc/cloud/cloud.cfg.d/01_network.cfg
network:
  config: disabled
EOF
```  

We setup cloud-init to call backk to Satellite
```
# cat << EOF > /etc/cloud/cloud.cfg.d/10_foreman.cfg
datasource_list: [NoCloud]
datasource:
  NoCloud:
    seedfrom: http://sat01.example.com/userdata/
EOF
```

Make up a backup of the default cloud-init and relace the default cloud-init
```
# cp /etc/cloud/cloud.cfg ~/cloud.cfg.`date -I`
# cat << EOF > /etc/cloud/cloud.cfg
cloud_init_modules:
 - bootcmd

cloud_config_modules:
 - runcmd

cloud_final_modules:
 - scripts-per-once
 - scripts-per-boot
 - scripts-per-instance
 - scripts-user
 - phone-home

system_info:
  distro: rhel
  paths:
    cloud_dir: /var/lib/cloud
    templates_dir: /etc/cloud/templates
  ssh_svcname: sshd

# vim:syntax=yaml
EOF
```

We will now unregister the server from Satellite
```
# subscription-manager unregister
Unregistering from: sat01.example.com:443/rhsm
System has been unregistered.
# subscription-manager clean
All local data removed
```
We will creat a clean up script.
```
# cat > ~/clean.sh <<EOF
#!/bin/bash

# stop logging services
/usr/bin/systemctl stop rsyslog
/usr/bin/service stop auditd

# remove old kernels
# /bin/package-cleanup -oldkernels -count=1

#clean yum cache
/usr/bin/yum clean all

#force logrotate to shrink logspace and remove old logs as well as truncate logs
/usr/sbin/logrotate -f /etc/logrotate.conf
/bin/rm -f /var/log/*-???????? /var/log/*.gz
/bin/rm -f /var/log/dmesg.old
/bin/rm -rf /var/log/anaconda
/bin/cat /dev/null > /var/log/audit/audit.log
/bin/cat /dev/null > /var/log/wtmp
/bin/cat /dev/null > /var/log/lastlog
/bin/cat /dev/null > /var/log/grubby

#remove udev hardware rules
/bin/rm -f /etc/udev/rules.d/70*

#remove uuid from ifcfg scripts
/bin/cat > /etc/sysconfig/network-scripts/ifcfg-ens192 <<EOM
DEVICE=ens192
ONBOOT=yes
EOM

#remove SSH host keys
/bin/rm -f /etc/ssh/*key*

#remove root users shell history
/bin/rm -f ~root/.bash_history
unset HISTFILE

#remove root users SSH history
/bin/rm -rf ~root/.ssh/known_hosts
EOF
```

Run the clean script.
```
sh ~/clean.sh
```

Finally we will power off the system to make the VMWare template
```
# systemctl poweroff
```

