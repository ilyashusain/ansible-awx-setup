# awx setup

Below is an installation script for awx on centos7. Please replace  `echo "x.x.x.x my.site.net mysite" >> /etc/hosts` with the internal ip of the machine where this script is being run.

```
yum -y install epel-release git yum-cron
yum -y install nano
yum -y install glances
sed -i 's/no/yes/g' /etc/yum/yum-cron.conf
systemctl enable --now yum-cron
sed -i 's/ONBOOT=no/ONBOOT=yes/g' /etc/sysconfig/network-scripts/ifcfg-eth0
echo "x.x.x.x my.site.net mysite" >> /etc/hosts
firewall-cmd --permanent --add-service=http
firewall-cmd --reload
yum -y install ansible docker make automake python2-pip
systemctl enable --now docker
python -m pip install --force-reinstall pip==9.0.3
pip install docker-compose
pip install docker
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
source ~/.bashrc
nvm install 8
nvm use 8
cd /opt/
git clone https://github.com/ansible/awx
cd awx/
git clone https://github.com/ansible/awx-logos
mkdir /opt/postgresql
sed -i 's:postgres_data_dir=/tmp/pgdocker:postgres_data_dir=/opt/postgresql:g' /opt/awx/installer/inventory
sed -i 's:awx_official=false:awx_official=true:g' /opt/awx/installer/inventory
cd installer/
ansible-playbook -i inventory install.yml
```










