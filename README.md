# Vagrant RHCE lab setup

Vagrant files for RHCE exam preparation.
Learn more about vagrant at: https://www.vagrantup.com/


## Installation

### Ubuntu 16.04.1 LTS

#### Provider: virtualbox

1. Update repos: `sudo apt update`
2. Install vagrant: `sudo apt -y install vagrant`
3. Install virtualbox: `sudo apt -y install virtualbox`
4. Patch a file if vagrant version is 1.8.1 (If you manually installed latest vagrant, no need to patch) link: http://stackoverflow.com/questions/36811863/cant-install-vagrant-plugins-in-ubuntu-16-04/36991648
5. Download centos vagrant box:	`vagrant box add centos/7` If asked for provider select virtualbox.
6. Clone this and run `vagrant up` inside lab/virtualbox

###CentOS 7.2/Fedora 24

#### Provider: libvirt

1. Update: `sudo yum update`
2. Install dependencies and vagrant: 
```
sudo yum -y install vagrant redhat-rpm-config vagrant-libvirt vagrant-libvirt-doc libvirt-devel libxslt-devel libxml2-devel virt-manager
```
3. Install required vagrant plugins: 
```
vagrant plugin install vagrant-libvirt 
vagrant plugin install fog
vagrant plugin install sahara
```
4. Download centos 7 vagrant box: `vagrant box add centos/7` If asked for provider select libvirt.
5. Clone this and run `vagrant up` inside lab/libvirt


##### If using fedora replace yum by dnf
##### run `vagrant up --no-parallel` to prevent bringing up all VM's together when using libvirta

### Run these commands if you do not have a local repo

#### Ubuntu 16.04.1 LTS

```
sed -i '7,13s/^/#/g' virtualbox/scripts/classroom/classroom.sh
sed -i '12,18s/^/#/g' virtualbox/scripts/server/server.sh
sed -i '12,18s/^/#/g' virtualbox/scripts/desktop/desktop.sh
```

#### CentOS 7.2/Fedora 24

```
sed -i '7,13s/^/#/g' libvirt/scripts/classroom/classroom.sh
sed -i '12,18s/^/#/g' libvirt/scripts/server/server.sh
sed -i '12,18s/^/#/g' libvirt/scripts/desktop/desktop.sh
```

## Recommendations

1. If possible clone the official centos mirrors so that the packages are available locally. Find the nearest mirros from: https://www.centos.org/download/mirrors/

### Ubuntu 16.04.1 LTS

1. Run a cron job daily using the script, scripts/repoupdate 
2. Install apache webserver
```
sudo apt -y install apache2
sudo rm -f /var/www/html/*
sudo systemctl enable apache2
sudo systemctl restart apache2
```
3. Modify the ip address in the scripts in scripts/ folder to the ip of your base machine: 
```
find scripts/ -type f -name "*.sh" -exec sed -i 's/172.16.0.143/<your-ip>/g' {}
```

### CentOS 7.2/Fedora 24

1. Run a cron job daily using the script, scripts/repoupdate
2. Install apache webserver
```
sudo yum -y install httpd
sudo sed -i s/^/#/g /etc/httpd/conf.d/welcome.conf
sudo systemctl enable httpd
sudo systemctl restart httpd
```
3. Modify the ip address in the scripts in scripts/ folder to the ip of your bas
e machine: 
```
find scripts/ -type f -name "*.sh" -exec sed -i 's/172.16.0.143/<your-ip>/g' {}
```
#### Modify the scripts in scripts/ folder as per need



