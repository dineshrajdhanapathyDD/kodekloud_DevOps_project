

Questions:
During the weekly meeting, the Nautilus DevOps team discussed about the automation and configuration management solutions that they want to implement. While considering several options, the team has decided to go with (Ansible0 for now due to its simple setup and minimal pre-requisites. The team wanted to start testing using Ansible, so they have decided to use (jump host) as an Ansible controller to test different kind of tasks on rest of the servers.


Install (ansible) version (4.10.0) on (Jump host) using (pip3) only. Make sure Ansible binary is available globally on this system, i.e all users on this system are able to run Ansible commands.


Solution:  
1. Switch to Root user on jump server

thor@jump_host ~$ sudo su -

[sudo] password for thor: mjolnir123
[root@jump_host ~]# cd /etc/yum.repos.d/
[root@jump_host yum.repos.d]# ll
-bash: ll: command not found
[root@jump_host yum.repos.d]# ls
CentOS-Stream-AppStream.repo         CentOS-Stream-NFV.repo               epel-testing-modular.repo
CentOS-Stream-BaseOS.repo            CentOS-Stream-PowerTools.repo        epel-testing.repo
CentOS-Stream-Debuginfo.repo         CentOS-Stream-RealTime.repo          epel.repo
CentOS-Stream-Extras-common.repo     CentOS-Stream-ResilientStorage.repo  redhat.repo
CentOS-Stream-Extras.repo            CentOS-Stream-Sources.repo           ubi.repo
CentOS-Stream-HighAvailability.repo  epel-modular.repo
CentOS-Stream-Media.repo             epel-playground.repo


2. To Install specific ansible version, required ansible repo

[root@jump_host yum.repos.d]# vi ansible.repo
[root@jump_host yum.repos.d]# cat ansible.repo
[ansible]

name=Ansible RPM repository for Enterprise Linux 7 - $basearch

baseurl=https://releases.ansible.com/ansible/rpm/release/epel-7-$basearch/

enabled=1

gpgcheck=1

gpgkey=https://releases.ansible.com/keys/RPM-GPG-KEY-ansible-release.pub


 3. To Install Ansible run below commands using pip3

([root@jump_host yum.repos.d]# pip3 install ansible 4.10.0)-optional

[root@jump_host yum.repos.d]# pip3 install ansible
Collecting ansible
  Using cached ansible-4.10.0.tar.gz (36.8 MB)
  Preparing metadata (setup.py) ... done
Collecting ansible-core~=2.11.7
  Downloading ansible-core-2.11.12.tar.gz (7.1 MB)
     |████████████████████████████████| 7.1 MB 4.4 MB/s            
  Preparing metadata (setup.py) ... done
Collecting jinja2
  Downloading Jinja2-3.0.3-py3-none-any.whl (133 kB)
     |████████████████████████████████| 133 kB 49.1 MB/s            
Requirement already satisfied: PyYAML in /usr/lib64/python3.6/site-packages (from ansible-core~=2.11.7->ansible) (3.12)
Collecting cryptography
  Downloading cryptography-40.0.2-cp36-abi3-manylinux_2_28_x86_64.whl (3.7 MB)
     |████████████████████████████████| 3.7 MB 67.7 MB/s            
Collecting packaging
  Downloading packaging-21.3-py3-none-any.whl (40 kB)
     |████████████████████████████████| 40 kB 7.6 MB/s             
Collecting resolvelib<0.6.0,>=0.5.3
  Downloading resolvelib-0.5.4-py2.py3-none-any.whl (12 kB)
Collecting cffi>=1.12
  Downloading cffi-1.15.1-cp36-cp36m-manylinux_2_5_x86_64.manylinux1_x86_64.whl (402 kB)
     |████████████████████████████████| 402 kB 48.6 MB/s            
Collecting MarkupSafe>=2.0
  Downloading MarkupSafe-2.0.1-cp36-cp36m-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_12_x86_64.manylinux2010_x86_64.whl (30 kB)
Collecting pyparsing!=3.0.5,>=2.0.2
  Downloading pyparsing-3.1.1-py3-none-any.whl (103 kB)
     |████████████████████████████████| 103 kB 76.9 MB/s            
Collecting pycparser
  Downloading pycparser-2.21-py2.py3-none-any.whl (118 kB)
     |████████████████████████████████| 118 kB 84.3 MB/s            
Using legacy 'setup.py install' for ansible, since package 'wheel' is not installed.
Using legacy 'setup.py install' for ansible-core, since package 'wheel' is not installed.
Installing collected packages: pycparser, pyparsing, MarkupSafe, cffi, resolvelib, packaging, jinja2, cryptography, ansible-core, ansible
    Running setup.py install for ansible-core ... done
    Running setup.py install for ansible ... done
Successfully installed MarkupSafe-2.0.1 ansible-4.10.0 ansible-core-2.11.12 cffi-1.15.1 cryptography-40.0.2 jinja2-3.0.3 packaging-21.3 pycparser-2.21 pyparsing-3.1.1 resolvelib-0.5.4
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv


4. Validate the ansible version. 

[root@jump_host yum.repos.d]# ansible --version
[DEPRECATION WARNING]: Ansible will require Python 3.8 or newer on the controller starting with Ansible 2.12. Current 
version: 3.6.8 (default, Jun 26 2023, 20:26:30) [GCC 8.5.0 20210514 (Red Hat 8.5.0-20)]. This feature will be removed from 
ansible-core in version 2.12. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
/usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography. The next release of cryptography will remove support for Python 3.6.
  from cryptography.exceptions import InvalidSignature
ansible [core 2.11.12] 
  config file = None
  configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/local/lib/python3.6/site-packages/ansible
  ansible collection location = /root/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/local/bin/ansible
  python version = 3.6.8 (default, Jun 26 2023, 20:26:30) [GCC 8.5.0 20210514 (Red Hat 8.5.0-20)]
  jinja version = 3.0.3
  libyaml = True


5. Click on Finish & Confirm to complete the task successful
















error found : check the install method of ansible


[root@jump_host yum.repos.d]# yum install ansible 4.10.0
Failed loading plugin "subscription-manager": cannot import name 'which'
Failed loading plugin "upload-profile": cannot import name 'which'
Failed loading plugin "product-id": cannot import name 'which'
  
No match for argument: 4.10.0
Error: Unable to find a match: 4.10.0


