# CTI Data Collection

![Ansible Version](https://img.shields.io/badge/ansible-%3E%3D2.9-blue.svg)
![Python Version](https://img.shields.io/badge/python-%3E%3D3.4-blue.svg)

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

Установите pyATS в домашней директории
--------------------------------------
```
mkdir pyats && cd pyats
python3 -m venv .
source bin/activate .
pip install ansible pyats genie colorama
```

Реактивируйте виртуальное окружение
-----------------------------------
```
deactivate && source bin/activate .
```

Cклонируйте проект находясь в домашней директории
-------------------------------------------------
```
cd ~
git clone https://github.com/s-terra-ops-cti/DC.git
```

Установите зависимость ansible galaxy в проект
----------------------------------------------
```
cd DC
ansible-galaxy install -r reqs.yaml -p "${ANSIBLE_ROLES_PATH:-roles}"
```

Отредактируйте файл inventory
-----------------------------
```
[IOS]
R2 ansible_host=172.30.30.52

[ASA]
ASA ansible_host=172.30.30.55

[NXOS]
NXOS ansible_host=172.30.30.83
```
Добавьте все оборудование, работающее на IOS и IOSXE в группу [IOS], фаерволы ASA в [ASA] и свитчи Nexus в [NXOS]. В случае если хосты ресолвятся через DNS, IP-адреса опциональны.

Отредактируйте файлы asa.yml, ios.yml и nxos.yml в директории group_vars
------------------------------------------------------------------------
```
ansible_network_os: asa
ansible_user: 
# ansible_password: 
# ansible_become: yes
# ansible_become_password: 
```
Добавьте пользователя, под которым разрешен доступ на оборудование по SSH.

Можете раскоментировать:
* `ansible_password` – если не хотите вводить пароль при запуске playbook’а
* `ansible_become` – если на оборудовании есть enable пароль
* `ansible_become_password` – если не хотите вводить enable пароль при запуске playbook’а

Запустите playbook’и для каждого вида оборудования
--------------------------------------------------
* `ansible-playbook asa_collect.yaml -k` – для сбора данных с оборудования ASA
* `ansible-playbook ios_collect.yaml -k` – для сбора данных с оборудования c оборудования работающего на IOS и IOXE
* `ansible-playbook nxos_collect.yaml -k` – для сбора данных с оборудования Nexus

Ключ -k для последующего ввода пароля пользователя, указанного в файлах asa.yml, iso.yml и nxos.yml, если пароль не указан в `ansible_password`.

Если необходимо ввести пароль enable необходимо добавить ключ -K

Заархивируйте выходные данные
-----------------------------
```
tar -zcvf collection.tar.gz collection/ && md5sum collection.tar.gz > collection.md5
```
Отправьте архив вместе с файлом контрольной суммы `collection.md5` в CTI.