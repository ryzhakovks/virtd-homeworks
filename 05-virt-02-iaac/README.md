## Задача 1

- Опишите своими словами основные преимущества применения на практике IaaC паттернов.

```
Непрерывная интеграция (CI) - сокращение стоимости исправления дефекта за счет его раннего выявления. 
Непрерывная доставка (CD) - изменения выпускаются небольшими партиями, их можно быстро изменить или устранить.
Непрерывное развёртывание (CD) - упраздняет ручные действия, не требуя непосредственного утверждения со стороны ответственного лица. Но непрерывное развёртывание на прод системах может быть нежелательным, т.к. может разрушить инфраструктуру из-за ошибки в коде. Непрерывное развертывание лучше использовать на тестовой среде, а в прод запускать только после контроля со стороны ответственного лица.
```

- Какой из принципов IaaC является основополагающим?

```
Принципы IaC позволяют не фокусироваться на рутине, а заниматься более важными задачами. Автоматизация инфраструктуры позволяет эффективнее использовать существующие ресурсы. Также автоматизация позволяет минимизировать риск возникновения человеческой ошибки. Основополагающим принципом IaaC является идемпотентность, с помощью этого принципа не нужно задумываться о дрейфе конфигураций.
```

## Задача 2

- Чем Ansible выгодно отличается от других систем управление конфигурациями?

```
Ansible написан на Python (интерпретатор которого есть в большинстве дистрибутивов linux), использует метод push, что не требует установки агентов. Использует SSH без необходимости дополнительно докручивать PKI. 
```

- Какой, на ваш взгляд, метод работы систем конфигурации более надёжный push или pull?

```
Для управления малым парком машин лучше подходит push.

Для управления большим (больше 500 машин) лучше подоёдет pull. Реализация не потребует больших мощностей для машины управления, но при pull реализации необходимы системы с агентами. Что вызовет дополнительный расход ресурсов специалистов для подготовки машин.
```

## Задача 3

Установить на личный компьютер:

- VirtualBox
- Vagrant
- Ansible

*Приложить вывод команд установленных версий каждой из программ, оформленный в markdown.*

VirtualBox:

```
apt update
sudo apt-get install virtualbox
sudo apt-get install virtualbox—ext–pack

virtualbox -h
Oracle VM VirtualBox Manager 5.2.42_Ubuntu
(C) 2005-2020 Oracle Corporation
All rights reserved.

vboxmanage --version
5.2.42_Ubuntur137960
```

Vagrant:

```
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install vagrant

vagrant --version
Vagrant 2.3.0
```

Ansible:

```
sudo apt install ansible

ansible --version
ansible 2.5.1
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/root/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 2.7.17 (default, Jul  1 2022, 15:56:32) [GCC 7.5.0]
```

## Задача 4 (*)

Воспроизвести практическую часть лекции самостоятельно.

- Создать виртуальную машину.

Для нормальной отработки ansible playbooks пришлось немного изменить файл inventory. Т.к. ansible не находил интерпретатор python

```yml
[nodes:children]
manager

[manager]
server1.netology ansible_host=127.0.0.1 ansible_port=20011 ansible_user=vagrant

[manager:vars]
ansible_python_interpreter=/usr/bin/python3
```

```log
vagrant up
Bringing machine 'server1.netology' up with 'virtualbox' provider...
==> server1.netology: Importing base box 'bento/ubuntu-20.04'...
==> server1.netology: Matching MAC address for NAT networking...
==> server1.netology: Checking if box 'bento/ubuntu-20.04' version '202206.03.0' is up to date...
==> server1.netology: Setting the name of the VM: server1.netology
==> server1.netology: Clearing any previously set network interfaces...
==> server1.netology: Preparing network interfaces based on configuration...
    server1.netology: Adapter 1: nat
    server1.netology: Adapter 2: hostonly
==> server1.netology: Forwarding ports...
    server1.netology: 22 (guest) => 20011 (host) (adapter 1)
    server1.netology: 22 (guest) => 2222 (host) (adapter 1)
==> server1.netology: Running 'pre-boot' VM customizations...
==> server1.netology: Booting VM...
==> server1.netology: Waiting for machine to boot. This may take a few minutes...
    server1.netology: SSH address: 127.0.0.1:2222
    server1.netology: SSH username: vagrant
    server1.netology: SSH auth method: private key
    server1.netology:
    server1.netology: Vagrant insecure key detected. Vagrant will automatically replace
    server1.netology: this with a newly generated keypair for better security.
    server1.netology:
    server1.netology: Inserting generated public key within guest...
    server1.netology: Removing insecure key from the guest if it's present...
    server1.netology: Key inserted! Disconnecting and reconnecting using new SSH key...
==> server1.netology: Machine booted and ready!
==> server1.netology: Checking for guest additions in VM...
    server1.netology: The guest additions on this VM do not match the installed version of
    server1.netology: VirtualBox! In most cases this is fine, but in rare cases it can
    server1.netology: prevent things such as shared folders from working properly. If you see
    server1.netology: shared folder errors, please make sure the guest additions within the
    server1.netology: virtual machine match the version of VirtualBox you have installed on
    server1.netology: your host and reload your VM.
    server1.netology:
    server1.netology: Guest Additions Version: 6.1.34
    server1.netology: VirtualBox Version: 5.2
==> server1.netology: Setting hostname...
==> server1.netology: Configuring and enabling network interfaces...
==> server1.netology: Mounting shared folders...
    server1.netology: /vagrant => /root/virt-homeworks-virt-11/05-virt-02-iaac/src/vagrant
==> server1.netology: Running provisioner: ansible...
    server1.netology: Running ansible-playbook...

PLAY [nodes] *******************************************************************

TASK [Gathering Facts] *********************************************************
 [WARNING]: Module invocation had junk after the JSON data:
AttributeError("module 'platform' has no attribute 'dist'")

ok: [server1.netology]

TASK [Create directory for ssh-keys] *******************************************
ok: [server1.netology]

TASK [Adding rsa-key in /root/.ssh/authorized_keys] ****************************
changed: [server1.netology]

TASK [Checking DNS] ************************************************************
changed: [server1.netology]

TASK [Installing tools] ********************************************************
ok: [server1.netology] => (item=[u'git', u'curl'])

TASK [Installing docker] *******************************************************
changed: [server1.netology]

TASK [Add the current user to docker group] ************************************
changed: [server1.netology]

PLAY RECAP *********************************************************************
server1.netology           : ok=7    changed=4    unreachable=0    failed=0
```

- Зайти внутрь ВМ, убедиться, что Docker установлен с помощью команды
```
docker ps
```

```log
vagrant ssh
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-110-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Fri 23 Sep 2022 03:08:52 PM UTC

  System load:  0.83               Users logged in:          0
  Usage of /:   13.0% of 30.63GB   IPv4 address for docker0: 172.17.0.1
  Memory usage: 23%                IPv4 address for eth0:    10.0.2.15
  Swap usage:   0%                 IPv4 address for eth1:    192.168.192.11
  Processes:    115


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Fri Sep 23 15:08:34 2022 from 10.0.2.2
vagrant@server1:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
