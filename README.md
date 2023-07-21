# CTI Data Collection

Установите последнюю версию Ansible.
------------------------------------
```
sudo apt remove ansible
sudo apt --purge autoremove
sudo apt update
sudo apt upgrade
sudo apt -y install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt install ansible
```

Обновите модули cisco.asa, cisco.ios и cisco.nxos.
--------------------------------------------------
```
ansible-galaxy collection install cisco.asa
ansible-galaxy collection install cisco.ios
ansible-galaxy collection install cisco.nxos
```
