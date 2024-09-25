### Easy to install Prometheus and Grafana with ansible on Ubuntu using bento/ubuntu Vagrant box

![vagrant](https://img.shields.io/badge/vagrant-VM-1868F2?logo=vagrant)
![ubuntu](https://img.shields.io/badge/ubuntu-v22.04-E95420?logo=ubuntu)
![grafana](https://img.shields.io/badge/grafana-latest-F46800?logo=grafana)
![prometheus](https://img.shields.io/badge/prometheus-latest-E6522C?logo=prometheus)

> #### Make sure you have [vagrant](https://developer.hashicorp.com/vagrant/docs/installation) installed

User/password is always vagrant/vagrant | You need Grafana credentials for this code to work
|--------------------------------------|-----------------------------------------------|

***

#### Run the file
```
vagrant up --provision
```

#### Login to VMs with SSH

```
vagrant ssh <HOSTNAME>
```

#### Setting up Prometheus/Grafana

- Ansible is automatically installed on vagrant boxes. run the following command to install

|  /!\ You will need Grafana credentials user name and pwd to run this code : |
|-------------------------------------------------|

```
cd /vagrant/data/ansible-prometheus-grafana/
```

- Replace content of vault.yml with Grafana credentials

```
ansible-galaxy install -r requirements.yml
ansible-playbook playbook.yml
```