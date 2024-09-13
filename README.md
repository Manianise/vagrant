### Easy to install 2 machines with bento/ubuntu

![vagrant]https://img.shields.io/badge/vagrant-VM-1868F2?logo=vagrant
![ubuntu]https://img.shields.io/badge/ubuntu-v22.04.4-E95420?logo=ubuntu

> #### Make sure you have [vagrant](https://developer.hashicorp.com/vagrant/docs/installation) installed
>> user/password is always vagrant/vagrant

***

#### Run the VMS
```
vagrant up &&
vagrant ssh <HOSTNAME>
```

#### If you wish to add GUI :

```
sudo apt-get update &&
sudo apt-get install -y ubuntu-desktop
sudo apt-get install -y gnome-core gdm3
sudo systemctl set-default graphical.target
```

#### Restart VM

```
sudo reboot
```

